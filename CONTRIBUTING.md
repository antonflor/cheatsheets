# Contributing

Corrections and focused additions are welcome. The goal is operationally useful, concise, and verifiable reference material.

## Before submitting a change

1. Verify commands against official vendor or project documentation.
2. State the platform, operating system, or major release when syntax is version-sensitive.
3. Test examples in a lab or disposable environment when practical.
4. Mark destructive commands clearly and include a safer inspection step first.
5. Never commit real credentials, internal addresses, customer data, private architecture, or employer-specific interview material.

## Cheat-sheet structure

Use the following order when it fits the topic:

```markdown
# Product or Topic Cheat Sheet

> **Applies to:** Product family or major release
> **Last reviewed:** YYYY-MM-DD

A one-paragraph scope statement.

## Safety

Important operational warnings.

## Quick reference

Commands and concise explanations.

## Troubleshooting workflow

A read-only, least-disruptive-first sequence.

## Official references

- [Documentation title](https://example.com)
```

## Style rules

- Use one `#` heading per document.
- Use fenced code blocks with a language identifier when possible.
- Use placeholders in angle brackets, such as `<interface>` or `<project-id>`.
- Prefer tables for compact command references.
- Explain important side effects directly beside the command.
- Avoid marketing language such as “comprehensive,” “ultimate,” or “one-stop.”
- Keep conceptual overviews separate from command references when either becomes long.

## Pull requests

A pull request should explain:

- what changed;
- why the previous content was inaccurate or difficult to use;
- which official references were used;
- how the examples were validated.

For a simple typo or broken link, a short explanation is sufficient.