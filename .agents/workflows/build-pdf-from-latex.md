---
description: Compiles the LaTeX book source into notes/main.pdf using Docker texlive/texlive.
---

# Build PDF from LaTeX Workflow

// turbo-all

> [!IMPORTANT]
> **NO PLANNING REQUIRED**: This workflow is considered **trivially simple**. Proceed directly to execution without creating an Implementation Plan.

This workflow compiles `notes/main.tex` and included chapters into `notes/main.pdf` using the `build-pdf-from-latex` skill.

## Usage

- `/build-pdf-from-latex`: Compiles `notes/main.pdf` and verifies 0 errors.

## Execution Steps

1. **Execute Single Build Command (Cleanup & Fast Compile)**:
   Run the unified single-line build command:
   ```powershell
   docker rm -f latex-compiler 2>$null; docker run --name latex-compiler --rm -v "${PWD}/notes:/workdir" -w /workdir texlive/texlive pdflatex -interaction=nonstopmode main.tex
   ```
   *(This single command safely removes any previously stuck container and performs the compilation in one step.)*

2. **Second Pass (If References Changed)**:
   If compiler outputs `Label(s) may have changed. Rerun to get cross-references right`, rerun the build command above to resolve references.

3. **Handle Stale Cache (Only If Corrupted)**:
   If `main.aux` or `main.out` contains null bytes (`@^^@^^@`) or runaway bookmark errors:
   ```powershell
   powershell -Command "Remove-Item -Path 'notes/main.aux', 'notes/main.out' -Force -ErrorAction SilentlyContinue"
   docker rm -f latex-compiler 2>$null; docker run --name latex-compiler --rm -v "${PWD}/notes:/workdir" -w /workdir texlive/texlive pdflatex -interaction=nonstopmode main.tex
   ```

4. **Report Status**: Report exit status, page count, and path `notes/main.pdf`.

