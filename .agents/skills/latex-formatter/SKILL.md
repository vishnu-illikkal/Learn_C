---
name: LaTeX Formatter
description: Enforces LaTeX formatting standards for the C programming course book.
---

# LaTeX Formatter Skill

This skill provides instructions for maintaining professional LaTeX formatting in the C Programming course book.

## Typography Rules

- **Quotes**: Always use straight quotes (`"`) for code and identifiers. Avoid curly quotes unless specifically required for typesetting.
- **Dashes**:
  - Use en-dashes (`--`) for numerical ranges (e.g., `1--10`).
  - Use em-dashes (`---`) for sentence breaks.
- **Spaces**: Use non-breaking spaces (`~`) between values and units (e.g., `4~bytes`, `32~bits`, `16~MHz`).
- **Font Shape Safety (No Nested `\texttt` in `\textit`)**: Never nest `\texttt{...}` directly inside `\textit{...}` or `\emph{...}`. The `inconsolata` (`zi4`) monospace font lacks an italic shape variant, triggering `LaTeX Font: Font shape 'T1/zi4/m/it' undefined` warnings. Split italic segments around monospace text (e.g., `\textit{text before} \texttt{code} \textit{text after}`) or use `\textup{\texttt{code}}`.

## Margin Overflow Prevention

Long technical terms wrapped in `\texttt{}` (like function names or type definitions) do not automatically break across lines, often causing margin overflows in the PDF.

- **Rule**: For `\texttt{...}` terms longer than 18 characters (such as `\texttt{calculate_moving_average()}`) or those causing margin issues (`Overfull \hbox` warnings), you must use `\linebreak` before the term.
- **Preferred Method**: Insert `\linebreak` before the term. This forces it to a new line *while keeping the previous line fully justified*.
- **Note**: This will intentionally cause an `Underfull \hbox (badness 10000)` warning. This is expected and acceptable; do not try to "fix" the underfull warning.
- **Avoid**: Do not use `\\`, `\break`, `\newline`, or `\allowbreak` unless absolutely necessary.
- **Prohibited in Code Blocks**: Never use `\linebreak` (or any other LaTeX command) inside a `\begin{lstlisting} ... \end{lstlisting}` environment. Verbatim environments treat these as literal text, not commands.

## Character Escaping

The following special characters *must* be escaped when they appear in plain text in LaTeX:

- `_` becomes `\_`
  - **Exception**: Underscores must **NOT** be escaped inside `\label{...}`, `\ref{...}`, `\cite{...}`, `\includegraphics{...}`, or inside `\begin{lstlisting} ... \end{lstlisting}` blocks.
  - **Compliance Check**: Custom macros like `\begin{terminalBox}`, `\begin{doubtbox}` are **NOT** verbatim environments. Any underscores, ampersands, or special characters inside these boxes **MUST** be escaped (e.g., `uint32\_t`).
- `%` becomes `\%`
- `&` becomes `\&`
- `$` becomes `\$`
- `#` becomes `\#` (Especially important for preprocessor directives in text like `\textbf{\#include}`)
- `{` becomes `\{`
- `}` becomes `\}`

## Formatting Requirements

- **Code Terms**: Any reference to data types, variable names, functions, or keywords must be wrapped in `\texttt{}` (e.g., `\texttt{uint32_t}`, `\texttt{sizeof}`, `\texttt{NULL}`).
- **Cross-References**: Do not hardcode chapter or section numbers. Always use dynamic LaTeX label references (`\chapref{...}`, `\secref{...}`).

## Floating Elements (Figures and Tables)

- **Rule**: Always use the `[H]` (capital H) specifier for `figure` and `table` environments.
- **Layout Consistency**: Side-by-side images and multi-image figures are **prohibited**. Each image MUST have its own dedicated `\begin{figure}[H]` environment.

## Table Design and Width Constraints

- **Rule**: If a table contains text descriptions or is placed inside a custom container box, use the `tabularx` environment instead of standard `tabular`.
- **Constraint**: Always set the width of `tabularx` to `\linewidth`.
- **Auto-Wrapping Columns**: Use the `X` column specifier (or `>{\raggedright\arraybackslash}X`) for columns containing paragraph text or long descriptions.

## Paragraph Management

- **Rule**: A single paragraph should not exceed 3--4 sentences.
- **Guideline**: Split dense paragraphs at logical transition points.
- **Vertical Spacing**: Every paragraph split MUST be separated by at least one blank line in the `.tex` source.
- **Block Environment Separation**: Always insert a blank line in the `.tex` source before and after structural block environments.

## Mandatory Arguments for Custom Boxes

Most custom environments defined in `macros.tex` (like `tipBox`, `cautionBox`, `importantBox`, `doubtbox`, `terminologyBox`, `roadmapBox`) require a **mandatory argument** for the title.

- **Rule**: You must always provide a title in curly braces `{}` immediately after the `\begin{...}` tag (e.g., `\begin{tipBox}{Pointer Safety} ... \end{tipBox}`).
