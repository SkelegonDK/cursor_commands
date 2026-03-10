---
name: manuel-skills
description: Branching skill hub for Manuel's workflows. Routes to the correct procedure based on context—probing questions (ideation, debugging, new features, general), evidence-based research, security scanning, web component architecture analysis, Danish copy, or prompt templating. Use when the task matches any of these domains.
---

# Manuel Skills — Context-Based Branching

**First: identify the user's intent, then follow the matching branch below.**

## Branch Selection

| Context / Trigger | Branch | Reference |
|------------------|--------|-----------|
| Brainstorming, ideation, innovation | [Idea Generation](#branch-idea-generation) | [references/idea-generation.md](references/idea-generation.md) |
| Proposing solutions, need evidence, research | [Evidence-Based](#branch-evidence-based) | [references/based.md](references/based.md) |
| Debugging, troubleshooting, bugs | [Debugging](#branch-debugging) | [references/debugging.md](references/debugging.md) |
| Implementing new feature, requirements | [New Features](#branch-new-features) | [references/new-features.md](references/new-features.md) |
| General clarification, planning, ambiguity | [Probing](#branch-probing) | [references/probing.md](references/probing.md) |
| Security audit, vulnerabilities, OWASP | [Security Scan](#branch-security-scan) | [references/security-scan.md](references/security-scan.md) |
| Web components, React/Vue/Svelte architecture | [Web Components State Diagram](#branch-web-components) | [references/web-components.md](references/web-components.md) |
| Danish copy, naturligt dansk sprog | [Dansk Copy](#branch-dansk-copy) | [references/dansk-copy.md](references/dansk-copy.md) |
| Prompt template, role/persona prompts | [Prompt Template](#branch-prompt-template) | [references/prompt-template.md](references/prompt-template.md) |

---

## Branch: Idea Generation

**When:** User is brainstorming, exploring opportunities, generating ideas, or seeking innovation.

Ask probing questions from: Problem space, Constraints & resources, Success criteria, Inspiration & direction, Scope. Then synthesize and generate targeted ideas. See [references/idea-generation.md](references/idea-generation.md) for full question list.

---

## Branch: Evidence-Based

**When:** User needs recommendations grounded in real-world evidence, professional practice, or verified sources.

Follow: Discovery → Verification → Synthesis. Search for current solutions, cross-reference 2–3 sources, prioritize official docs and peer-reviewed content. Cite sources in output. See [references/based.md](references/based.md) for full protocol.

---

## Branch: Debugging

**When:** User is diagnosing bugs, troubleshooting, or fixing unexpected behavior.

Ask: Symptom description, Reproduction steps, Context & recent changes, Investigation so far, Environment details, Impact. Synthesize to map symptoms to root causes. See [references/debugging.md](references/debugging.md) for full questions.

---

## Branch: New Features

**When:** User wants to implement a new feature or build something new.

Ask: Core understanding (problem, user, success), Scope & boundaries, Technical considerations, Edge cases & validation. Synthesize into clear requirements. See [references/new-features.md](references/new-features.md) for full questions.

---

## Branch: Probing

**When:** User's request is ambiguous, or you need clarification before planning.

Ask: Goal clarification, Context & background, Requirements & constraints, Execution preferences. Summarize goal and top constraints before proposing a plan. See [references/probing.md](references/probing.md) for full questions.

---

## Branch: Security Scan

**When:** User wants a security audit, vulnerability assessment, or code security review.

Perform comprehensive scan: Injection, Access control, Crypto, Misconfiguration, XSS, Vulnerable components, Auth/session, Integrity, Logging, SSRF, Secrets, Input validation. Use OWASP Top 10 and CWE references. Output severity, evidence, remediation. See [references/security-scan.md](references/security-scan.md) for categories and format.

---

## Branch: Web Components State Diagram

**When:** User wants to understand or visualize web component architecture (React, Vue, Svelte, Astro, etc.).

Analyze: Component discovery → Connection analysis → State mapping. Produce Mermaid stateDiagram-v2. Classify components (Page, Container, Presentational, Layout, Provider, Utility). See [references/web-components.md](references/web-components.md) for diagram format and framework notes.

---

## Branch: Dansk Copy

**When:** User wants copy written or rewritten in natural Danish.

Rules: Kort, konkret, naturlige vendinger. Ingen dashes eller bullet points. Motiveret rejse med hjerte og hjerne. Selvsikker, moden, professionel tone. See [references/dansk-copy.md](references/dansk-copy.md).

---

## Branch: Prompt Template

**When:** User wants to craft a structured prompt with role, goal, task, requirements, and constraints.

Use template: Act as [role]. Goal: [outcome] for [audience]. Task: [imperative]. Requirements 1–3. Context. Constraints (Format, Style, Scope, Reasoning, Self-check). See [references/prompt-template.md](references/prompt-template.md).

---

## Usage

- **Automatic:** This skill applies when the task matches any branch above.
- **Manual:** Type `/manuel-skills` and specify the branch (e.g. "run security scan" or "help me probe before implementing").
