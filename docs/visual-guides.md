# Infrastructure Visual Guides

> **Applies to:** General infrastructure and network engineering concepts
> **Last reviewed:** 2026-07-18

These Mermaid diagrams supplement the detailed references with quick topology, lifecycle, and troubleshooting views. They are intentionally conceptual: use the linked pages for commands, platform differences, safety warnings, and implementation details.

The collection is deliberately limited to concepts where visual relationships improve comprehension. Command-oriented cloud, CLI, and utility references remain text-first rather than receiving decorative diagrams.

## Spanning tree: root and alternate path

Related reference: [networking/spanning-tree.md](networking/spanning-tree.md)

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

Related reference: [networking/leaf-spine.md](networking/leaf-spine.md)

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

Related reference: [automation/pulumi.md](automation/pulumi.md)

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

## Talos and Omni management boundary

Related references: [kubernetes-platforms/talos-linux.md](kubernetes-platforms/talos-linux.md) and [kubernetes-platforms/omni.md](kubernetes-platforms/omni.md)

Standalone Talos is managed directly through the Talos API. When a machine joins Omni, Omni becomes the management authority and brokers normal Talos and Kubernetes access through its identity and policy model.

```mermaid
flowchart LR
    Engineer["Engineer or automation"]
    TalosConfig["talosconfig"]
    TalosAPI["Talos API"]
    Standalone["Standalone Talos Linux nodes"]

    Identity["Identity provider or<br/>Omni service account"]
    OmniConfig["omniconfig"]
    Omni["Omni API and UI"]
    SideroLink["SideroLink<br/>encrypted connectivity"]
    Managed["Omni-managed Talos nodes"]
    Kubernetes["Kubernetes API"]

    Engineer --> TalosConfig
    TalosConfig --> TalosAPI
    TalosAPI --> Standalone

    Engineer --> OmniConfig
    Identity --> Omni
    OmniConfig --> Omni
    Omni --> SideroLink
    SideroLink --> Managed
    Omni --> Kubernetes
    Managed --> Kubernetes
```

The two paths are intentionally different. Direct standalone procedures should not be assumed to work unchanged on an Omni-managed machine.

## Omni machine-to-cluster lifecycle

Related reference: [kubernetes-platforms/omni.md](kubernetes-platforms/omni.md)

Omni can register manually provisioned machines or obtain machines through infrastructure providers, then allocate them to a cluster and manage their Talos and Kubernetes lifecycle.

```mermaid
flowchart LR
    Media["Omni installation media<br/>or Machine Join Config"]
    Provider["Infrastructure provider<br/>bare metal, Proxmox, vSphere,<br/>libvirt, or KubeVirt"]
    Boot["Boot or create machine"]
    Register["Register through SideroLink"]
    Inventory["Machine inventory<br/>labels and status"]
    Class["Machine class or<br/>explicit allocation"]
    Cluster["Cluster template<br/>control plane and workers"]
    Configure["Omni applies Talos configuration"]
    Bootstrap["Bootstrap Kubernetes"]
    Operate["Scale, upgrade,<br/>back up, and monitor"]
    Recover["Restore or reprovision"]

    Media --> Boot
    Provider --> Boot
    Boot --> Register
    Register --> Inventory
    Inventory --> Class
    Class --> Cluster
    Cluster --> Configure
    Configure --> Bootstrap
    Bootstrap --> Operate
    Operate --> Recover
    Recover --> Inventory
```

Out-of-band deletion of provider-managed or control-plane machines can leave Omni waiting for resources that no longer exist and can break etcd quorum.

## Linux boot sequence

Related reference: [linux/linux-boot.md](linux/linux-boot.md)

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

Related reference: [development/git.md](development/git.md)

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

## Spec-driven development lifecycle

Related reference: [development/spec-driven-development.md](development/spec-driven-development.md)

Spec-driven development separates expected behavior from technical design, breaks the design into verifiable tasks, and checks the completed implementation against the original acceptance criteria.

```mermaid
flowchart LR
    Idea["Problem or feature idea"]
    Spec["Specification<br/>what and why"]
    Clarify{"Requirements clear<br/>and testable?"}
    Refine["Clarify scope, constraints,<br/>and edge cases"]
    Plan["Technical plan<br/>how"]
    Tasks["Ordered tasks"]
    Implement["Implement one task"]
    Validate{"Tests and acceptance<br/>criteria pass?"}
    Correct["Correct code, plan,<br/>or specification"]
    Review["Pull request and review"]
    Deploy["Merge, deploy,<br/>and verify"]
    Learn["Operational feedback<br/>and new requirements"]

    Idea --> Spec
    Spec --> Clarify
    Clarify -->|No| Refine
    Refine --> Spec
    Clarify -->|Yes| Plan
    Plan --> Tasks
    Tasks --> Implement
    Implement --> Validate
    Validate -->|No| Correct
    Correct --> Implement
    Validate -->|Yes| Review
    Review --> Deploy
    Deploy --> Learn
    Learn --> Spec
```

The diagram is conceptual. A small low-risk change may combine stages, while a production or regulated change may require additional security, architecture, migration, and approval gates.

## Operational troubleshooting sequence

Related guidance: [../STYLE_GUIDE.md](../STYLE_GUIDE.md)

This sequence is reusable across network, Linux, cloud, and infrastructure-automation troubleshooting.

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
