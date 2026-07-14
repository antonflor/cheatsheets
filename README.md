# Infrastructure and Network Engineering Cheatsheets

Practical quick-reference notes for network engineering, Linux operations, cloud platforms, infrastructure automation, and troubleshooting.

> [!WARNING]
> These notes are a memory aid, not a substitute for vendor documentation, change review, backups, or a tested rollback plan. Commands that alter state can cause outages or data loss. Validate syntax against the exact software and platform release you operate.

## How to use this repository

- Replace values such as `<interface>`, `<namespace>`, and `<resource-id>` before running commands.
- Start with read-only inspection commands and capture the current state.
- Treat `clear`, `delete`, `destroy`, `flush`, `reset`, `prune`, and `--force` operations as destructive.
- Prefer official documentation for release-specific behavior.
- Open a content-correction issue when a command is obsolete, unsafe, ambiguous, or vendor-specific.

## Networking fundamentals

| Topic | Reference |
|---|---|
| Clos fabrics | [clos.md](clos.md) |
| Leaf-spine design | [leafspine.md](leafspine.md) |
| LLDP | [lldp.md](lldp.md) |
| OSI model | [osi.md](osi.md) |
| OSPF | [ospf.md](ospf.md) |
| TCP | [tcp.md](tcp.md) |
| UDP | [udp.md](udp.md) |

## Network platforms and routing software

| Platform | Reference |
|---|---|
| Arista EOS | [arista.md](arista.md) |
| BIRD | [bird.md](bird.md) |
| Cisco IOS / IOS XE | [cisco.md](cisco.md) |
| Fortinet FortiGate | [fortinet.md](fortinet.md) |
| Juniper Junos | [juniper.md](juniper.md) |

## Cisco contact center

| Product | Reference |
|---|---|
| Cisco Unified Intelligence Center | [cisco-cuic.md](cisco-cuic.md) |
| Cisco Unified Customer Voice Portal | [cisco-cvp.md](cisco-cvp.md) |
| Cisco Intelligent Contact Management | [cisco-icm.md](cisco-icm.md) |

## Cloud platforms

| Topic | Reference |
|---|---|
| AWS services | [awscloud.md](awscloud.md) |
| AWS CLI | [awscli.md](awscli.md) |
| Microsoft Azure and Azure CLI | [azure.md](azure.md) |
| Google Cloud services | [gcpcloud.md](gcpcloud.md) |
| Google Cloud CLI | [gcloud.md](gcloud.md) |

## Containers, orchestration, and infrastructure as code

| Topic | Reference |
|---|---|
| Docker and Docker Compose | [docker.md](docker.md) |
| Kubernetes and kubectl | [kubernetes.md](kubernetes.md) |
| Terraform | [terraform.md](terraform.md) |
| Jenkins CI/CD | [jenkins-cicd.md](jenkins-cicd.md) |
| Puppet | [puppet.md](puppet.md) |

## Linux and operations

| Topic | Reference |
|---|---|
| awk | [awk.md](awk.md) |
| Debian | [debian.md](debian.md) |
| HAProxy | [haproxy.md](haproxy.md) |
| iptables | [iptables.md](iptables.md) |
| Linux boot process | [linux_kernel_boot.md](linux_kernel_boot.md) |
| nmap | [nmap.md](nmap.md) |
| Pacemaker | [pacemaker.md](pacemaker.md) |
| Corosync | [corosync.md](corosync.md) |
| Regular expressions | [regex.md](regex.md) |
| SCP | [scp.md](scp.md) |
| sed | [sed.md](sed.md) |
| Xen | [xen.md](xen.md) |

## Development and data

| Topic | Reference |
|---|---|
| Git | [github.md](github.md) |
| Kafka | [kafka.md](kafka.md) |
| REST APIs | [rest-api.md](rest-api.md) |
| SQL | [sql.md](sql.md) |

## Maintenance policy

Accuracy-sensitive documents should include an **Applies to** line and a **Last reviewed** date. A review date means the examples received a documentation review; it does not guarantee compatibility with every release or environment.

Repository quality checks validate the maintained documentation surface on pull requests and changes to `main`. See [CONTRIBUTING.md](CONTRIBUTING.md) and [STYLE_GUIDE.md](STYLE_GUIDE.md) before adding or substantially rewriting a sheet.

## License

Content is available under the [MIT License](LICENSE).
