# Documentation Style Guide

## Purpose

Each file should help an engineer answer a specific operational question quickly. Prefer commands, decision points, expected output, and cautions over broad product descriptions.

## Repository and folder conventions

Reference sheets live under a shallow domain hierarchy:

```text
docs/<domain>/<topic>.md
```

Keep project-level documentation and repository metadata at the root. Do not place ordinary reference sheets beside `README.md`.

Folder rules:

- Use an existing domain whenever it reasonably fits.
- Keep the hierarchy to `docs/<domain>/<file>`; avoid deeper nesting.
- Do not create a directory for a single vendor, command, or page unless multiple related references justify it.
- Keep closely coupled technologies together, such as Pacemaker and Corosync.
- Separate distinct operational domains, such as containers and automation.
- Update the README, cross-references, and CI rules in the same change when restructuring files.

## Filename conventions

Use descriptive lowercase kebab-case filenames. Choose the canonical name engineers are most likely to search for.

### Protocols and standards

Use a well-known acronym when it is canonical and unambiguous:

```text
lldp.md
ospf.md
tcp.md
udp.md
```

Use a descriptive name when the acronym is ambiguous or the qualifier is part of the concept:

```text
osi-model.md
spanning-tree.md
leaf-spine.md
```

Do not mechanically append redundant suffixes such as `-protocol` when the subject is already clear. `spanning-tree.md` is preferred over both `stp.md` and `spanning-tree-protocol.md` because it is searchable and avoids ambiguity with shielded twisted pair.

### Vendors and products

Name the actual platform or product, not merely the company:

```text
arista-eos.md
cisco-ios.md
fortigate.md
junos.md
google-cloud.md
```

Within a domain-specific folder, omit redundant prefixes when the acronym is the normal product name:

```text
docs/cisco-contact-center/cuic.md
docs/cisco-contact-center/cvp.md
docs/cisco-contact-center/icm.md
```

### General rules

- Separate words with hyphens, not underscores or compressed spellings.
- Avoid filenames broader or narrower than the actual content.
- Combine a platform overview and its primary CLI when they serve the same audience and would otherwise repeat concepts.
- Keep separate files when tools have different lifecycles, safety models, or operational workflows, such as Terraform and Pulumi.
- Avoid renaming stable single-word files solely for cosmetic consistency.
- When renaming or merging files, update the README, relative links, and documentation-quality checks in the same change.

## Required status metadata

New or technically revalidated sheets must place these lines immediately below the title:

```markdown
> **Applies to:** Product family or major release
> **Last reviewed:** YYYY-MM-DD
```

Use `General concepts` when the material is standards-based rather than tied to one implementation.

An older page that has only been structurally cleaned up must not receive a false review date. Mark it clearly instead:

```markdown
> **Status:** Legacy reference — technical validation pending
```

Do not add new unvalidated legacy pages. The legacy status exists only to make inherited content honest while it is being replaced or reviewed.

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

The repository-wide diagrams live in [`docs/visual-guides.md`](docs/visual-guides.md). Command-heavy cloud and utility sheets remain text-first unless a topology or lifecycle diagram adds clear operational value. Topic files may embed a diagram directly when it is essential to understanding that specific page.

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
