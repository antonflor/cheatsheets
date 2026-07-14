# Google Cloud Cheat Sheet

> **Applies to:** Current Google Cloud services and Google Cloud CLI (`gcloud`)
> **Last reviewed:** 2026-07-14

A practical reference for Google Cloud resource hierarchy, common services, authentication, Compute Engine, Google Kubernetes Engine, Cloud Storage, IAM, and VPC networking.

> [!WARNING]
> Verify the active account, project, region, and zone before changing resources. Google Cloud commands often succeed against whichever project is currently selected, even when it is not the project you intended.

## Resource hierarchy and scope

| Scope | Purpose |
|---|---|
| Organization | Top-level administrative boundary for a company or domain |
| Folder | Optional grouping for departments, environments, or policy boundaries |
| Project | Primary billing, API, IAM, quota, and resource-management boundary |
| Region | Geographic area containing multiple zones |
| Zone | Deployment location within a region |

Projects have both a human-readable project ID and an internal project number. Many APIs and IAM relationships use one or the other, so verify which value a command expects.

## Common services

| Area | Services |
|---|---|
| Compute | Compute Engine, Cloud Run, App Engine, Cloud Functions |
| Containers | Google Kubernetes Engine (GKE), Artifact Registry |
| Storage | Cloud Storage, Persistent Disk, Filestore |
| Databases | Cloud SQL, Spanner, Firestore, Bigtable, Memorystore |
| Data and analytics | BigQuery, Dataflow, Dataproc, Pub/Sub |
| Networking | VPC, Cloud Load Balancing, Cloud NAT, Cloud DNS, Cloud CDN, Cloud VPN, Cloud Interconnect |
| Security | IAM, Secret Manager, Cloud KMS, Security Command Center |
| Operations | Cloud Logging, Cloud Monitoring, Error Reporting, Trace |
| Build and delivery | Cloud Build, Cloud Deploy, Artifact Registry |

## CLI setup and identity

```bash
gcloud version
gcloud init
gcloud auth login
gcloud auth list
gcloud config configurations list
gcloud config list
gcloud projects list
```

Show the active account and project:

```bash
gcloud auth list --filter=status:ACTIVE
gcloud config get-value project
gcloud config get-value compute/region
gcloud config get-value compute/zone
```

Set defaults:

```bash
gcloud config set project <project-id>
gcloud config set compute/region <region>
gcloud config set compute/zone <zone>
```

Use named configurations to separate environments or identities:

```bash
gcloud config configurations create <configuration-name>
gcloud config configurations activate <configuration-name>
gcloud config configurations describe <configuration-name>
gcloud config configurations delete <configuration-name>
```

## Application Default Credentials

Local user credentials for client libraries:

```bash
gcloud auth application-default login
```

Inspect the Application Default Credentials environment:

```bash
gcloud auth application-default print-access-token
```

> [!WARNING]
> `gcloud auth login` credentials and Application Default Credentials are separate. Do not assume that authenticating the CLI also configures every SDK or application.

Prefer workload identity, Workload Identity Federation, or service-account impersonation for automation instead of downloading long-lived service-account keys.

## Service-account impersonation

Print a short-lived access token:

```bash
gcloud auth print-access-token \
  --impersonate-service-account=<service-account-email>
```

Set impersonation for the active CLI configuration:

```bash
gcloud config set auth/impersonate_service_account <service-account-email>
gcloud config unset auth/impersonate_service_account
```

Verify the effective identity by testing a read-only command against the intended project before making changes.

## Enable and inspect APIs

```bash
gcloud services list --enabled
gcloud services list --available
gcloud services enable <service-name>.googleapis.com
gcloud services disable <service-name>.googleapis.com
```

> [!CAUTION]
> Disabling an API can interrupt dependent workloads or management operations. Identify active resources and dependencies first.

## Compute Engine

Inspect instances:

```bash
gcloud compute instances list
gcloud compute instances describe <instance> --zone=<zone>
gcloud compute machine-types list --zones=<zone>
```

Create and access an instance:

```bash
gcloud compute instances create <instance> \
  --zone=<zone> \
  --machine-type=<machine-type> \
  --image-family=debian-12 \
  --image-project=debian-cloud

gcloud compute ssh <instance> --zone=<zone>
```

Lifecycle operations:

```bash
gcloud compute instances stop <instance> --zone=<zone>
gcloud compute instances start <instance> --zone=<zone>
gcloud compute instances reset <instance> --zone=<zone>
gcloud compute instances delete <instance> --zone=<zone>
```

> [!DANGER]
> Deleting an instance can also delete attached disks when their auto-delete setting is enabled. Inspect disk attachments and backup requirements first.

## Google Kubernetes Engine

```bash
gcloud container clusters list
gcloud container clusters describe <cluster> --location=<region-or-zone>
gcloud container clusters get-credentials <cluster> \
  --location=<region-or-zone>
```

Create and resize a cluster:

```bash
gcloud container clusters create <cluster> \
  --location=<region-or-zone> \
  --num-nodes=<count>

gcloud container clusters resize <cluster> \
  --location=<region-or-zone> \
  --num-nodes=<count>
```

Inspect available upgrades:

```bash
gcloud container get-server-config --location=<region-or-zone>
gcloud container clusters describe <cluster> \
  --location=<region-or-zone> \
  --format='yaml(currentMasterVersion,currentNodeVersion,releaseChannel)'
```

Delete a cluster:

```bash
gcloud container clusters delete <cluster> \
  --location=<region-or-zone>
```

