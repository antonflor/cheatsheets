# Talos Linux Cheat Sheet

> **Applies to:** Talos Linux 1.13 and `talosctl` 1.13
> **Last reviewed:** 2026-07-18

A practical operational reference for installing, configuring, inspecting, upgrading, backing up, and recovering Kubernetes nodes running Talos Linux.

> [!WARNING]
> Talos is managed through its API rather than SSH. Always verify the active `talosconfig` context, endpoints, target nodes, cluster identity, and control-plane quorum before applying configuration, upgrading, resetting, or recovering a node.

## What Talos Linux is

Talos Linux is an immutable, API-managed operating system designed specifically for Kubernetes.

Key characteristics:

- no SSH server or interactive package-management workflow;
- declarative machine configuration;
- management through the Talos API and `talosctl`;
- minimal userspace and reduced mutable surface area;
- containerd and Kubernetes components managed as system services;
- support for bare metal, virtual machines, cloud platforms, edge systems, and local test clusters.

Talos Linux is the node operating system. Kubernetes is the orchestrator running on top of it. [Omni](omni.md) is an optional management platform above Talos and Kubernetes.

## Core objects

| Object | Purpose |
|---|---|
| Machine configuration | Declarative operating-system and Kubernetes-node configuration |
| `talosconfig` | Client contexts, API endpoints, node targets, and credentials |
| Endpoint | Talos API address used by `talosctl` to reach the cluster |
| Node | Machine targeted by a command; commonly supplied with `--nodes` or `-n` |
| Control plane | Kubernetes API and etcd nodes |
| Worker | Kubernetes workload node without control-plane components |
| Maintenance mode | Pre-configuration API state used during installation or recovery |
| Image Factory schematic | Content-addressed image customization definition |
| System extension | Additional supported firmware, drivers, or services included in Talos images |

## Install `talosctl`

macOS or Linux with Homebrew:

```bash
brew install siderolabs/tap/talosctl
```

Verify the client:

```bash
talosctl version --client
talosctl --help
```

Keep the `talosctl` client version aligned with the Talos version being operated, especially while generating machine configurations or performing upgrades.

## Inspect client context before operating

```bash
talosctl config contexts
talosctl config info
talosctl config context <context>
talosctl config endpoint <control-plane-endpoint-1> <control-plane-endpoint-2>
talosctl config node <default-node-1> <default-node-2>
```

Override context targets for one command:

```bash
talosctl --context <context> \
  --endpoints <endpoint> \
  --nodes <node> \
  version
```

> [!CAUTION]
> Endpoints and nodes are different. Endpoints are API entry points; nodes are the machines the request acts on. Avoid leaving a destructive command dependent on an old default node list.

## Local disposable cluster

Create a local Docker-backed Talos cluster:

```bash
talosctl cluster create docker
kubectl get nodes -o wide
```

Inspect a local node:

```bash
talosctl dashboard --nodes <node-ip>
```

Destroy the disposable cluster:

```bash
talosctl cluster destroy
```

Do not treat the local Docker quickstart as a production architecture reference.

## Standalone cluster lifecycle

A normal standalone deployment follows this sequence:

1. Boot each machine with Talos installation media.
2. Generate control-plane, worker, and client configurations.
3. Apply the machine configuration to each node.
4. Configure `talosctl` endpoints and node targets.
5. Bootstrap etcd exactly once on one control-plane node.
6. Retrieve kubeconfig and verify Kubernetes health.

Generate machine configurations:

```bash
talosctl machineconfig gen \
  <cluster-name> \
  https://<kubernetes-api-endpoint>:6443 \
  --output-dir ./generated
```

Validate before applying:

```bash
talosctl validate \
  --config ./generated/controlplane.yaml \
  --mode metal \
  --strict
```

Apply to a machine in maintenance mode:

```bash
talosctl apply-config \
  --insecure \
  --nodes <node-ip> \
  --file ./generated/controlplane.yaml
```

Bootstrap one control-plane node:

```bash
talosctl bootstrap --nodes <bootstrap-control-plane>
```

Retrieve Kubernetes credentials:

```bash
talosctl kubeconfig ./kubeconfig
KUBECONFIG=./kubeconfig kubectl get nodes -o wide
```

> [!DANGER]
> Bootstrap is a one-time cluster operation. Do not run it again on an already bootstrapped cluster unless performing a documented recovery procedure.

## Read-only health checks

Start with these commands before changing a node:

