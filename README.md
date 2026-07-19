# Infrastructure Reference

Practical quick-reference notes for network engineering, Linux operations, cloud platforms, infrastructure automation, and troubleshooting.

> [!WARNING]
> These notes are a memory aid, not a substitute for vendor documentation, change review, backups, or a tested rollback plan. Commands that alter state can cause outages or data loss. Validate syntax against the exact software and platform release you operate.

## How to use this repository

- Browse references by domain under [`docs/`](docs/).
- Replace values such as `<interface>`, `<namespace>`, and `<resource-id>` before running commands.
- Start with read-only inspection commands and capture the current state.
- Treat `clear`, `delete`, `destroy`, `flush`, `reset`, `prune`, and `--force` operations as destructive.
- Prefer official documentation for release-specific behavior.
- Use [the visual guides](docs/visual-guides.md) for topology, lifecycle, state-transition, and troubleshooting diagrams.
- Open a content-correction issue when a command is obsolete, unsafe, ambiguous, or vendor-specific.

## Visual guides

The diagrams are intentionally limited to concepts where visual relationships improve understanding; command-oriented references remain text-first.

| Concept | Visual reference | Detailed reference |
|---|---|---|
| Spanning-tree root and alternate path | [Visual guide](docs/visual-guides.md#spanning-tree-root-and-alternate-path) | [Spanning Tree](docs/networking/spanning-tree.md) |
| Leaf-spine topology | [Visual guide](docs/visual-guides.md#leaf-spine-fabric) | [Leaf-spine design](docs/networking/leaf-spine.md) |
| Pulumi change lifecycle | [Visual guide](docs/visual-guides.md#pulumi-change-lifecycle) | [Pulumi](docs/automation/pulumi.md) |
| Linux boot sequence | [Visual guide](docs/visual-guides.md#linux-boot-sequence) | [Linux boot and kernel](docs/linux/linux-boot.md) |
| Git branch and pull-request workflow | [Visual guide](docs/visual-guides.md#git-branch-and-pull-request-workflow) | [Git](docs/development/git.md) |
| Operational troubleshooting sequence | [Visual guide](docs/visual-guides.md#operational-troubleshooting-sequence) | [Documentation style guide](STYLE_GUIDE.md) |

## Networking fundamentals

| Topic | Reference |
|---|---|
| Clos fabrics | [docs/networking/clos.md](docs/networking/clos.md) |
| Leaf-spine design | [docs/networking/leaf-spine.md](docs/networking/leaf-spine.md) |
| LLDP | [docs/networking/lldp.md](docs/networking/lldp.md) |
| OSI model | [docs/networking/osi-model.md](docs/networking/osi-model.md) |
| OSPF | [docs/networking/ospf.md](docs/networking/ospf.md) |
| Spanning Tree | [docs/networking/spanning-tree.md](docs/networking/spanning-tree.md) |
| TCP | [docs/networking/tcp.md](docs/networking/tcp.md) |
| UDP | [docs/networking/udp.md](docs/networking/udp.md) |

## Network platforms and routing software

| Platform | Reference |
|---|---|
| Arista EOS | [docs/network-platforms/arista-eos.md](docs/network-platforms/arista-eos.md) |
| BIRD | [docs/network-platforms/bird.md](docs/network-platforms/bird.md) |
| Cisco IOS / IOS XE | [docs/network-platforms/cisco-ios.md](docs/network-platforms/cisco-ios.md) |
| FortiGate | [docs/network-platforms/fortigate.md](docs/network-platforms/fortigate.md) |
| Junos | [docs/network-platforms/junos.md](docs/network-platforms/junos.md) |

## Cisco contact center

| Product | Reference |
|---|---|
| Cisco Unified Intelligence Center | [docs/cisco-contact-center/cuic.md](docs/cisco-contact-center/cuic.md) |
| Cisco Unified Customer Voice Portal | [docs/cisco-contact-center/cvp.md](docs/cisco-contact-center/cvp.md) |
| Cisco Intelligent Contact Management | [docs/cisco-contact-center/icm.md](docs/cisco-contact-center/icm.md) |

## Cloud platforms

| Platform | Reference |
|---|---|
| Amazon Web Services and AWS CLI | [docs/cloud/aws.md](docs/cloud/aws.md) |
| Microsoft Azure and Azure CLI | [docs/cloud/azure.md](docs/cloud/azure.md) |
| Google Cloud and Google Cloud CLI | [docs/cloud/google-cloud.md](docs/cloud/google-cloud.md) |

## Containers and orchestration

| Topic | Reference |
|---|---|
| Docker and Docker Compose | [docs/containers/docker.md](docs/containers/docker.md) |
| Kubernetes and kubectl | [docs/containers/kubernetes.md](docs/containers/kubernetes.md) |

## Infrastructure automation

| Topic | Reference |
|---|---|
| Pulumi | [docs/automation/pulumi.md](docs/automation/pulumi.md) |
| Terraform | [docs/automation/terraform.md](docs/automation/terraform.md) |
| Jenkins | [docs/automation/jenkins.md](docs/automation/jenkins.md) |
| Puppet | [docs/automation/puppet.md](docs/automation/puppet.md) |

## Linux and operations

| Topic | Reference |
|---|---|
| awk | [docs/linux/awk.md](docs/linux/awk.md) |
| Debian | [docs/linux/debian.md](docs/linux/debian.md) |
| HAProxy | [docs/linux/haproxy.md](docs/linux/haproxy.md) |
| iptables | [docs/linux/iptables.md](docs/linux/iptables.md) |
| Linux boot and kernel | [docs/linux/linux-boot.md](docs/linux/linux-boot.md) |
| nmap | [docs/linux/nmap.md](docs/linux/nmap.md) |
| Regular expressions | [docs/linux/regex.md](docs/linux/regex.md) |
| SCP | [docs/linux/scp.md](docs/linux/scp.md) |
| sed | [docs/linux/sed.md](docs/linux/sed.md) |
| Xen | [docs/linux/xen.md](docs/linux/xen.md) |

## High availability

| Topic | Reference |
|---|---|
| Corosync | [docs/high-availability/corosync.md](docs/high-availability/corosync.md) |
| Pacemaker | [docs/high-availability/pacemaker.md](docs/high-availability/pacemaker.md) |

## Development and data

| Topic | Reference |
|---|---|
| Git | [docs/development/git.md](docs/development/git.md) |
| Kafka | [docs/development/kafka.md](docs/development/kafka.md) |
| REST APIs | [docs/development/rest-api.md](docs/development/rest-api.md) |
| SQL | [docs/development/sql.md](docs/development/sql.md) |

## Repository layout

Reference material uses a shallow `docs/<domain>/<topic>.md` structure. Project-level files stay at the repository root:

```text
README.md
CONTRIBUTING.md
STYLE_GUIDE.md
LICENSE
docs/
```

Filenames use the canonical name engineers are likely to search for. Common, unambiguous acronyms remain short (`tcp.md`, `ospf.md`), while ambiguous concepts stay descriptive (`spanning-tree.md`, `osi-model.md`). Vendor references name the actual platform (`arista-eos.md`, `junos.md`) instead of only the company.

## Maintenance policy

Technically revalidated documents include an **Applies to** line and a **Last reviewed** date. Inherited version-sensitive pages that have not yet been revalidated are labeled **Legacy reference — technical validation pending** rather than receiving a misleading review date.

Repository quality checks validate project documentation and every Markdown file under `docs/` on pull requests and changes to `main`. See [CONTRIBUTING.md](CONTRIBUTING.md) and [STYLE_GUIDE.md](STYLE_GUIDE.md) before adding or substantially rewriting a sheet.

## License

Content is available under the [MIT License](LICENSE).
