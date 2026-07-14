# Pulumi Cheat Sheet

> **Applies to:** Pulumi CLI 3.x and Pulumi Infrastructure as Code
> **Last reviewed:** 2026-07-14

A practical reference for projects, stacks, configuration, previews, deployments, drift reconciliation, imports, state recovery, and CI usage.

> [!WARNING]
> Pulumi commands operate against the currently selected stack. Run `pulumi stack` and verify the organization, project, stack, cloud account, and region before changing infrastructure.

## Core concepts

| Concept | Meaning |
|---|---|
| Project | A Pulumi program described by `Pulumi.yaml` |
| Stack | An isolated instance of a project, such as `dev`, `stage`, or `prod` |
| Resource | A cloud, Kubernetes, SaaS, or custom object managed by Pulumi |
| Configuration | Per-stack values stored in `Pulumi.<stack>.yaml` |
| Secret | An encrypted configuration value or output |
| State | Pulumi's record of managed resources and their relationships |
| Backend | Pulumi Cloud or a self-managed object-storage/filesystem backend |

## Installation and identity

```bash
pulumi version
pulumi about
pulumi login
pulumi whoami
```

Log in to a self-managed backend:

```bash
pulumi login s3://<bucket>/<prefix>
pulumi login azblob://<container>/<prefix>
pulumi login gs://<bucket>/<prefix>
pulumi login file://<absolute-path>
```

Do not casually switch backends. Confirm that the intended stacks exist in the destination backend before running an update.

## Create a project

```bash
pulumi new
pulumi new <template>
pulumi new <template> --name <project-name> --stack <stack-name>
```

Common templates can target AWS, Azure, Google Cloud, Kubernetes, TypeScript, Python, Go, .NET, Java, or YAML.

## Stack management

```bash
pulumi stack ls
pulumi stack
pulumi stack select <stack>
pulumi stack init <stack>
pulumi stack rename <new-name>
pulumi stack history
pulumi stack output
pulumi stack output <output-name>
```

Fully qualified stack names can use this form:

```text
<organization>/<project>/<stack>
```

Display a secret output only when necessary:

```bash
pulumi stack output <output-name> --show-secrets
```

> [!WARNING]
> `--show-secrets` writes decrypted values to the terminal and potentially to shell history, CI logs, or screen recordings.

## Configuration and secrets

```bash
pulumi config
pulumi config get <key>
pulumi config set <key> <value>
pulumi config set <key> <value> --secret
pulumi config rm <key>
pulumi config refresh
```

Set structured values:

```bash
pulumi config set --path 'network.subnets[0].cidr' '10.10.1.0/24'
pulumi config set --path 'network.subnets[1].cidr' '10.10.2.0/24'
```

Copy configuration between stacks only after reviewing the destination:

```bash
pulumi config cp --dest <destination-stack>
```

Prefer workload identity, OpenID Connect, service-account impersonation, or short-lived credentials over long-lived cloud keys.

## Preview changes

```bash
pulumi preview
pulumi preview --diff
pulumi preview --refresh
pulumi preview --save-plan <plan-file>
```

A preview should be the normal first step for production changes. Review creates, replacements, deletes, provider changes, and unexpected diffs.

Exit with a nonzero status when changes are present:

```bash
pulumi preview --expect-no-changes
```

This is useful for drift or policy checks, but it will also fail for intentional pending changes.

## Deploy changes

```bash
pulumi up
pulumi up --diff
pulumi up --plan <plan-file>
```

Noninteractive CI usage:

```bash
pulumi up --yes --non-interactive
```

> [!WARNING]
> Do not use `--yes` interactively as a convenience flag. It removes the final confirmation barrier and is primarily intended for controlled automation.

Target a specific resource only for exceptional recovery or migration work:

```bash
pulumi up --target <resource-urn>
pulumi up --target-dependents --target <resource-urn>
```

Targeted updates can leave the stack partially converged. Follow with a full preview and update.

## Refresh and drift

Inspect differences between Pulumi state and the provider:

```bash
pulumi refresh --preview-only
pulumi preview --refresh
```

Reconcile Pulumi state with the provider:

```bash
pulumi refresh
```

> [!WARNING]
> `pulumi refresh` changes Pulumi state to match the provider. Review the preview carefully so that accidental out-of-band changes are not silently accepted.

## Import existing resources

Interactive import:

```bash
pulumi import
```

Import one resource:

```bash
pulumi import <type-token> <logical-name> <provider-resource-id>
```

Generate an import file, review it, and then run:

```bash
pulumi import --file <import-file.json>
```

After import, add the generated resource definition to the Pulumi program and run `pulumi preview` to ensure there is no replacement or unexpected update.

## State backup and recovery

Export state before manual state work:

```bash
pulumi stack export --file <stack>-backup.json
```

Restore a reviewed export:

```bash
pulumi stack import --file <stack>-backup.json
```

Inspect state operations:

```bash
pulumi state --help
pulumi state delete <resource-urn>
pulumi state unprotect <resource-urn>
pulumi state repair
```

> [!CAUTION]
> State commands bypass normal resource lifecycle behavior. Export a backup first, understand the resource dependencies, and use them only for recovery or carefully planned migrations.

## Resource protection

Resources created with the `protect` option cannot be deleted until protection is removed. For an emergency state-only change:

```bash
pulumi state unprotect <resource-urn>
```

Prefer changing the program and deploying normally when possible.

## Logs and troubleshooting

```bash
pulumi logs
pulumi logs --follow
pulumi about
pulumi plugin ls
pulumi install
pulumi stack history
pulumi cancel
```

Use `pulumi cancel` only when an update is stuck and no active engine process should continue. A canceled update may require `pulumi refresh` or state repair.

Enable diagnostic verbosity temporarily:

```bash
pulumi preview --logtostderr -v=3
```

Higher verbosity can expose provider requests or sensitive values. Sanitize logs before sharing them.

## Destroy and remove a stack

Preview destruction:

```bash
pulumi destroy --preview-only
```

Destroy managed resources:

```bash
pulumi destroy
```

Remove the empty stack record:

```bash
pulumi stack rm <stack>
```

> [!DANGER]
> `pulumi destroy` deletes managed infrastructure. Verify the selected stack and cloud identity, inspect protected resources, confirm backups, and understand retained external dependencies before proceeding.

## CI/CD baseline

Typical pull-request validation:

```bash
pulumi preview --non-interactive --diff
```

Typical controlled deployment:

```bash
pulumi stack select <organization>/<project>/<stack>
pulumi up --yes --non-interactive
```

Recommended controls:

- use short-lived federated cloud credentials;
- scope the Pulumi access token to the required organization and environment;
- serialize updates to the same stack;
- require preview review or policy checks for production;
- avoid printing secret outputs;
- retain update history and deployment logs;
- run a post-deployment preview or health check.

## References

- [Pulumi CLI command reference](https://www.pulumi.com/docs/iac/cli/commands/)
- [Pulumi stacks](https://www.pulumi.com/docs/iac/concepts/stacks/)
- [Pulumi configuration](https://www.pulumi.com/docs/iac/concepts/config/)
- [Pulumi state and backends](https://www.pulumi.com/docs/iac/concepts/state-and-backends/)
- [Pulumi import](https://www.pulumi.com/docs/iac/adopting-pulumi/import/)
