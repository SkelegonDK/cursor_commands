# PRD Interview Agent — Reference

Extended question bank and optional PRD template variants. Use when the user needs more options or a different document shape.

---

## Extended question bank

### Vision and context
- What are we building, in one sentence?
- Why is this important to the business or product strategy?
- Why now? What's changed?
- Who are the main stakeholders? Who has final say?
- How does this relate to existing products or initiatives?

### Problem and users
- What problem does this solve?
- Who experiences this problem most? (primary user/persona)
- What have you or others already tried? What worked or didn't?
- What do you know for sure about your users? What are you inferring?
- What assumptions are you making about user behavior or needs?

### Scope and success
- What is the one thing we must get right for this to be worthwhile?
- How will you personally define success?
- What does success look like in 3/6/12 months?
- How will we measure it? (metrics, outcomes, qualitative bar)
- What is the target release or timeline?
- What would make you say "we shouldn't ship this"?

### Assumptions and constraints
- What are you assuming about users, technology, or the business?
- Any hard constraints? (platform, compliance, performance, integrations, budget)
- What dependencies matter? (other teams, systems, data, vendors)
- What could block or delay this?

### Out of scope and open questions
- What are we explicitly not doing in this version?
- What might we consider later but not now?
- What's still unclear or needs research before we build?
- What would you need to see or decide before signing off?

---

## Alternate PRD template (minimal)

For very small efforts or MVPs:

```markdown
# [Name] — PRD (minimal)

## What and why
[1–2 sentences.]

## Who it's for
[Primary user/role.]

## Success
[How we'll know it worked.]

## In scope
- [Item 1]
- [Item 2]

## Out of scope
- [Item 1]

## Open questions
- [Question 1]
```

---

## Alternate PRD template (with design/tech)

When the PRD must reference design or technical direction:

Add after **User stories**:

```markdown
## User interaction and design
[Link or brief description of key flows, wireframes, or design direction.]

## Technical considerations
[Key technical constraints, stack, or integration points—high level only.]
```

Keep implementation details in separate specs; PRD stays "just enough" context (Atlassian).

---

## Anti-patterns (avoid)

- **Spec'ing everything up front:** Agile PRDs focus on shared understanding, not exhaustive detail before any build.
- **No out-of-scope:** Without it, scope creep and misalignment are likely.
- **Vague success:** Prefer "X metric improves by Y" or "User can do Z in under N steps" over "better experience."
- **Solo authorship:** PRDs are more effective when created with input from design and development (or their proxies).
- **Static document:** Treat the PRD as a living doc; update when scope or assumptions change.

---

## Trusted sources

- [Atlassian: How to create a product requirements document (PRD)](https://www.atlassian.com/agile/product-management/requirements) — Agile PRD structure, goals, assumptions, user stories, out-of-scope, collaboration.
- [Aha: PRD templates](https://aha.io/roadmapping/guide/requirements-management/what-is-a-good-product-requirements-document-template) — Product requirements and template options.
- [Stakeholder / discovery interviews](https://www.uxpin.com/studio/blog/stakeholder-interview-questions/) — Probing questions for roles, goals, success, assumptions.
