# Contributing

Corrections and focused additions are welcome. The goal is operationally useful, concise, and verifiable reference material.

## Before submitting a change

1. Verify commands against official vendor or project documentation.
2. State the platform, operating system, or major release when syntax is version-sensitive.
3. Test examples in a lab or disposable environment when practical.
4. Mark destructive commands clearly and include a safer inspection step first.
5. Never commit real credentials, internal addresses, customer data, private architecture, or employer-specific interview material.

## Choose the correct location

Reference sheets belong under a shallow domain directory:

```text
docs/<domain>/<topic>.md
```

Current domains are:

- `networking`
- `network-platforms`
- `cisco-contact-center`
- `cloud`
- `containers`
- `automation`
- `linux`
- `high-availability`
- `development`

Use the closest existing domain instead of creating a one-file directory. Add a new domain only when multiple related references justify it. Keep `README.md`, `CONTRIBUTING.md`, `STYLE_GUIDE.md`, and `LICENSE` at the repository root.

## Name the file intentionally

Use the canonical name engineers are likely to search for:

- common, unambiguous protocol acronyms: `tcp.md`, `udp.md`, `ospf.md`, `lldp.md`;
- descriptive names for ambiguous concepts: `spanning-tree.md`, `osi-model.md`;
- actual platform names: `arista-eos.md`, `cisco-ios.md`, `fortigate.md`, `junos.md`;
- concise product acronyms inside an already specific folder: `docs/cisco-contact-center/cvp.md`.

Do not rename files solely to make everything an acronym or solely to spell everything out. Optimize for clarity, searchability, and domain context.

When adding, moving, or renaming a sheet:

1. update the root README index;
2. update all relative cross-references;
3. keep the hierarchy shallow;
4. ensure documentation-quality checks pass.

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

When inherited content has only received a structural cleanup and has not been technically revalidated, use:

```markdown
> **Status:** Legacy reference — technical validation pending
```

Do not assign a fresh review date to unvalidated content.

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
