# Google Cloud CLI Cheat Sheet

> **Applies to:** Current Google Cloud CLI (`gcloud`)
> **Last reviewed:** 2026-07-14

A practical reference for authentication, configuration, Compute Engine, GKE, Cloud Storage, IAM, and networking.

## Authentication and configuration

```bash
gcloud version
gcloud init
gcloud auth login
gcloud auth list
gcloud config configurations list
gcloud config list
gcloud config set project <project-id>
gcloud config set compute/region <region>
gcloud config set compute/zone <zone>
gcloud projects list
```

For local Application Default Credentials used by client libraries:

```bash
gcloud auth application-default login
```

> [!WARNING]
> Do not confuse user CLI credentials with Application Default Credentials. Use workload identity or service-account impersonation for automation instead of downloading long-lived keys whenever possible.

## Service-account impersonation

```bash
gcloud auth print-access-token \
  --impersonate-service-account=<service-account-email>

gcloud config set auth/impersonate_service_account <service-account-email>
gcloud config unset auth/impersonate_service_account
```

## Compute Engine

```bash
gcloud compute instances list
gcloud compute instances describe <instance> --zone=<zone>
gcloud compute instances create <instance> \
  --zone=<zone> \
  --machine-type=<machine-type> \
  --image-family=debian-12 \
  --image-project=debian-cloud
gcloud compute ssh <instance> --zone=<zone>
gcloud compute instances stop <instance> --zone=<zone>
gcloud compute instances start <instance> --zone=<zone>
gcloud compute instances delete <instance> --zone=<zone>
```

## Google Kubernetes Engine

```bash
gcloud container clusters list
gcloud container clusters describe <cluster> --location=<region-or-zone>
gcloud container clusters get-credentials <cluster> \
  --location=<region-or-zone>
gcloud container clusters create <cluster> \
  --location=<region-or-zone> \
  --num-nodes=3
gcloud container clusters resize <cluster> \
  --location=<region-or-zone> \
  --num-nodes=<count>
gcloud container clusters delete <cluster> \
  --location=<region-or-zone>
```

## Cloud Storage

Use the current `gcloud storage` command group for new scripts:

```bash
gcloud storage buckets list
gcloud storage buckets create gs://<bucket> --location=<location>
gcloud storage ls gs://<bucket>
gcloud storage cp <local-file> gs://<bucket>/<object>
gcloud storage cp gs://<bucket>/<object> <local-path>
gcloud storage rsync --recursive <local-directory> gs://<bucket>/<prefix>
gcloud storage rm gs://<bucket>/<object>
```

`gsutil` remains available in many installations, but do not introduce it into new automation unless compatibility requires it.

## IAM and service accounts

```bash
gcloud iam roles list
gcloud iam service-accounts list
gcloud iam service-accounts create <name> \
  --display-name="<display-name>"
gcloud projects get-iam-policy <project-id>
gcloud projects add-iam-policy-binding <project-id> \
  --member="serviceAccount:<service-account-email>" \
  --role="roles/<role>"
```

Avoid project-owner and broad primitive roles. Prefer predefined or custom roles with the minimum required permissions.

## VPC networking

```bash
gcloud compute networks list
gcloud compute networks describe <network>
gcloud compute networks create <network> --subnet-mode=custom
gcloud compute networks subnets list
gcloud compute networks subnets create <subnet> \
  --network=<network> \
  --region=<region> \
  --range=<cidr>
gcloud compute firewall-rules list
gcloud compute firewall-rules create <rule> \
  --network=<network> \
  --direction=INGRESS \
  --action=ALLOW \
  --rules=tcp:<port> \
  --source-ranges=<cidr>
```

## Output and troubleshooting

```bash
gcloud <group> <command> --help
gcloud info
gcloud topic formats
gcloud compute instances list \
  --format='table(name,zone.basename(),status,networkInterfaces[0].networkIP)'
gcloud components update
```

## References

- [Google Cloud CLI documentation](https://cloud.google.com/sdk/gcloud)
- [Cloud Storage with the gcloud CLI](https://cloud.google.com/storage/docs/discover-object-storage-gcloud)
- [Service-account impersonation](https://cloud.google.com/docs/authentication/use-service-account-impersonation)
