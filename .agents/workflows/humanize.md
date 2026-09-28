---
description: Polish chapter writing to sound natural, direct, and human.
---

# Humanizer Workflow

This workflow guides you through auditing and rewriting text to remove AI writing signatures such as inflated symbolism, overused AI vocabulary, and formulaic structures.

## Steps

1. **Read Target Segment**: Identify and read the specific LaTeX chapter or document section that needs humanizing.
2. **Analyze for AI Patterns**: Scan the text for:
   - **Inflated Symbolism**: Phrases like "serves as a testament" or "underscores the importance."
   - **Promotional/Vague Language**: Words like "groundbreaking," "vibrant," "delve into."
   - **AI Vocabulary**: High-frequency words like "furthermore," "pivotal," "tapestry," "fostering."
   - **Formulaic Structures**: Excessive use of em dashes, rule-of-three, or overly rigid bullet points.
3. **Draft Humanized Version**:
   - Write like an experienced, approachable engineering mentor explaining concepts on a whiteboard.
   - Use direct, concrete descriptions and active verbs.
   - Vary sentence lengths and rhythm.
4. **Apply Changes**: Use `replace_file_content` to update the chapter file.
