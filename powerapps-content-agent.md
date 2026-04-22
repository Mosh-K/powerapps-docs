# Power Apps Content Optimization Agent

## Objective

Analyze Power Apps documentation using user feedback and content signals.
Identify gaps, outdated content, and high-impact improvements.
Generate updated content and create a draft PR with a linked ADO work item.

---

## Data Sources

Use the following sources for analysis:

- Content engagement report (Power BI)
- UUF / verbatim user feedback
- Deflection report
- Source code (GitHub: MicrosoftDocs/powerapps-docs-pr)

---

## Analysis Tasks

### 1. Identify Content Opportunities

- Find high-traffic articles not updated recently
- Detect articles with high deflection rates
- Identify recurring user pain points from UUF/verbatim feedback
- Highlight unclear, incomplete, or outdated guidance

### 2. Prioritize Fixes

Focus on:

- High traffic + high deflection
- Frequently reported user confusion
- Missing or incomplete Power Apps scenarios (e.g., canvas apps, model-driven apps, Copilot features)

---

## Content Update Tasks

### 3. Generate Improvements

For each identified article:

- Improve clarity and accuracy
- Add missing steps or scenarios
- Update terminology to current product language
- Ensure content is:
  - Present tense
  - Active voice
  - Beginner-friendly where applicable

### 4. Output Format

- Provide updated markdown for each file
- Keep structure aligned with Microsoft Learn standards
- Include headings, steps, and examples where relevant

---

## PR Creation

### 5. Create GitHub PR

Repository: MicrosoftDocs/powerapps-docs-pr

Requirements:

- Base branch: main
- Create a new branch (DO NOT fork)
- Create PR in draft mode
- Include:
  - Clear title (customer-focused)
  - Summary of changes
  - Rationale based on user feedback
- Add labels:
  - `ai-assisted` (if applicable)

---

## ADO Work Item

### 6. Create and Link Work Item

- Create ADO work item for tracking
- Use correct parent (per team guidance)
- Link PR to ADO work item
- Include:
  - Problem statement
  - Source of feedback (UUF, deflection, engagement)
  - Expected impact

---

## Output

Return:

- PR link and number
- ADO work item link
- Summary of:
  - What was changed
  - Why (based on feedback/data)
  - Expected impact (deflection reduction, clarity improvement)

---

## Notes

- Prefer branching over forks
- Ensure all changes are reviewed before publishing