> [!DANGER]
> Cluster deletion removes the Kubernetes control plane and node pools. Confirm persistent-volume retention, load balancers, DNS, and external dependencies first.

## Cloud Storage

Use `gcloud storage` for new scripts:

```bash
gcloud storage buckets list
gcloud storage buckets describe gs://<bucket>
gcloud storage buckets create gs://<bucket> --location=<location>
gcloud storage ls gs://<bucket>
gcloud storage cp <local-file> gs://<bucket>/<object>
gcloud storage cp gs://<bucket>/<object> <local-path>
gcloud storage rsync --recursive <local-directory> gs://<bucket>/<prefix>
gcloud storage rm gs://<bucket>/<object>
```

Inspect object versions and retention before bulk deletion:

```bash
gcloud storage ls --all-versions gs://<bucket>/<prefix>
gcloud storage buckets describe gs://<bucket> \
  --format='yaml(versioning,retentionPolicy,lifecycle)'
```

`gsutil` remains available for compatibility, but new automation should generally use `gcloud storage` unless a required feature is unavailable.

## IAM and service accounts

```bash
gcloud iam roles list
gcloud iam service-accounts list
gcloud iam service-accounts describe <service-account-email>
gcloud iam service-accounts create <name> \
  --display-name='<display-name>'
```

Inspect project policy:

```bash
gcloud projects get-iam-policy <project-id>
gcloud projects get-iam-policy <project-id> \
  --format='table(bindings.role,bindings.members)'
```

Add or remove a binding:

```bash
gcloud projects add-iam-policy-binding <project-id> \
  --member='serviceAccount:<service-account-email>' \
  --role='roles/<role>'

gcloud projects remove-iam-policy-binding <project-id> \
  --member='serviceAccount:<service-account-email>' \
  --role='roles/<role>'
```

Avoid broad primitive roles such as Owner, Editor, and Viewer for routine access. Prefer predefined or custom roles with the minimum permissions required.

## VPC networking

List and inspect networks:

```bash
gcloud compute networks list
gcloud compute networks describe <network>
gcloud compute networks subnets list
gcloud compute routes list --filter='network:<network>'
```

Create a custom-mode VPC and subnet:

```bash
gcloud compute networks create <network> --subnet-mode=custom

gcloud compute networks subnets create <subnet> \
  --network=<network> \
  --region=<region> \
  --range=<cidr>
```

Firewall policy inspection:

```bash
gcloud compute firewall-rules list
gcloud compute firewall-rules describe <rule>
```

Create an ingress rule:

```bash
gcloud compute firewall-rules create <rule> \
  --network=<network> \
  --direction=INGRESS \
  --action=ALLOW \
  --rules=tcp:<port> \
  --source-ranges=<cidr> \
  --target-tags=<network-tag>
```

Avoid `0.0.0.0/0` and `::/0` unless public exposure is explicitly required and protected by additional controls.

## Cloud Run

```bash
gcloud run services list --region=<region>
gcloud run services describe <service> --region=<region>
gcloud run deploy <service> \
  --image=<registry>/<project>/<repository>/<image>:<tag> \
  --region=<region>
gcloud run services update-traffic <service> \
  --to-latest \
  --region=<region>
```

Inspect IAM before enabling unauthenticated access.

## Cloud SQL

```bash
gcloud sql instances list
gcloud sql instances describe <instance>
gcloud sql databases list --instance=<instance>
gcloud sql users list --instance=<instance>
gcloud sql backups list --instance=<instance>
```

Delete only after confirming final backups and dependent applications:

```bash
gcloud sql instances delete <instance>
```

## Logging and monitoring

```bash
gcloud logging logs list
gcloud logging read '<filter>' --limit=<count> --freshness=<duration>
gcloud monitoring policies list
gcloud monitoring channels list
```

Examples:

```bash
gcloud logging read \
  'resource.type="gce_instance" severity>=ERROR' \
  --limit=50 \
  --freshness=1h
```

## Output formatting and filtering

```bash
gcloud <group> <command> --help
gcloud topic filters
gcloud topic formats
gcloud info
```

Useful formats:

```bash
gcloud compute instances list --format='table(name,zone.basename(),status)'
gcloud projects list --format='value(projectId)'
gcloud compute instances list --filter='status=RUNNING'
```

## Troubleshooting checklist

1. Verify active identity with `gcloud auth list`.
2. Verify project, region, and zone with `gcloud config list`.
3. Confirm the required API is enabled.
4. Check IAM permissions and organization policies.
5. Confirm quota and regional capacity.
6. Use `--verbosity=info` or `--verbosity=debug` temporarily.
7. Inspect Cloud Audit Logs for denied or changed operations.
8. Compare the command's fully expanded resource scope with the intended target.

```bash
gcloud info
gcloud config list
gcloud services list --enabled
gcloud projects get-iam-policy <project-id>
gcloud logging read 'protoPayload.status.code!=0' --limit=50
```

Debug output can contain request details and identifiers. Sanitize it before sharing.

## References

- [Google Cloud CLI reference](https://cloud.google.com/sdk/gcloud/reference)
- [Authenticate for the Google Cloud CLI](https://cloud.google.com/sdk/docs/authorizing)
- [Google Cloud products and services](https://cloud.google.com/products)
- [Cloud Storage with the gcloud CLI](https://cloud.google.com/storage/docs/discover-object-storage-gcloud)
- [Service-account impersonation](https://cloud.google.com/docs/authentication/use-service-account-impersonation)
