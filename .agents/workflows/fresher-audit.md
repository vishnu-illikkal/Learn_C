---
description: Perform a Fresher-Friendliness Audit on a course book chapter to ensure accessibility for students and beginners.
---

# Fresher Friendliness Audit Workflow

Use this workflow to evaluate a LaTeX chapter for pedagogical clarity, tone, and jargon use for students and beginners.

1. **Initialization**: Target the specified chapter file (e.g., `notes/chapters/chapter01.tex`).
2. **Analysis**: Scan for:
    - Undefined acronyms or jargon on first use.
    - Dismissive/gatekeeping language (`simply`, `just`, `obviously`, `trivial`).
    - Lack of clear mental models or memory diagrams for abstract concepts (e.g., pointers, memory segments, bit shifting).
    - Opaque code comments ("What" instead of "Why").
3. **Report Generation**:
    - Provide a structured table of findings.
    - Provide specific "Fresher-Friendly" rewrites for each flagged section.
4. **Conclusion**: Prompt the user to review findings and decide if updates should be applied.
