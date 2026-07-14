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

## Visual diagrams

Use Mermaid when a diagram explains relationships that are harder to understand as a flat list. Good candidates include:

- physical or logical topology;
- packet, request, or control-plane paths;
- state transitions;
- deployment and infrastructure lifecycles;
- decision trees;
- failure domains;
- ordered boot or troubleshooting sequences.

Do not add a diagram merely to repeat a command list, glossary, or short numbered procedure. A diagram should make a specific concept faster to understand.

Guidelines:

- Keep diagrams conceptual unless the document is explicitly vendor-specific.
- Use labels that remain readable in both GitHub light and dark themes.
- Avoid custom colors and styling unless they convey essential meaning.
- Keep node and edge counts low enough to read without zooming.
- Add a short explanation before or after each diagram.
- State important simplifications and do not imply that one diagram represents every vendor or failure case.
- Link visual guides to the detailed operational reference containing commands and cautions.
- Prefer one focused diagram over a large all-in-one architecture drawing.

The repository-wide diagrams live in [visual-guides.md](visual-guides.md). Command-heavy cloud and utility sheets remain text-first unless a topology or lifecycle diagram adds clear operational value. Topic files may embed a diagram directly when it is essential to understanding that specific page.

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