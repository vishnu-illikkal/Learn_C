---
description: Automatically apply fixes identified during a Fresher Friendliness review.
---

# Fix Fresher Friendliness Workflow

Use this workflow to automatically address pedagogical gaps, jargon issues, and tone concerns in a LaTeX chapter.

## Steps

1. **Identify Target Chapter**: Target the specified chapter file in `notes/chapters/`.
2. **Phase 1: Jargon & Acronym Definitions**:
   - Ensure all acronyms (e.g., GCC, CPU, RAM, ALU, LSB, MSB) are expanded on first use or defined in a `\begin{terminologyBox}`.
3. **Phase 2: Tone & Phrasing Correction**:
   - Remove dismissive words (`simply`, `just`, `obviously`) and rewrite in clear, encouraging language.
4. **Phase 3: Conceptual Anchoring**:
   - Add intuitive analogies or TikZ memory boxes for memory layout and pointer mechanisms.
5. **Phase 4: Code Clarity**:
   - Ensure comments explain the rationale ("Why") rather than just restating C syntax.
6. **Phase 5: Final Formatting Check**:
   - Verify all environments, listings, and dynamic references adhere to `latex-formatter`.
