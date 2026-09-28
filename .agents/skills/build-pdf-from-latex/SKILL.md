---
name: build-pdf-from-latex
description: Compiles LaTeX study notes into notes/main.pdf using Docker texlive/texlive, verifies zero errors/warnings, resolves cross-references, and recovers from corrupted cache/aux files.
---

# Build PDF from LaTeX Skill

This skill compiles the LaTeX book source (`notes/main.tex` and included chapters in `notes/chapters/`) into the final publication PDF (`notes/main.pdf`).

> [!IMPORTANT]
> **DECOUPLED EXECUTION:**
> This skill is intentionally decoupled from note-taking. It must run **ONLY** when explicitly requested by the user (via `/build-pdf-from-latex`, `/build-pdf`, "compile pdf", "build pdf", etc.) or during the commit workflow.

---

## 1. Trigger Conditions

Activate this skill whenever the user explicitly requests:
- `/build-pdf-from-latex` or `/build-pdf`
- "build pdf", "compile pdf", "compile notes", "generate pdf"
- "rebuild main.pdf", "run pdflatex"

---

## 2. Environment & Prerequisites

1. **Docker Execution Only:**
   - LaTeX compilation is powered entirely by the official `texlive/texlive` Docker container. No local TeX installation is required on the host system.
2. **Strictly Single-Threaded / Zero Parallel Compilations (CRITICAL):**
   - **NEVER** launch concurrent or overlapping compilation runs.
   - Always run the pre-flight check below before starting a build.
3. **Wait for Docker Process Completion with \texttt{docker wait} (Zero Inference Waste):**
   - **DO NOT guess, poll in loops, or assume arbitrary timeouts** (e.g., 5s, 10s). When compiling extensive multi-chapter documents (e.g., 400+ pages), a single pass takes 30–60+ seconds.
   - Use deterministic commands like `docker wait` so the shell/agent blocks cleanly until the container exits, eliminating token waste and inference retry loops.
   - Prematurely interrupting, polling in tight loops, or firing secondary commands before Docker finishes causes file lock contention on `main.pdf` and corrupts `main.aux` / `main.out` with null bytes (`^@^@^@`).
4. **User-Controlled Inclusion State in `notes/main.tex`:**
   - **NEVER** automatically comment out or uncomment chapter lines in `notes/main.tex`. Which chapters are included is strictly under the user's manual control.

---

## 3. Concrete Compilation Command

### Standard Single Build Command (Recommended)

This single command handles everything: it removes any existing container named `latex-compiler` (preventing name collisions if a previous run was aborted) and immediately compiles `notes/main.tex`:

```powershell
docker rm -f latex-compiler 2>$null; docker run --name latex-compiler --rm -v "${PWD}/notes:/workdir" -w /workdir texlive/texlive pdflatex -interaction=nonstopmode main.tex
```

> [!TIP]
> This single chained command is the **only command needed** for normal builds. It preserves warm `.aux` and `.out` cache files in `notes/` and compiles cleanly in one shot.

---

## 4. Known Error Diagnostics & Automatic Recovery

### A. Corrupted `.aux` or `.out` Files (Null Bytes / Runaway Arguments)
- **Symptoms:**
  - `! Text line contains an invalid character. l.1 ...@^^@^^@^^@...`
  - `Runaway argument? ... ! File ended while scanning use of \@@BOOKMARK.`
- **Root Cause:** A prior build was interrupted, or file lock collision wrote incomplete binary data into intermediate cache files.
- **Recovery Action:** Delete the corrupted cache files and rerun compilation immediately:
  ```powershell
  powershell -Command "Remove-Item -Path 'notes/main.aux', 'notes/main.out' -Force -ErrorAction SilentlyContinue"
  ```
  Then rerun the `pdflatex` command.

### B. Unresolved References or Changed Labels
- **Symptom:**
  - `LaTeX Warning: Label(s) may have changed. Rerun to get cross-references right.`
  - `LaTeX Warning: There were undefined references.`
- **Action:** Run a second pass of `pdflatex` so all dynamic cross-references (`\chapref`, `\secref`) and table of contents bookmarks resolve cleanly.

### C. Monospace Bold Font Warning
- **Symptom:**
  - `LaTeX Font Warning: Font shape 'T1/zi4/b/n' undefined using 'T1/zi4/m/n' instead`
- **Cause:** Using `\textbf{\texttt{...}}` (nested bold and code). Inconsolata does not support bold monospace. Fix in the respective chapter by using either `\textbf{...}` or `\texttt{...}` independently.

---

## 5. Verification Checklist

After compilation:
- [ ] Confirm command exited with exit code `0`.
- [ ] Confirm output line: `Output written on main.pdf (X pages, Y bytes)`.
- [ ] Report the compilation status, total page count, and PDF file path to the user.
