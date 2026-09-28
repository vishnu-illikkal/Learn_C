# Foundations of C Programming (Learn_C)

An interactive, AI-mentored C programming course book and workspace designed to guide engineering students from core fundamentals to advanced systems and Embedded C foundations.

---

## 📖 Overview & Course Philosophy

This course is built for students who want to master standard C (ISO C99/C11) and low-level memory mechanics before transitioning to embedded systems, microcontroller firmware, and systems engineering.

### Why This Approach Beats Video Courses (Udemy, YouTube, etc.)

Traditional pre-recorded video courses often leave students feeling stuck, disengaged, or overwhelmed. Here is why active AI pair-learning in Antigravity transforms your learning experience:

| Feature | Traditional Video Courses (Udemy / YouTube) | Learn_C in Antigravity |
| :--- | :--- | :--- |
| **Learning Pace** | Fixed playlist speed. Skipping causes gaps; rewinding is tedious. | **100% Adaptive to You:** Slow down on tricky pointer math or breeze past concepts you already know. |
| **Doubt Resolution** | Post on dead Q&A forums or search StackOverflow for hours. | **Instant 1-on-1 Mentorship:** Ask any question, at any depth, as many times as you need. |
| **Learning Style** | **Passive Watching:** Gives a false sense of understanding until you write code alone. | **Active Pair-Programming:** You code, break, debug, and understand memory mechanically in real time. |
| **Personalization** | Generic lectures recorded for a broad audience. | **Personalized Feedback:** Explanations adapt to your background and specific embedded systems goals. |
| **Course Notes & Deliverables** | Messy screenshots, lost bookmarks, or static PDF slides. | **Lifelong LaTeX & PDF Book:** You build your own publication-grade textbook (`notes/main.pdf`) for career reference. |

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

### 2. Mastering Antigravity: The `@` and `/` Shortcuts

If you are new to Google Antigravity, two fundamental features power your interaction with the AI instructor:

#### A. The `@` Symbol (Context & File Mentioning)
Typing `@` in the chat input opens an interactive autocomplete picker. You do not need to type full directory paths—just typing `@` followed by any part of the filename (e.g., `@chapter01.tex` or `@main.c`) is enough. The autocomplete popup will instantly filter and display the matching files for you to select.

- **When to use it:**
  - Asking doubts about your code: *"Can you explain what line 12 in `@main.c` is doing?"*
  - Reviewing notes: *"Can you check if `@chapter01.tex` explains stack frames clearly?"*
  - Debugging errors: *"I'm getting a compiler error in `@main.c`, here is the terminal output..."*

#### B. The `/` Symbol (Slash Commands & Automated Workflows)
Typing `/` in the chat brings up automated workflows that run complex tasks (such as building the PDF, updating LaTeX notes, or committing changes to Git) in a single command.

- **Available Slash Commands:**

| Command | Description | Everyday Usage Scenario |
| :--- | :--- | :--- |
| `/take-latex-notes` | Formats and saves the current topic or doubt resolution into the active chapter `.tex` file. | Type `/take-latex-notes` whenever you finish discussing a concept and want it recorded in your book. |
| `/build-pdf-from-latex` | Compiles your LaTeX files in `notes/` into `notes/main.pdf` using the Docker container. | Type `/build-pdf-from-latex` when you want to generate or view your updated study book PDF. |
| `/commit` | Rebuilds the PDF, stages changes, generates a clean commit message, and tags the version. | Type `/commit` when you finish a study session to save your progress locally. |
| `/push` | Pushes all local commits and version release tags to your remote GitHub repository. | Type `/push` after `/commit` to sync everything to your remote GitHub repository. |
| `/fresher-audit` | Analyzes a chapter `.tex` file for beginner-friendliness, jargon clarity, and tone. | Type `/fresher-audit` if you want to verify that a chapter's explanations are intuitive. |
| `/abbreviations` | Audits a chapter for technical abbreviations and organizes them into a terminology box. | Type `/abbreviations` to compile acronym definitions for the chapter. |

### 3. Full Customization: Editing Skills, Rules & Workflows

Everything in this repository is designed to be fully transparent and customizable:

- **Feel Free to Edit Anything:** You have full ownership of this project. If you spot a flaw, want to tweak the teaching style, or want to customize how LaTeX notes are formatted, you can directly edit any file in `.agents/` or `notes/`.
- **Modifying Agent Behavior:**
  - Edit [`.agents/AGENTS.md`](file:///.agents/AGENTS.md) to adjust the instructor's persona, course scope, or check-in rules.
  - Edit [`.agents/rules/c-best-practices.md`](file:///.agents/rules/c-best-practices.md) to add your own coding standards.
- **Customizing Skills & Workflows:**
  - Edit files in [`.agents/skills/`](file:///.agents/skills/) (e.g., `latex-formatter`, `take-latex-notes`) to change LaTeX styling, box environments, or listing formats.
  - Edit or create markdown files in [`.agents/workflows/`](file:///.agents/workflows/) to add new slash commands tailored to your learning workflow.
- **Instant Hot-Reload:** Antigravity automatically detects your changes to the `.agents/` folder immediately—no restart required!

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