```bash
talosctl version
talosctl health
talosctl dashboard
talosctl services
talosctl get members
talosctl get machines
talosctl etcd members
talosctl etcd status
kubectl get nodes -o wide
kubectl get pods --all-namespaces
```

Some resources or columns vary by Talos release. Use discovery commands when needed:

```bash
talosctl get rd
talosctl get <resource-type> --output yaml
```

## Services, logs, and support bundles

List service state:

```bash
talosctl services
```

Inspect service logs:

```bash
talosctl logs kubelet
talosctl logs containerd
talosctl logs etcd
talosctl logs kubelet --follow
```

Inspect kernel messages:

```bash
talosctl dmesg
talosctl dmesg --follow
```

Collect a support archive:

```bash
talosctl support \
  --nodes <node-1>,<node-2> \
  --output talos-support.zip
```

Support bundles can contain network topology, workload metadata, logs, and infrastructure details. Sanitize them before sharing outside the organization.

## Inspect configuration and runtime resources

```bash
talosctl get machineconfig --output yaml
talosctl get machinestatus
talosctl get nodename
talosctl get links
talosctl get addresses
talosctl get routes
talosctl get extensions
talosctl mounts
talosctl usage
talosctl processes
talosctl netstat
```

Resource names can change as APIs evolve. Use `talosctl get rd` to identify current resource definitions.

## Patch machine configuration

Preview a configuration application:

```bash
talosctl apply-config \
  --nodes <node> \
  --file <machine-config.yaml> \
  --dry-run
```

Patch a node from a YAML patch:

```bash
talosctl patch machineconfig \
  --nodes <node> \
  --patch @<patch-file.yaml>
```

Apply a complete reviewed configuration:

```bash
talosctl apply-config \
  --nodes <node> \
  --file <machine-config.yaml>
```

Useful application modes include `auto`, `no-reboot`, `reboot`, `staged`, and `try`.

```bash
talosctl apply-config \
  --nodes <node> \
  --file <machine-config.yaml> \
  --mode try \
  --timeout 2m
```

> [!WARNING]
> A machine configuration is the source of truth for the node. Avoid interactive one-off edits that are not reflected in Git or the configuration-generation workflow.

## Image Factory and system extensions

Talos Image Factory generates customized installation assets from a content-addressed schematic.

Common customizations include:

- supported system extensions;
- firmware and microcode;
- kernel arguments;
- platform overlays;
- ISO, PXE, disk, UKI, and installer images;
- Secure Boot assets.

Inspect the installed schematic and extensions:

```bash
talosctl get extensions
```

A typical upgrade image follows this pattern:

```text
factory.talos.dev/metal-installer/<schematic-id>:<talos-version>
```

Use the same intended schematic across installation media and the installer image so nodes do not unexpectedly lose required extensions during installation or upgrade.

## Upgrade Talos Linux

Before upgrading:

```bash
talosctl health
talosctl etcd members
talosctl etcd status
kubectl get nodes -o wide
kubectl get pods --all-namespaces
```

Upgrade one node with an Image Factory installer:

```bash
talosctl upgrade \
  --nodes <node> \
  --image factory.talos.dev/metal-installer/<schematic-id>:<target-version>
```

Operational sequence:

1. Confirm the supported upgrade path.
2. Confirm a recent etcd backup.
3. Upgrade one control-plane node at a time.
4. Wait for the node, etcd, and Kubernetes to become healthy.
5. Continue with remaining control-plane nodes.
6. Upgrade workers in controlled batches.
7. Run a full post-upgrade validation.

> [!DANGER]
> Never upgrade multiple control-plane nodes simultaneously without proving that etcd quorum remains available.

## Upgrade Kubernetes

```bash
talosctl upgrade-k8s \
  --nodes <control-plane-node> \
  --to <target-kubernetes-version>
```

Review compatibility between the current Talos release and the target Kubernetes release before starting. Validate API health, node readiness, system pods, admission webhooks, networking, storage, and critical workloads afterward.

## etcd inspection and backup

Inspect members and status:

```bash
talosctl etcd members
talosctl etcd status --nodes <control-plane-1>,<control-plane-2>,<control-plane-3>
```

Create a snapshot from a healthy control-plane node:

```bash
talosctl etcd snapshot ./etcd-$(date +%Y%m%d-%H%M%S).snapshot \
  --nodes <control-plane-node>
```

Store snapshots outside the cluster, protect them as sensitive data, and test the recovery process.

## Recover etcd from a snapshot

Recovery is a planned disaster-recovery operation, not a routine repair command.

