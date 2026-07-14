# Documentation Style Guide

## Purpose

Each file should help an engineer answer a specific operational question quickly. Prefer commands, decision points, expected output, and cautions over broad product descriptions.

## Filename conventions

Use descriptive lowercase kebab-case filenames:

```text
google-cloud.md
leaf-spine.md
spanning-tree.md
```

Rules:

- Use the product or protocol name an engineer is likely to search for.
- Separate words with hyphens, not underscores or compressed spellings.
- Avoid filenames that are broader or narrower than the actual content.
- Do not name a Git reference `github.md` unless the document is specifically about GitHub rather than Git.
- Combine a platform overview and its primary CLI when they serve the same audience and would otherwise repeat concepts.
- Keep separate files when tools have different lifecycles, safety models, or operational workflows, such as Terraform and Pulumi.
- Avoid renaming stable single-word files only for cosmetic consistency.

When renaming or merging files, update the README, cross-references, and documentation-quality workflow in the same change.

## Required metadata for maintained sheets

Place these lines immediately below the title:

```markdown
> **Applies to:** Product family or major release
> **Last reviewed:** YYYY-MM-DD
```

Use `General concepts` when the material is standards-based rather than tied to one implementation.

## Command presentation

Use angle-bracket placeholders:

```bash
show interface <interface>
kubectl get pods --namespace <namespace>
```

Do not use realistic credentials, public IP addresses belonging to third parties, internal hostnames, or customer-specific identifiers.

For state-changing commands, show a read-only verification command first and add an inline warning when the operation is destructive.

## Troubleshooting order

Troubleshooting sections should generally proceed in this order:

1. confirm scope and symptoms;
2. inspect state without changing it;
3. collect logs, counters, and timestamps;
4. compare intended and actual configuration;
5. apply the least disruptive correction;
6. verify service recovery;
7. document the change and rollback state.

## Terminology

- Use current product names and mention former names only when useful for searchability.
- Distinguish a protocol standard from a vendor implementation.
- Do not imply a command is portable across vendors or releases when it is not.
- Use “data center” as two words in prose.
- Expand an acronym on first use unless it is universally understood in the document’s audience.

## References

Prefer official project documentation, standards documents, vendor command references, and RFCs. Avoid copying large passages. Link to the authoritative source and summarize the operational implication.
