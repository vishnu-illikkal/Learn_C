---
name: take-latex-notes
description: Takes or updates LaTeX study notes (including topics, code examples, user questions, code provided in doubts, error outputs, and doubt resolutions) in notes/main.tex and notes/chapters/ when requested by the user.
---

# LaTeX Note-Taking Skill for C Learning

> [!CRITICAL]
> **ZERO-TOLERANCE QUALITY GATES:**
> Every rule in this skill is a **MANDATORY REQUIREMENT**, not an optional preference. Violating inline code rules, syntax rules, or checklist items constitutes a **FAILED note execution**. You MUST perform an explicit checklist audit before writing or saving any `.tex` file.

## Trigger Conditions

Activate this skill whenever the user explicitly asks to take notes, add notes, record a topic in LaTeX, or update the study document (e.g., "take notes", "add this to notes", "note this down", "/take-latex-notes").

## Responsibilities & Workflow

1. **Explicit Request Check**:
   - Confirm the user has requested notes. Do NOT generate or modify notes automatically.

2. **Pre-Execution Quality Gate (MANDATORY)**:
   - Before invoking `write_to_file` or `replace_file_content` on any `.tex` file, you MUST explicitly verify that your proposed changes satisfy all rules in Section 4 and Section 5 of this skill.

3. **Locate Target Chapter & Update `notes/main.tex` (User-Controlled Inclusion)**:
   - Check `notes/main.tex` and existing files in `notes/chapters/`.
   - Default to `notes/chapters/chapter01.tex` for introductory concepts (or create a new chapter file `notes/chapters/chapterXX.tex` if moving to a new chapter/topic).
   - **Strict Rule on `notes/main.tex`:**
     - **NEVER automatically comment out, uncomment, or modify existing chapter lines.** Commenting and inclusion state is strictly under the user's manual control.
     - When a new chapter `notes/chapters/chapterXX.tex` is created, simply append `\input{chapters/chapterXX.tex}` in its proper numerical sequence if not already present, without altering any other lines.

