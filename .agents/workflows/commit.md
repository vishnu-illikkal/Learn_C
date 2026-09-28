---
description: Stage all changes (or specified files), generate a commit message, and commit with a tag.
---

# Commit Workflow

// turbo-all

This workflow handles committing your changes safely, ensuring no concurrent LaTeX compilations collide or corrupt auxiliary files when compiling extensive multi-chapter documents.

## Usage

- `/commit`: Stages all modified and new files.
- `/commit <file1> <file2> ...`: Stages only the specified files.
- `/commit all`: Stages all modified and new files.

## Steps

1. **Pre-Build Concurrency Gate (CRITICAL: Zero Parallel Compilations)**:
   - When compiling many chapters (e.g., Chapters 01 through 37, 400+ pages), LaTeX compilation can take 30--90 seconds.
   - **Check for Running Compilations:** Inspect if a Docker container or background task running `texlive/texlive`, `pdflatex`, or `latexmk` is active:
     ```powershell
     docker ps --filter "ancestor=texlive/texlive" --format "{{.ID}} {{.Status}}"
     ```
   - **Rule:** If a build is currently running, **NEVER launch another compilation**. Wait for the active process/task to finish completely. Parallel builds will collide on `notes/main.aux`, `notes/main.pdf`, and `main.synctex(busy)`, causing null-byte corruption (`^@^@^@`) and fatal build aborts.
   - **Auxiliary Cleanup on Crash:** If an earlier build was abruptly interrupted and left corrupted auxiliary files, clean them before starting a fresh run:
     ```powershell
     Remove-Item -Force notes/main.aux, notes/main.out, notes/main.fls, notes/main.fdb_latexmk -ErrorAction SilentlyContinue
     ```

2. **Update Version (Alpha/Prerelease)**:
   - Identify the most recent version tag:
     ```powershell
     git tag --sort=-v:refname | Select-Object -First 5
     ```
   - Determine `<next_version>` and write version files with explicit UTF-8 encoding:
     ```powershell
     Set-Content -Path version.tex -Value "\newcommand{\gitversion}{<next_version>}" -Encoding utf8
     Set-Content -Path notes/version.tex -Value "\newcommand{\gitversion}{<next_version>}" -Encoding utf8
     ```

3. **Rebuild PDF (If Notes Changed) & Wait to Completion**:
   - If `main.tex`, `version.tex`, or any chapter file was modified, compile the document through Docker and **wait for completion**:
     ```powershell
     docker run --rm -v "${PWD}/notes:/workdir" -w /workdir texlive/texlive pdflatex -interaction=nonstopmode main.tex
     ```
   - Verify the command exits with code 0 before proceeding to staging.

4. **Stage Changes**:
   - Stage specifying files or use `.` for all:
     ```bash
     git add <files_or_dot>
     ```

5. **Analyze Changes**:
   - Inspect staged changes to verify context:
     ```bash
     git status
     git diff --cached --stat
     ```

6. **Generate & Commit**:
   - Generate a professional message following [Conventional Commits](https://www.conventionalcommits.org/) and execute the commit:
     ```bash
     git commit -m "<ai_generated_message>"
     ```

7. **Tag the Release**:
   - Tag the current commit with the same `<next_version>`:
     ```bash
     git tag <next_version>
     ```
