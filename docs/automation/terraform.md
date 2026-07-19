# Terraform CLI Cheat Sheet

> **Applies to:** Terraform 1.x
> **Last reviewed:** 2026-07-14

A quick reference for formatting, validating, planning, applying, inspecting, and carefully repairing Terraform-managed infrastructure.

## Safety

> [!WARNING]
> Review every plan before applying it. State commands can change Terraform’s record without changing real infrastructure. Back up remote state and confirm locking before manual state repair.

## Daily workflow

```bash
terraform version
terraform fmt --recursive
terraform init
terraform validate
terraform plan -out=tfplan
terraform show tfplan
terraform apply tfplan
```

Use a saved plan when the reviewed plan must be exactly the one applied. Never commit plan files because they can contain sensitive values.

## Initialization and providers

```bash
terraform init
terraform init -upgrade
terraform providers
terraform providers schema -json
terraform providers lock \
  -platform=linux_amd64 \
  -platform=darwin_arm64
```

Commit `.terraform.lock.hcl`. Do not commit `.terraform/`.

## Planning

```bash
terraform plan
terraform plan -out=tfplan
terraform plan -refresh-only
terraform plan -destroy
terraform plan -var-file=<environment>.tfvars
terraform plan -target=<resource-address>
```

> [!CAUTION]
> `-target` is intended for exceptional recovery or focused troubleshooting. Re-run a full plan afterward to detect remaining drift.

## Applying and destroying

```bash
terraform apply
terraform apply tfplan
terraform apply -refresh-only
terraform destroy
```

Avoid `-auto-approve` in interactive production workflows unless a reviewed automation pipeline controls the inputs and plan.

## Outputs and inspection

```bash
terraform output
terraform output -json
terraform show
terraform show -json tfplan
terraform console
terraform graph
```

## Workspaces

```bash
terraform workspace show
terraform workspace list
terraform workspace new <name>
terraform workspace select <name>
terraform workspace delete <name>
```

CLI workspaces share a backend and configuration. They are not a complete isolation boundary for environments with different credentials, policies, or blast radii.

## State inspection and repair

```bash
terraform state list
terraform state show <resource-address>
terraform state pull > state-backup.json
terraform state mv <source-address> <destination-address>
terraform state rm <resource-address>
```

Before a manual state change:

1. stop concurrent runs;
2. confirm state locking;
3. save a state backup;
4. document the intended mapping;
5. run a full plan afterward.

## Import existing infrastructure

Terraform supports import blocks in configuration:

```hcl
import {
  to = <resource-address>
  id = "<provider-resource-id>"
}
```

Then run:

```bash
terraform plan
```

The legacy CLI form is still available:

```bash
terraform import <resource-address> <provider-resource-id>
```

Import adds an object to state; it does not automatically produce a complete, maintainable configuration.

## Replace a resource

Prefer a reviewable plan using `-replace`:

```bash
terraform plan -replace=<resource-address> -out=tfplan
terraform apply tfplan
```

`terraform taint` is deprecated. Do not teach it as the normal replacement workflow.

## Dependency and configuration troubleshooting

```bash
terraform fmt -check -recursive
terraform validate
terraform providers
terraform state list
terraform plan -refresh-only
TF_LOG=DEBUG terraform plan
```

Debug logs can contain sensitive values. Store and share them carefully, then unset logging:

```bash
unset TF_LOG
unset TF_LOG_PATH
```

## Locked state

Investigate the active lock owner before using:

```bash
terraform force-unlock <lock-id>
```

> [!DANGER]
> Forcing an active lock can allow concurrent state writes and corrupt state. Use it only after proving that the locking process is gone.

## Official references

- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
- [Terraform state commands](https://developer.hashicorp.com/terraform/cli/commands/state)
- [Replace resources](https://developer.hashicorp.com/terraform/cli/state/taint)
- [Import existing resources](https://developer.hashicorp.com/terraform/language/import)