4. **Format Content & Heading Hierarchy Alignment**:
   - **Always Match Existing Chapter Hierarchy:** Before writing headings, inspect the chapter's existing macro-structure:
     - **Major Standalone Topics / Core Concepts:** Use `\section{Topic Title}` (e.g., `\section{Understanding Pointer Variables and Memory Addresses}`, `\section{Bitwise Manipulation with Masks}`). Never subordinate a new major standalone topic as a `\subsection` under a preceding sibling section.
     - **Sub-Concepts & Mechanics:** Use `\subsection{Subtopic Title}` (e.g., `\subsection{Direct Value vs. Pointer Dereferencing}`).
     - **Granular Breakdowns:** Use `\subsubsection{Granular Topic Title}`.
   - **Simple, Direct Section & Heading Names (Zero Cognitive Effort)**:
     - Headings must require **zero cognitive effort** for the student/reader to instantly understand what the section covers.
     - Always use plain, everyday, self-explanatory titles (e.g., `\section{Pass-by-Value vs. Pass-by-Reference in C}` or `\subsection{Summary of Fixed-Width Integer Types}`).
     - **STRICTLY FORBIDDEN:** Never use abstract, fancy, metaphoric, or academic phrasing in headings. Explicitly avoid patterns like *"The Spectrum of..."*, *"Taxonomy of..."*, *"Anatomy of..."*, *"Exegesis of..."*, *"Preamble to..."*, *"Disquisition on..."*.
   - **Zero Cognitive Load Paragraphs & Sentences**:
     - Write explanations in plain, direct, everyday English so the reader needs **zero brain power** to parse what a sentence means.
     - State *what a concept or construct does* directly, accompanied by simple memory mental models instead of overly formal academic jargon.
     - **Example (Good):** *"A pointer is a variable that stores the memory address of another variable rather than storing a direct value."*
     - **Example (Bad):** *"A pointer constitutes an indirect referencing mechanism encapsulating machine-level indirection across virtual address topologies."*
   - **Low Cognitive Load Code Examples (Practical & Intuitive Over Academic Abstractions)**:
     - **STRICTLY FORBIDDEN:** NEVER use abstract, academic, or confusing dummy utility functions (such as `Foo(x)`, `Bar(x)`, `DoMath()`, or overly convoluted mathematical recursions) in code examples.
     - **MANDATORY:** Always use **real-world, clear scenarios** (such as `calculate_average`, `swap_values`, `set_bit`, `read_sensor_buffer`, `student_record_t`) where the variable names and context immediately communicate intent without mental friction.
     - **Example (Bad):** `int func_xyz(int a, int b, int *c)` (cryptic names and zero context).
     - **Example (Good):** `void swap_values(int32_t *a, int32_t *b)` (practical, intuitive scenario).
   - **Baseline-First Code Examples (Unadorned Primitive First)**:
     - Whenever introducing a NEW language feature or syntax (e.g., Pointers, Structures, Function Pointers, Dynamic Allocation), **NEVER** present it combined *only* with complex secondary features in the initial explanation.
     - **Always show the unadorned baseline form first** before introducing nested pointers, complex structs, or multi-dimensional arrays.
     - **Example (Bad):** Introducing pointers *only* through pointer-to-pointer arrays or function pointer tables.
     - **Example (Good):** Showing standard scalar pointer dereference `int32_t *ptr = &val;` FIRST, then progressing to pointer arithmetic and arrays.
   - **Code & Diagram Preservation Rule (Editorial Curation & Completeness)**:
     - **Capture Clarified & Finalized Code:** When code examples, struct definitions, pointer mechanics, or memory diagrams are explored and clarified in chat, ensure the **refined, authoritative, and finalized versions** are preserved in dedicated `\begin{lstlisting}[language=C]` blocks. Do not record raw broken exploratory drafts; capture the clean, finalized solution that emerged from the clarification.
     - **Visual Memory Model Diagram Preservation:** Whenever a memory layout, stack frame, or pointer indirection is explained using an effective visual mental model, translate it into a clean TikZ diagram (`\begin{tikzpicture}...\end{tikzpicture}`).
     - **Do Not Degrade Critical Code into Pure Prose:** When a concept fundamentally relies on concrete C syntax (such as pointer arithmetic or struct padding), do not strip away the code in favor of abstract text summaries.
     - **Zero Negative-Constraint Leakage Across Turns:** If the user previously requested *"no code"* or *"brief"* in an earlier turn, that constraint applies **STRICTLY to that single turn**. Subsequent note-taking turns must resume full, curated documentation with all relevant code blocks and diagrams included.
   - **TikZ Memory & Architecture Diagram Standards (Zero Text Collision)**:
     - **Default to Clear Vertical or Horizontal Memory Grids:**
       - When designing memory layout diagrams, stack/heap frames, or pointer boxes, use distinct styles and consistent node spacing ($\Delta y \ge 1.8\text{cm}$ or $\Delta x \ge 3.0\text{cm}$).
     - **Standard TikZ Theme Palette:**
       ```latex
       \begin{tikzpicture}[
           memcell/.style={draw=primary, fill=primary!8, rounded corners=2pt, thick, align=center, font=\small, minimum width=4cm, minimum height=0.9cm},
           ptrbox/.style={draw=secondary, fill=secondary!8, rounded corners=2pt, thick, align=center, font=\small, minimum width=3cm, minimum height=0.9cm},
           valbox/.style={draw=codegreen, fill=codegreen!10, rounded corners=2pt, thick, align=center, font=\small, minimum width=3cm, minimum height=0.9cm},
           arrow/.style={->, >=stealth, thick, color=darktext}
       ]
       ```
   - Summarize key explanations and rules clearly.
   - Embed runnable C code snippets using listings:

     ```latex
     \begin{lstlisting}[language=C]
     #include <stdio.h>
     #include <stdint.h>

     int main(void) {
         int32_t number = 42;
         int32_t *ptr = &number;

         printf("Value: %d, Address: %p\n", *ptr, (void*)ptr);
         return 0;
     }
     \end{lstlisting}
     ```

   - **Integration of User Questions: Primary Course Content vs. \texttt{doubtbox} (STRICT RULE)**:
     - **NOT All User Questions Belong in a Doubtbox:** The user frequently asks questions to gain deeper clarity, explore under-the-hood mechanisms, or verify memory behavior (e.g., *"Why do we cast pointer to \texttt{(void*)} in \texttt{printf}?"*, *"Why does array name decay to pointer?"*, *"Why is size of struct larger than sum of its members?"*).
     - **Default Rule: Integrate Clarity & Mechanism Questions Directly into Main Body Content:**
       - All questions seeking clarity, deeper explanations, memory mechanics, or compiler rationale **MUST be incorporated directly into the regular instructional text** (as normal explanatory paragraphs, dedicated `\section` / `\subsection` topics, code examples, or comparison tables).
       - The book must read as a cohesive, professional textbook, NOT a fragmented chat transcript or an endless chain of callout boxes.
     - **Strictly Restricted \texttt{doubtbox} Usage:** A `\begin{doubtbox}{...}` is **ONLY** permitted for:
       1. **Error Diagnostics:** When the user provides a specific broken code snippet or compiler warning/error message that needs line-by-line debugging.
       2. **Counter-Intuitive Gotchas:** Classic beginner traps or subtle pitfalls that would interrupt the main narrative flow (e.g., *"Why does \texttt{scanf("\%d", val)} crash without \texttt{\&}?"*).
     - **Prohibition:** NEVER wrap general questions, clarifications, architectural deep dives, or progressive explanations into a `doubtbox`. Write them as standard book prose.
     - If an error diagnostic snippet is recorded in a doubtbox, format it cleanly:

     ```latex
     \begin{doubtbox}{Why did this code produce a segmentation fault?}
     \begin{lstlisting}[language=C]
     #include <stdio.h>

     int main(void) {
         int *ptr = NULL;
         *ptr = 10; // Dereferencing NULL pointer
         return 0;
     }
     \end{lstlisting}
     \textbf{Runtime Error:} \texttt{Segmentation fault (core dumped)} \par \vspace{0.3em}
     \textbf{Answer:} The pointer \texttt{ptr} points to address \texttt{0x0 (NULL)}, which is protected memory. Attempting to write to it causes an illegal memory access trap.
     \end{doubtbox}
     ```

   - **Program Execution Command & Output Rule (Two Separate Boxes)**:
     - **ALWAYS** present the compilation/execution command and the resulting terminal output in **two separate, distinct listing boxes**:

     ```latex
     \textbf{Compilation \& Execution Command:}

     \begin{lstlisting}[language=bash]
     gcc -Wall -Wextra main.c -o main && ./main
     \end{lstlisting}

     \textbf{Program Execution Output:}

     \begin{lstlisting}
     <terminal output here>
     \end{lstlisting}
     ```

   - **Table Creation & Formatting Standards (\texttt{tabularx} Best Practices)**:
     - **Full Width:** Always wrap tables in `\begin{center}` or `\begin{table}[H]\centering` and use `\begin{tabularx}{\linewidth}{|...|}`.
     - **Strict Column Selection Rules:**
       - **`l` or `c` Columns (STRICTLY RESTRICTED):** Allowed **ONLY** for short atomic values (maximum 10--12 characters) such as short keywords, types (`uint32_t`), or symbols (`\cmark`, `\xmark`).
       - **`X` Columns (MANDATORY for Content):** **ALWAYS** use `X` columns for descriptions, code expressions, questions, or multi-word explanations.
     - **CRITICAL MATHEMATICAL RULE: Proportional `\hsize` Sum MUST Strictly Equal Total Count of `X` Columns ($N$):**
       - In `tabularx`, if you assign custom `\hsize` multipliers (e.g., `>{\hsize=A\hsize...}X`), the sum of all coefficients $\sum A$ **MUST STRICTLY EQUAL the total number of `X` columns ($N$) in that table**.
       - Use `>{\raggedright\arraybackslash}X` on all `X` columns to prevent awkward word justification.

   - **Bi-Directional Chapter Cross-Referencing Standard (Upstream & Downstream Links)**:
     - **Dynamic References via Macros (Zero Hardcoding):**
       - **NEVER hardcode chapter/section numbers or titles in links.** C study notes use dynamic macros defined in `notes/macros.tex`:
         - `\chapref{chap:label}`: Dynamically expands to `\hyperref[chap:label]{\textbf{Chapter~\ref{chap:label} (\nameref{chap:label})}}`.
         - `\secref{sec:label}`: Dynamically expands to `\hyperref[sec:label]{\textbf{Section~\ref{sec:label} (\nameref{sec:label})}}`.
     - **Downstream Roadmap Section at Start of Foundational Chapters (`roadmapBox`):**
       - Always place the downstream roadmap inside a dedicated **`\section{Roadmap: Where This Concept is Used in Later Chapters}`** at the very beginning of the foundational chapter (immediately below `\chapter{...}` and `\label{...}`).

