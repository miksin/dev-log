# AGENTS.md — Dev-Log Agent Rules

All AI agents contributing to this repo must follow these rules:

## Article Standards
1. Every blog post MUST include a thumbnail image in frontmatter (`image.src` and `image.alt`)
2. Article content must sound like a human developer wrote it — no stiff AI prose
3. Write in Traditional Chinese (繁體中文)
4. Target length: 400–800 words per post

## Pre-Publish Gate (Mandatory)
Before any article PR is merged, the Chronicler agent MUST run the `avoid-ai-writing` skill audit:

1. Run **detect mode** first — paste the article and ask Hermes to audit with `avoid-ai-writing` skill
2. If P0 or P1 issues are found, run **rewrite mode** to fix them
3. P2 issues: fix if time allows, otherwise document in PR description
4. Only submit the PR after audit passes (no P0/P1 remaining)

**Context profile**: use `technical-blog` for posts about code/tools; `blog` for everything else.

## Workflow
- Follow Issue → Branch → PR → Review → Merge
- Branch naming: feat/issue-N-short-desc or fix/issue-N-short-desc
- Commit messages: conventional commits (feat:, fix:, docs:, chore:)

## Content Guidelines
- Piano-related posts: include screenshots or UI descriptions
- Dev-log meta posts: explain what was built, why, and key decisions
- Avoid: marketing language, excessive bullet points, robotic transitions

## Builder Ethos

### Boil the Lake
AI makes the marginal cost of completeness near-zero. When the complete solution costs minutes more than the shortcut — **do the complete thing**. Deferring tests, cutting edge cases, or choosing the "90% solution" are anti-patterns in an AI-assisted workflow.

### Search Before Building
Before implementing anything unfamiliar, check if it already exists. Cost of checking: near-zero. Cost of reinventing: high. Layers of knowledge: (1) tried-and-true patterns, (2) current ecosystem best practices, (3) first-principles reasoning — prize (3) above all.

### User Sovereignty
AI models recommend. Users decide. Present options clearly, state what context you might be missing, and ask. Never act unilaterally on changes to stated direction — even when two agents agree.