```bash
talosctl bootstrap \
  --nodes <recovery-control-plane> \
  --recover-from ./etcd.snapshot
```

Before recovery:

- confirm the snapshot belongs to the intended cluster;
- preserve machine configurations and secrets;
- understand whether surviving etcd members must be reset;
- use console access;
- follow the recovery procedure for the exact Talos version.

> [!DANGER]
> Incorrect etcd recovery can permanently replace newer cluster state or create split-brain conditions.

## Reboot, shutdown, and rollback

```bash
talosctl reboot --nodes <node>
talosctl shutdown --nodes <node>
talosctl rollback --nodes <node>
```

Use forced reboot or shutdown modes only when graceful teardown is impossible and the effect on workloads and etcd is understood.

## Reset a node

Inspect first:

```bash
kubectl get node <node-name> -o wide
talosctl etcd members
talosctl get disks --nodes <node>
```

Reset a machine:

```bash
talosctl reset --nodes <node>
```

Reset selected Talos partitions instead of every disk:

```bash
talosctl reset \
  --nodes <node> \
  --system-labels-to-wipe STATE \
  --system-labels-to-wipe EPHEMERAL
```

> [!DANGER]
> `talosctl reset` is destructive. The default reset can wipe the machine, remove it from Kubernetes, and remove it from etcd. Cloud VMs may become unbootable when their system disk is wiped.

## Secure Boot and disk encryption

Talos supports Secure Boot using signed Unified Kernel Images and can use TPM-backed LUKS2 keys for system-volume encryption.

Verify Secure Boot state:

```bash
talosctl get securitystate --nodes <node>
```

Operational cautions:

- switching a non-UKI installation to Secure Boot may require reinstallation;
- preserve signing and PCR-policy keys when using custom Secure Boot assets;
- TPM policy changes can affect volume unlock;
- encryption settings commonly apply during initial volume provisioning, not retroactively;
- maintain tested recovery procedures before enabling encryption broadly.

## Networking and KubeSpan

Talos networking is declared in machine configuration and exposed through API resources.

```bash
talosctl get links
talosctl get addresses
talosctl get routes
talosctl get resolvers
talosctl get time
```

KubeSpan can provide encrypted node-to-node connectivity across networks. Treat it as part of the cluster design: document address selection, discovery dependencies, MTU, routing, failure behavior, and observability.

## Troubleshooting workflow

1. Confirm the active context, endpoints, and node targets.
2. Check physical or virtual console state.
3. Run `talosctl version`, `health`, `services`, and `dashboard`.
4. Inspect `dmesg` and the relevant service logs.
5. Verify network addresses, routes, DNS, and time synchronization.
6. Check etcd membership and quorum on control-plane nodes.
7. Check Kubernetes nodes and system pods.
8. Compare the running machine configuration with the intended configuration.
9. Collect a support bundle before destructive repair.
10. Apply the least disruptive documented recovery action.

## Common failure patterns

| Symptom | Checks |
|---|---|
| `talosctl` cannot connect | Context, endpoint reachability, TCP 50000, certificate validity, Omni ownership |
| Node in maintenance mode | Installation media, missing or rejected machine configuration, disk state |
| Kubernetes API unavailable | Control-plane services, etcd quorum, API endpoint or load balancer |
| Worker remains `NotReady` | Kubelet logs, CNI, DNS, time, container images, certificates |
| Upgrade stalls | Console, installer image, extension compatibility, disk space, network access |
| Missing driver or firmware | Image Factory schematic and installed extensions |
| Node rejoins with old settings | Generated configuration, applied patch, reboot requirement, wrong target node |
| etcd unhealthy | Member list, disk latency, quorum, time, network loss, alarms |

## References

- [Talos Linux documentation](https://docs.siderolabs.com/talos)
- [Talos Linux 1.13 CLI reference](https://docs.siderolabs.com/talos/v1.13/reference/cli)
- [Talos Linux getting started](https://docs.siderolabs.com/talos/v1.13/getting-started/getting-started)
- [Image Factory](https://docs.siderolabs.com/talos/v1.13/learn-more/image-factory)
- [Talos logging](https://docs.siderolabs.com/talos/v1.13/configure-your-talos-cluster/logging-and-telemetry/logging)
- [Resetting a machine](https://docs.siderolabs.com/talos/v1.13/configure-your-talos-cluster/lifecycle-management/resetting-a-machine)
- [Secure Boot](https://docs.siderolabs.com/talos/v1.13/platform-specific-installations/bare-metal-platforms/secureboot)
