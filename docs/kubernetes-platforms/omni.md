# Sidero Labs Omni Cheat Sheet

> **Applies to:** Current Sidero Labs Omni and `omnictl`
> **Last reviewed:** 2026-07-18

A practical operational reference for registering Talos Linux machines, creating and managing Kubernetes clusters, handling access, upgrades, backups, infrastructure providers, and recovery through Omni.

> [!WARNING]
> Once a machine joins Omni, Omni becomes the management authority for that Talos installation. The local Talos API is disabled for normal direct administration, and future configuration changes must be made through Omni. Leaving that model generally requires reprovisioning the machine with standard Talos installation media.

## Product relationship

| Name | Meaning |
|---|---|
| Sidero Labs | The vendor that develops Talos Linux and Omni |
| Talos Linux | Immutable, API-managed Kubernetes node operating system |
| Omni | Fleet, lifecycle, identity, and cluster-management platform for Talos and Kubernetes |
| SideroLink | Encrypted connectivity used between machines and Omni |
| Sidero Metal | Separate older bare-metal and Cluster API project; not another name for Omni |

Omni is not a Kubernetes distribution. It manages Talos Linux machines and the Kubernetes clusters running on them.

## Core capabilities

- register Talos machines across bare metal, virtual machines, cloud, edge, and mixed environments;
- create, scale, upgrade, and delete Kubernetes clusters;
- manage Talos and Kubernetes access through centralized identity;
- provide encrypted machine connectivity through SideroLink;
- create machine classes and cluster templates;
- integrate infrastructure providers for static and dynamic machine provisioning;
- manage etcd backups and cluster recovery;
- expose UI and API-driven workflows through `omnictl`;
- support workload access and service-proxy features where enabled.

## Hosted and self-hosted Omni

Omni is available as:

- hosted SaaS operated by Sidero Labs;
- licensed self-hosted deployment;
- non-production or home-lab use under the applicable BUSL terms.

Production use requires a commercial license. Verify current licensing and deployment terms before adopting Omni for business workloads.

Self-hosting options can include:

- a single virtual machine;
- Kubernetes not managed by the same Omni instance;
- external etcd;
- more complex high-availability designs.

Hosted Omni is generally the lowest-operational-overhead option. Self-hosted Omni introduces responsibility for backups, upgrades, certificates, identity, storage, registry services, and disaster recovery.

## Install `omnictl`

macOS or Linux with Homebrew:

```bash
brew install siderolabs/tap/omnictl
```

Verify the client:

```bash
omnictl version
omnictl --help
```

## Client configuration

Omni client configuration is called `omniconfig`.

Default path:

```text
$HOME/.talos/omni/config
```

Inspect contexts:

```bash
omnictl config contexts
omnictl config info
omnictl config context <context>
```

Create a context manually when required:

```bash
omnictl config new \
  --url <omni-grpc-url> \
  --identity <identity> \
  <path>
```

Environment override:

```bash
export OMNICONFIG=<path-to-omniconfig>
```

> [!CAUTION]
> Verify the active Omni context and identity before applying templates, deleting clusters, rotating join tokens, or downloading administrative credentials.

## Inspect available resources

Omni uses a resource-oriented API similar in feel to `kubectl` and `talosctl`.

```bash
omnictl get rd
omnictl get clusters
omnictl get machines
omnictl get machineclasses
omnictl get clusterstatuses
omnictl get etcdbackupstatuses
```

Inspect one resource:

```bash
omnictl get <resource-type> <resource-id> -o yaml
```

Watch changes:

```bash
omnictl get <resource-type> --watch
```

Resource names can evolve. Use `omnictl get rd` rather than assuming an old resource spelling remains valid.

## Download installation media

Download Omni-configured media:

```bash
omnictl download iso
```

Use the UI or CLI to generate media that includes the intended Omni join configuration and Talos image customizations.

Typical lifecycle:

1. Generate installation media.
2. Boot a bare-metal machine or VM.
3. Allow the machine to register through SideroLink.
4. Label or classify the machine.
5. Assign it to a cluster or machine class.
6. Let Omni apply Talos configuration and bootstrap Kubernetes.

## Join an existing Talos machine

Create a join token:

```bash
omnictl jointoken create <token-name> --ttl <duration>
```

List tokens:

```bash
omnictl jointoken list
```

Generate a partial machine configuration:

```bash
omnictl jointoken machine-config <machine-id> \
  --token-name <token-name>
```

Generate join kernel arguments when needed:

```bash
omnictl jointoken kernel-args --token-name <token-name>
```

Join methods include:

- booting preconfigured Omni installation media;
- applying a Machine Join Config to a machine booted from standard Talos media.

