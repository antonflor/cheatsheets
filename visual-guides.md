# Infrastructure Visual Guides

> **Applies to:** General infrastructure and network engineering concepts
> **Last reviewed:** 2026-07-14

These Mermaid diagrams supplement the detailed cheat sheets with quick topology, lifecycle, and troubleshooting views. They are intentionally conceptual: use the linked reference pages for commands, platform differences, safety warnings, and implementation details.

The collection is deliberately limited to concepts where visual relationships improve comprehension. Command-oriented cloud, CLI, and utility references remain text-first rather than receiving decorative diagrams.

## Spanning tree: root and alternate path

Related reference: [spanning-tree.md](spanning-tree.md)

In a redundant Layer 2 triangle, spanning tree elects a root bridge and keeps one redundant path from forwarding. The alternate path can transition to forwarding after a topology failure.

```mermaid
flowchart TB
    Root["SW1<br/>Root bridge"]
    SW2["SW2<br/>Root port toward SW1"]
    SW3["SW3<br/>Root port toward SW1"]
    UsersA["Access devices"]
    UsersB["Access devices"]

    Root ---|"Designated / forwarding"| SW2
    Root ---|"Designated / forwarding"| SW3
    SW2 -. "Alternate / discarding on one side" .- SW3
    SW2 --- UsersA
    SW3 --- UsersB
```

The exact blocked side depends on root-path cost and bridge or port tie-breakers. The diagram does not imply that the same physical port always blocks.

## Leaf-spine fabric

Related reference: [leaf-spine.md](leaf-spine.md)

Every leaf connects to every spine. Traffic between endpoints on different leaves normally uses one spine hop, providing predictable path length and multiple equal-cost routes.

```mermaid
flowchart TB
    Spine1["Spine 1"]
    Spine2["Spine 2"]

    Leaf1["Leaf 1"]
    Leaf2["Leaf 2"]
    Leaf3["Leaf 3"]

    Rack1["Rack / endpoints"]
    Rack2["Rack / endpoints"]
    Rack3["Rack / endpoints"]

    Spine1 --- Leaf1
    Spine1 --- Leaf2
    Spine1 --- Leaf3
    Spine2 --- Leaf1
    Spine2 --- Leaf2
    Spine2 --- Leaf3

    Leaf1 --- Rack1
    Leaf2 --- Rack2
    Leaf3 --- Rack3
```

A healthy routed fabric normally loses capacity rather than reachability when one spine fails, assuming every leaf still has another working spine path.

## Pulumi change lifecycle

Related reference: [pulumi.md](pulumi.md)

Pulumi evaluates program code and stack configuration, previews the proposed change, applies approved changes through provider APIs, and records resource state in the selected backend.

```mermaid
flowchart LR
    Code["Pulumi program"]
    Config["Stack configuration<br/>and encrypted secrets"]
    Preview["pulumi preview"]
    Review{"Expected change?"}
    Update["pulumi up"]
    Providers["Cloud, Kubernetes,<br/>SaaS, or custom provider APIs"]
    Infra["Actual infrastructure"]
    State["Pulumi state backend"]
    Investigate["Fix code, config,<br/>identity, or drift"]
    Refresh["pulumi refresh<br/>after review"]

    Code --> Preview
    Config --> Preview
    Preview --> Review
    Review -->|Yes| Update
    Review -->|No| Investigate
    Investigate --> Preview
    Update --> Providers
    Providers --> Infra
    Update --> State
    Infra --> Refresh
    Refresh --> State
```

A preview is not a substitute for validating the selected stack, cloud identity, region, policy controls, and rollback plan.

## Linux boot sequence

Related reference: [linux-boot.md](linux-boot.md)

The boot process moves from firmware into the bootloader, kernel, early userspace, PID 1, and finally normal services and login targets.

```mermaid
flowchart TD
    Power["Power on or reboot"]
    Firmware["BIOS / UEFI<br/>hardware initialization"]
    BootEntry["Boot entry and bootloader"]
    Kernel["Linux kernel<br/>CPU, memory, drivers"]
    Initramfs["Initramfs / early userspace<br/>storage, encryption, LVM, RAID"]
    RootFS["Real root filesystem"]
    PID1["PID 1<br/>usually systemd"]
    Units["Mounts, devices, sockets,<br/>and service units"]
    Target["Configured target<br/>and user access"]

    Power --> Firmware
    Firmware --> BootEntry
    BootEntry --> Kernel
    Kernel --> Initramfs
    Initramfs --> RootFS
    RootFS --> PID1
    PID1 --> Units
    Units --> Target
```

A failure should be investigated at the earliest incomplete stage: firmware entry, bootloader, kernel, initramfs, root filesystem, or userspace service activation.

## Git branch and pull-request workflow

Related reference: [git.md](git.md)

A small branch-based workflow keeps changes isolated, reviewable, and recoverable before they reach the shared default branch.

```mermaid
flowchart LR
    Main["main"]
    Branch["Create feature branch"]
    Edit["Edit and inspect diff"]
    Commit["Commit focused changes"]
    Push["Push branch"]
    PR["Open pull request"]
    Checks{"Review and checks pass?"}
    Fix["Update branch<br/>or resolve conflicts"]
    Merge["Merge or squash"]
    UpdatedMain["Updated main"]

    Main --> Branch
    Branch --> Edit
    Edit --> Commit
    Commit --> Push
    Push --> PR
    PR --> Checks
    Checks -->|No| Fix
    Fix --> Edit
    Checks -->|Yes| Merge
    Merge --> UpdatedMain
```

Use `git status`, staged and unstaged diffs, and a backup branch before destructive history repair.

## Operational troubleshooting sequence

Related guidance: [STYLE_GUIDE.md](STYLE_GUIDE.md)

This sequence is reusable across network, Linux, cloud, and infrastructure-as-code troubleshooting.

```mermaid
flowchart TD
    Scope["Confirm scope and symptoms"]
    Observe["Inspect state without changing it"]
    Evidence["Collect logs, counters,<br/>timestamps, and recent changes"]
    Compare["Compare intended and actual state"]
    Cause{"Likely cause supported<br/>by evidence?"}
    More["Gather narrower evidence"]
    Correct["Apply the least disruptive correction"]
    Verify["Verify service and failure recovery"]
    Record["Document change, evidence,<br/>and rollback state"]

    Scope --> Observe
    Observe --> Evidence
    Evidence --> Compare
    Compare --> Cause
    Cause -->|No| More
    More --> Evidence
    Cause -->|Yes| Correct
    Correct --> Verify
    Verify --> Record
```

Avoid using a diagram merely to restate a flat command list. Mermaid is most valuable for relationships, paths, decisions, state transitions, and failure domains.