5. **Bi-Directional Cross-Referencing Synchronization (MANDATORY ACTIVE STEP)**:
   - Immediately after writing or updating notes in a chapter that references earlier chapters via `\chapref{chap:<target>}`:
   - **MANDATORY ACTION:** You MUST open each referenced chapter file `notes/chapters/<target>.tex`.
   - Inspect its `\section{Roadmap: Where This Concept is Used in Later Chapters}` and `\begin{roadmapBox}`.
   - If a back-link to the current chapter (`\item \chapref{chap:<current>}: ...`) is missing, you MUST immediately use `replace_file_content` to add it.

6. **Strict LaTeX Syntax Rules (ZERO Markdown Allowed in .tex Files)**:
   - **STRICT PROHIBITION (ZERO MARKDOWN ALLOWED):** Never write markdown syntax (`**bold**`, `*italic*`, `` `code` ``) inside `.tex` files under any circumstances. Always use native LaTeX macros (`\textbf{...}`, `\textit{...}`, `\texttt{...}`).
   - **NO `**` FOR BOLD:** Always use `\textbf{...}`.

   | Anti-Pattern / Prohibited Syntax | Correct LaTeX Syntax | Rationale |
   | :--- | :--- | :--- |
   | `**bold text**` | `\textbf{bold text}` | Markdown bold syntax does not compile in LaTeX; produces literal asterisks. |
   | `*italic text*` or `_italic text_` | `\textit{italic text}` | Markdown italics corrupt LaTeX paragraphs. |
   | `` `inline code` `` | `\texttt{inline code}` | Backtick code syntax breaks LaTeX typesetting. |
   | `\textbf{\texttt{code}}` | `\textbf{code}` or `\texttt{code}` | **NEVER nest `\textbf` and `\texttt` together.** Inconsolata does not support bold monospace. |
   | Inline struct/function code in `\texttt{...}` e.g., `\texttt{struct node \{ ... \}}` | Use dedicated `\begin{lstlisting}[language=C]` code blocks. | Inline `\texttt` with struct declarations or escaped braces (`\{ ... \}`) looks terrible in PDF output. |
   | `\begin{tabularx}{\linewidth}{\|l\|l\|l\|X\|}` with long text in `l` | `\begin{tabularx}{\linewidth}{\|>{\raggedright\arraybackslash}X\|l\|...>{\raggedright\arraybackslash}X\|}` | Prevents tables stretching beyond the right page margin. |
   | `\textit{... \texttt{code} ...}` | `\textit{...} \texttt{code} \textit{...}` or `\textup{\texttt{code}}` | Prevents `T1/zi4/m/it` undefined font shape warnings in Inconsolata. |
   | `\begin{lstlisting}[language=text]` or `[language=yaml]` | `\begin{lstlisting}` | Unsupported languages crash LaTeX's listings package. Use plain `\begin{lstlisting}` for configuration and terminal outputs. |
   | `\paragraph{Title:}\begin{lstlisting}` | `\vspace{0.5em}\textbf{Title:}\begin{lstlisting}` | `\paragraph` is a run-in heading that clashes with display-mode listing frames. |
   | Emojis (`✅`, `❌`) | `\cmark` and `\xmark` | Raw Unicode emojis cause character encoding compilation errors. |
   | Bare `_`, `%`, `&`, `#`, `$` | `\_`, `\%`, `\&`, `\#`, `\$` | Unescaped special characters break LaTeX compilation. |

