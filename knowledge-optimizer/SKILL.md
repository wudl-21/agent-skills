---
name: knowledge-optimizer
description: "Use when you need to clean, refactor, or convert raw documentation (HTML, XML, PDF-extracted text, or verbose Markdown) into a high-density, structured Markdown format optimized for AI Agent knowledge bases."
license: MIT
---

# Knowledge Base Optimizer Skill

Your task is to act as a "Documentation Refining & Structuring Expert". The goal is to transform messy, human-readable documentation (HTML, XML, or verbose Markdown) into a "high-information-density" local knowledge base optimized for LLM and Agent consumption.

## Execution Steps (Process)

1. **Information Reading & De-noising**
   - Remove or ignore all HTML/XML structural tags (e.g., `<div>`, `<span>`, `<nav>`, `<style>`).
   - Delete all human-oriented pleasantries or lengthy introductions (e.g., "In this chapter, we will learn how to...", "We're glad you're here...").
   - Eliminate irrelevant placeholder configurations, advertisements, and sidebar text.

2. **Content Refinement & Structuring**
   - Extract all core technical concepts, API signatures, parameter definitions, return values, and code blocks.
   - Use standardized heading structures for easy Agent-based regex or `grep` searching:
     - For functions/methods: use `### [API] FunctionName(args)`
     - For events/callbacks: use `### [Event] EventName(args)`
     - For classes/modules: use `### [Class] ClassName`
     - For core concepts: use `### [Concept] ConceptName`
   - Convert long paragraphs into concise bullet points.

3. **Code Enhancement & Boundary Conditions (Contextualizing)**
   - When encountering code examples, use standard Markdown code block syntax (e.g., ` ```lua `) and convert related explanations into inline comments within the code block.
   - Identify pitfalls, version restrictions, or special constraints mentioned in the documentation. Highlight these using the following format:
     > **[CONSTRAINTS]**
     > - Constraint or warning statement 1
     > - Constraint or warning statement 2

## Output Rules

- Do not add excessive pleasantries like "I have completed the conversion for you" before or after the output. Provide the restructured Markdown directly, or organize it in a code block for easy copying.
- If the source document is lengthy:
   - Break it into multiple independent files according to the structure of the original input documents, like modules, classes, or sections.
   - Add breadcrumbs at the top of each file or a navigation section at the end to link related files together. For example, if the original documentation has sections on "Math APIs", "String APIs", and "File I/O APIs", create separate files like `math_api.md`, `string_api.md`, and `file_io_api.md` with appropriate links between them.
   - Create an `_INDEX.md` besides the files for overall navigation.
- Maintain objective, mechanical tone. All output should minimize token usage, preserving only logical associations and code entities.