> [!DANGER]
> Joining a machine changes its management model. Confirm that existing automation, direct Talos access, recovery procedures, and break-glass expectations are compatible with Omni before enrolling production nodes.

## SideroLink connectivity

SideroLink provides encrypted communication between Omni and managed machines.

Check when registration or management fails:

- DNS resolution and time synchronization;
- outbound network access to Omni endpoints;
- UDP reachability where required;
- firewall and proxy rules;
- join-token validity;
- machine serial or console logs;
- MTU and path quality;
- duplicate machine identities or stale resources.

HTTP tunneling may be available for networks that block UDP, but it adds overhead and should not be enabled without a network requirement.

## Download kubeconfig and talosconfig

Download and merge a cluster kubeconfig:

```bash
omnictl kubeconfig \
  --cluster <cluster-name> \
  --merge
```

Write to a specific path:

```bash
omnictl kubeconfig ./kubeconfig \
  --cluster <cluster-name> \
  --force
```

Download a cluster Talos configuration:

```bash
omnictl talosconfig \
  --cluster <cluster-name> \
  --merge
```

Download a generic Omni Talos configuration:

```bash
omnictl talosconfig ./talosconfig --force
```

> [!WARNING]
> Administrative kubeconfig and talosconfig files are sensitive credentials. Prefer identity-backed access, short-lived sessions, narrowly scoped service accounts, secure storage, and auditable workflows.

## Cluster templates

Omni cluster templates are YAML documents that declare cluster resources and can be stored in Git.

Common document kinds include:

- `Cluster`;
- `ControlPlane`;
- `Workers`;
- `Machine`.

A minimal conceptual template:

```yaml
kind: Cluster
name: <cluster-name>
kubernetes:
  version: <kubernetes-version>
talos:
  version: <talos-version>
features:
  diskEncryption: true
  backupConfiguration:
    interval: 1h
---
kind: ControlPlane
machines:
  - <machine-id-1>
  - <machine-id-2>
  - <machine-id-3>
---
kind: Workers
name: workers
machineClass:
  name: <machine-class>
  size: 3
```

Render and validate offline:

```bash
omnictl cluster template render \
  --file <cluster-template.yaml>
```

Apply through Omni:

```bash
omnictl apply \
  --file <rendered-resources.yaml> \
  --dry-run

omnictl apply \
  --file <rendered-resources.yaml>
```

Store patches, manifests, version choices, and machine-class intent with the template so the cluster can be reproduced and reviewed.

## Machine classes

Machine classes select machines by labels and desired count.

Use them for:

- automatically selecting available machines;
- dynamic infrastructure-provider capacity;
- worker pools with different hardware profiles;
- location, environment, role, or accelerator placement;
- scaling through declarative cluster templates.

Validate label selectors carefully. An overly broad class can allocate the wrong hardware or location to a production cluster.

## Infrastructure providers

Current documented provider types include:

- bare metal;
- Proxmox;
- vSphere;
- libvirt;
- KubeVirt.

An Omni instance can connect to multiple provider deployments. A provider commonly represents one location, management API, credential boundary, region, or network segment.

### Static providers

Static providers manage existing machines and can handle operations such as:

- power control;
- PXE boot;
- Talos provisioning;
- returning machines to an available pool.

### Dynamic providers

Dynamic providers create and destroy machines through another infrastructure API.

Operational controls:

- isolate provider credentials by location or platform;
- scope service accounts narrowly;
- document capacity and quota limits;
- monitor provisioning failures and stale instances;
- avoid deleting provider-managed machines out of band;
- test cluster deletion and failed-provision cleanup.

List configured providers:

```bash
omnictl infraprovider list
```

## Talos configuration overrides

Omni is the source of truth for critical Talos configuration, identity, certificates, endpoints, and connectivity. Some fields that work in standalone Talos are forbidden or ignored under Omni.

Use Omni-supported patches and template fields rather than directly editing Omni-owned values.

> [!CAUTION]
> A direct Talos configuration that is valid for standalone Talos may be rejected, ignored, or later reconciled by Omni.

## Cluster upgrades

Omni manages both Talos Linux and Kubernetes upgrades.

Before upgrading:

```bash
omnictl get clusters
omnictl get clusterstatuses
omnictl get etcdbackupstatus <cluster-name> -o yaml
```

Operational sequence:

1. Confirm the supported Talos and Kubernetes version path.
2. Confirm a recent successful etcd backup.
3. Confirm control-plane count and quorum.
4. Review disruption budgets and workload shutdown behavior.
5. Start the Talos or Kubernetes upgrade through Omni.
6. Monitor each machine and cluster status.
7. Validate nodes, system pods, networking, storage, and workloads.
8. Synchronize updated Kubernetes bootstrap manifests where required.

