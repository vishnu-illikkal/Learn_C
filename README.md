# Foundations of C Programming (Learn_C)

An interactive, AI-mentored C programming course book and workspace designed to guide engineering students from core fundamentals to advanced systems and Embedded C foundations.

---

## 📖 Overview & Course Philosophy

This course is built for students who want to master standard C (ISO C99/C11) and low-level memory mechanics before transitioning to embedded systems, microcontroller firmware, and systems engineering.

### How Learning Works in Antigravity

1. **Adaptive Onboarding:** On first launch, the AI instructor asks for your name, programming background, and goals to personalize your pace and your LaTeX notes book.
2. **One Bite-Sized Concept at a Time:** Every topic is taught individually with clear mental models, memory diagrams, and runnable code examples.
3. **The Check-In Rule:** The instructor never rushes or jumps ahead. After each explanation, it stops to check if you have questions.
4. **Interactive Doubt Resolution:** If you get stuck on pointers, typecasting, or memory layout, the instructor breaks it down visually.
5. **Notes on Request:** LaTeX study notes in `notes/chapters/` are taken and updated strictly when you ask for them.

---

## 🚀 Getting Started with Google Antigravity

### 1. Open the Repository in Antigravity IDE

1. Clone or open the `Learn_C` folder in **Google Antigravity IDE**:
   ```bash
   git clone https://github.com/vishnu-illikkal/Learn_C.git
   ```
2. Start chatting with the AI instructor in the Antigravity conversation panel.
3. Introduce yourself or say **"Hi, I'm ready to learn C"** to begin your personalized course.

### 2. Useful Slash Commands & Workflows

Type these commands directly in the Antigravity prompt to trigger specialized tasks:

| Command | Description |
| :--- | :--- |
| `/build-pdf-from-latex` | Compiles your LaTeX notes in `notes/` into `notes/main.pdf` via Docker. |
| `/take-latex-notes` | Summarizes and records the current lesson/doubt into the chapter `.tex` file. |
| `/commit` | Safely compiles the PDF, stages files, generates a commit message, and tags the release. |
| `/push` | Pushes all commits and release tags to your GitHub repository. |
| `/fresher-audit` | Audits a chapter for jargon clarity, tone, and beginner friendliness. |

---

## 🐳 Docker & LaTeX PDF Engine Setup

You do **not** need to install a heavy local TeX Live distribution (~6–8 GB) on your machine. All PDF compilations run inside an isolated, official Docker container (`texlive/texlive`).

### Step 1: Install & Start Docker Desktop

1. Download and install **[Docker Desktop](https://www.docker.com/products/docker-desktop/)** for Windows / macOS / Linux.
2. Ensure Docker Desktop is running (the whale icon in your system tray should show "Engine running").

### Step 2: Pull the LaTeX Compiler Image (One-Time Setup)

Open your terminal (PowerShell, Command Prompt, or Bash) and run:

```bash
docker pull texlive/texlive:latest
```

*(This downloads the complete TeX Live container with all required fonts, TikZ packages, and tcolorbox libraries).*

### Step 3: Compiling Your Notes to PDF

You can build the PDF in either of two ways:

#### Option A: Inside Antigravity (Recommended)
Simply type in the chat:
```text
/build-pdf-from-latex
```

#### Option B: From the Terminal (PowerShell)
Run this command from the repository root:

```powershell
docker rm -f latex-compiler 2>$null; docker run --name latex-compiler --rm -v "${PWD}/notes:/workdir" -w /workdir texlive/texlive pdflatex -interaction=nonstopmode main.tex
```

Your compiled book will be generated at:
```text
notes/main.pdf
```

---

## 📂 Repository Structure

```text
Learn_C/
├── .agents/                  # AI instructor configuration, rules, skills & workflows
│   ├── AGENTS.md             # Persona, pedagogical rules & course scope
│   ├── rules/                # C best practices and safety guidelines
│   ├── skills/               # Automated LaTeX formatting & PDF build skills
│   └── workflows/            # Slash commands (/commit, /push, /build-pdf-from-latex)
├── notes/                    # LaTeX source files for the course book
│   ├── chapters/             # Modular chapter files (chapter01.tex, etc.)
│   ├── main.tex              # Document root & preamble
│   ├── macros.tex            # Custom color schemes, doubtboxes, and TikZ styles
│   └── main.pdf              # Generated publication-quality PDF notes
├── code/                     # Practice C code snippets and examples
└── README.md                 # Project documentation & setup guide
```

---

## 🛠️ Recommended Local Tools for C Coding

While reading notes and interacting with Antigravity, you can compile and run C practice code using:

- **GCC Compiler (MinGW-w64 on Windows or native GCC on Linux/macOS):**
  ```bash
  gcc -Wall -Wextra -std=c99 main.c -o main && ./main
  ```
- **VS Code C/C++ Extension Pack** for syntax highlighting and debugging.
