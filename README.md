# Cursor Skills

A collection of reusable Cursor skills (`.mdc` rules) that can be invoked on demand to guide AI behavior for specific tasks.

## Skills

| Skill | Description |
|-------|-------------|
| **based** | Evidence-based research — grounds suggestions in real, verified sources |
| **debugging** | Systematic debugging — asks diagnostic questions to efficiently resolve bugs |
| **dansk-copy** | Danish copywriting — writes short, confident, professional Danish copy |
| **idea-generation** | Idea generation — asks probing questions to generate relevant, innovative ideas |
| **new-features** | New feature planning — asks clarifying questions before implementation |
| **probing-questions** | General probing — reduces ambiguity and gathers details for thorough planning |
| **prompt-template** | Prompt template — reusable structure for crafting well-formed prompts |
| **security-scan** | Security audit — comprehensive code scan based on OWASP Top 10 and CWE/SANS Top 25 |
| **web-components-state-diagram** | Architecture analysis — generates Mermaid state diagrams from component codebases |

## Rules

| Rule | Description |
|------|-------------|
| **test-before-assuming** | Always run tests before assuming why they fail |

## Usage

Skills are stored as `.mdc` files in `_cursor/rules/` with `alwaysApply: false`. They are invoked manually by referencing them in Cursor (e.g. `@rules` or by selecting the skill in the rules panel).

## Structure

```
_cursor/
├── rules/
│   ├── based.mdc
│   ├── debugging.mdc
│   ├── dansk-copy.mdc
│   ├── idea-generation.mdc
│   ├── new-features.mdc
│   ├── probing-questions.mdc
│   ├── prompt-template.mdc
│   ├── security-scan.mdc
│   ├── test-before-assuming.mdc
│   └── web-components-state-diagram.mdc
```