Omni upgrades control-plane nodes in a controlled sequence and checks etcd health. Do not bypass this process by deleting machines or Kubernetes nodes directly.

## Stalled upgrade troubleshooting

Inspect machine logs:

```bash
omnictl machine-logs <machine-id>
```

Inspect cluster and machine resources:

```bash
omnictl get clusterstatuses <cluster-name> -o yaml
omnictl get machines <machine-id> -o yaml
```

Use serial, IPMI, VNC, or hypervisor console access when the Talos API is unavailable.

During a stalled control-plane upgrade:

- do not delete the VM or bare-metal resource out of band;
- do not add control-plane nodes as a quorum repair attempt;
- do not run `kubectl delete node` against a control-plane node;
- do not continue upgrading other control-plane nodes;
- preserve logs and backup state before recovery.

## etcd backups

Inspect backup status:

```bash
omnictl get etcdbackupstatus <cluster-name> -o yaml
```

Confirm:

- the error field is empty;
- the last backup time is recent;
- backup storage is durable and protected;
- restoration has been tested;
- retention meets recovery objectives.

Backups are the primary recovery path for an unrecoverable control-plane failure.

## Cluster deletion

Preview deletion:

```bash
omnictl cluster delete <cluster-name> --dry-run --verbose
```

Delete the cluster:

```bash
omnictl cluster delete <cluster-name>
```

Handle disconnected machines only with full understanding of whether they still exist and contain data:

```bash
omnictl cluster delete <cluster-name> \
  --destroy-disconnected-machines
```

> [!DANGER]
> Cluster deletion can destroy managed virtual machines, release bare-metal machines, remove cluster credentials, and permanently eliminate workloads and data not stored externally.

## Users, service accounts, and access

Inspect users:

```bash
omnictl user list
```

Use service accounts for automation instead of sharing human credentials.

Access design should distinguish:

- Omni administrative access;
- Talos machine access;
- Kubernetes API access;
- workload service access;
- infrastructure-provider credentials;
- emergency break-glass access.

Integrate enterprise identity providers and map access to least-privilege roles. Review inactive users and service accounts regularly.

## Audit and support

Review audit events:

```bash
omnictl audit-log
```

Download a cluster support bundle:

```bash
omnictl support \
  --cluster <cluster-name> \
  --output <support-file>
```

Support bundles and audit logs can contain sensitive infrastructure, identity, and workload metadata. Protect them accordingly.

## Common failure patterns

| Symptom | Checks |
|---|---|
| Machine never appears in Omni | Join token, media or join config, SideroLink network path, DNS, time, console logs |
| Machine is connected but unavailable for a cluster | Allocation state, labels, machine class, existing cluster ownership |
| Cluster creation stalls | Control-plane count, installation disk, Talos image, provider state, API endpoint |
| `talosctl` direct access stops working | Machine is Omni-managed; use Omni-provided talosconfig and supported access path |
| Provider cannot create machines | Credentials, quotas, templates, network placement, provider logs |
| Upgrade stalls | Machine logs, console, backup status, quorum, image availability |
| Kubeconfig access fails | Identity session, role, cluster state, OIDC or service-account configuration |
| Machine remains disconnected | SideroLink reachability, token revocation, firewall, clock, machine identity |
| Template apply changes unexpected resources | Rendered diff, document names, selectors, machine classes, version fields |

## Troubleshooting workflow

1. Verify the active omniconfig context and identity.
2. Check Omni service health and account status.
3. Inspect cluster, machine, and provider resources.
4. Review machine and provider logs.
5. Confirm SideroLink connectivity, DNS, time, MTU, and firewall behavior.
6. Verify backup status before any destructive recovery.
7. Compare templates and patches with the resources currently stored in Omni.
8. Use console access for machines that cannot reach Omni.
9. Avoid out-of-band deletion of machines or control-plane nodes.
10. Apply the documented Omni recovery path and verify the full cluster afterward.

## References

- [Omni overview](https://docs.siderolabs.com/omni/overview/what-is-omni)
- [Omni CLI reference](https://docs.siderolabs.com/omni/reference/cli)
- [Join machines to Omni](https://docs.siderolabs.com/omni/omni-cluster-setup/registering-machines/join-machines-to-omni)
- [Cluster templates](https://docs.siderolabs.com/omni/reference/cluster-templates)
- [Infrastructure providers](https://docs.siderolabs.com/omni/infrastructure-and-extensions/infrastructure-providers)
- [Upgrade Omni clusters](https://docs.siderolabs.com/omni/cluster-management/upgrading-clusters)
- [Talos configuration overrides](https://docs.siderolabs.com/omni/cluster-management/talos-config-overrides)
- [Options for running Omni](https://docs.siderolabs.com/omni/self-hosted/options-for-running-omni)