7. **Pre-Save Verification Checklist**:
   Before updating or saving any `.tex` file:
   - [ ] **Zero Markdown Bold Check:** Confirm **NO** `**` exists anywhere in the file. All bold text must use `\textbf{...}`.
   - [ ] **Curated Code & Diagram Preservation Check:** Confirm that all finalized code patterns, struct definitions, pointer mechanics, and visual memory diagrams clarified in chat are cleanly captured using dedicated `lstlisting` and TikZ blocks.
   - [ ] **Low Cognitive Load Code Check:** Confirm code examples use practical, real-world scenarios (`calculate_sum`, `swap_values`, `student_record_t`) rather than cryptic abstract code (`foo`, `bar`).
   - [ ] **Baseline-First Check:** Confirm that new code examples present the minimal unadorned syntax first (e.g., basic pointer before double pointer or pointer arithmetic).
   - [ ] **No Nested `\textbf{\texttt{...}}` Macros:** Confirm **NO** `\textbf{\texttt{...}}` exists. Use either `\textbf{code}` or `\texttt{code}` independently.
   - [ ] **No Struct/Function Declarations in Inline `\texttt`:** Confirm **NO** struct/function declarations with escaped braces (`\{ ... \}`) exist in `\texttt{...}`. Always use `\begin{lstlisting}[language=C]` code blocks.
   - [ ] **Table Column Audit:** Inspect every `l` and `c` column in all tables. Confirm that **NO** cell contains sentences, questions, multi-word phrases, or code snippets longer than 12 characters.
   - [ ] **Tabularx \texttt{\textbackslash hsize} Multiplier Sum Audit (CRITICAL):** Count the exact number of `X` columns ($N$) in each table. Verify that the sum of all `\hsize` multipliers strictly equals $N$.
   - [ ] **No Run-In Paragraphs Before Listings (Heading Overlap Check):** Confirm **NO** `\paragraph{...}` directly precedes a `\begin{lstlisting}`.
   - [ ] **Listing Language Audit:** Confirm ONLY supported languages are used (`C`, `bash`, `sh`). Plain `\begin{lstlisting}` without a language tag for terminal outputs.
   - [ ] No `\texttt{...}` exists inside `\textit{...}` or `\emph{...}`.
   - [ ] No raw markdown syntax (`**`, `*`, `` ` ``) remains.
   - [ ] All `\begin{...}` and `\end{...}` pairs are balanced.
   - [ ] Special characters outside `lstlisting` blocks are escaped (`\%`, `\_`, `\&`, `\#`, `\$`).
   - [ ] Command and Output listings are separated into distinct blocks.
   - [ ] **Bi-Directional Cross-Referencing Sync Gate:** Confirm dynamic macros `\chapref{...}` / `\secref{...}` are used and back-links updated.

8. **Decoupled PDF Compilation (Do NOT Auto-Build PDF)**:
   - **MANDATORY DECOUPLING:** Taking LaTeX notes does **NOT** build or compile `notes/main.pdf`. Do **NOT** automatically invoke Docker compilation commands during `/take-latex-notes`.
   - Compiling the LaTeX notes into a PDF is strictly handled by the dedicated **`build-pdf-from-latex`** skill (or `/build-pdf-from-latex` workflow) when explicitly requested by the user.
