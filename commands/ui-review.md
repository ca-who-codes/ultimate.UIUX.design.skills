---
description: Audit a UI (file, component, screenshot, or live page) against the UI/UX Design Pro checklist and return prioritized, specific findings.
argument-hint: "[path/to/component, URL, or description of what to review]"
---

Review the following interface for UI/UX quality: **$ARGUMENTS**

Use the `design-reviewer` approach:

1. Read `knowledge/05-quality/review-checklist.md` and `knowledge/INDEX.md`, then load the reference files relevant to what's being reviewed.
2. Inspect the actual target — read the component code at the given path, analyze the screenshot, or fetch the page. Evaluate real values (spacing, sizes, hex, contrast ratios, focus states), not impressions.
3. Check every dimension: hierarchy, spacing/alignment, typography, color/contrast, component states (empty/loading/error/success), interaction/motion, responsive, accessibility, performance, microcopy, polish.

Return findings grouped by severity (🔴 Blocking / 🟡 Should fix / 🟢 Polish), each with the issue, **why** it matters (cite the knowledge file), and the **exact fix** (specific value or code). End with "✅ What's working" and the **Top 3 highest-leverage changes**.

Be specific and honest. Default to finding real issues; separate objective failures from subjective taste.
