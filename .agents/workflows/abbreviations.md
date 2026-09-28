---
description: Audits a chapter for technical abbreviations and adds them to a terminologyBox.
---

# Abbreviation Audit Workflow

1. Open the LaTeX chapter you wish to audit.
2. The agent will:
    - Identify all caps abbreviations (e.g. **LVGL**, **SPI**, **UART**).
    - Look up their full names and basic explanations.
    - Add or update a `\begin{terminologyBox}` at the beginning of the chapter or section.
    - Format entries consistently as `\item \textbf{TERM (Full Name):} Explanation`.
3. Review the generated `terminologyBox` and confirm the definitions are accurate for the target level (beginner students).
