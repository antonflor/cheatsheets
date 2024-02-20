### GCP `gcloud` CLI Cheat Sheet

#### Introduction to GCP `gcloud` CLI
The `gcloud` command-line interface is a part of the Google Cloud SDK and provides a way to manage GCP services and resources.

- **Purpose**: Manage GCP resources, automate tasks, and handle administrative tasks.

---

#### Initial Setup and Configuration
**Install Google Cloud SDK**
- Installation varies by operating system and is available on the GCP documentation site.

**Initialize gcloud CLI**
- `gcloud init`
- Initializes the gcloud environment, sets up the default configuration, project, and authenticates user.

**Set Default Project**
- `gcloud config set project [PROJECT_ID]`
- Sets a default project for gcloud commands.

#### Managing Compute Engine
**List Compute Instances**
- `gcloud compute instances list`
- Lists all Compute Engine instances in the current project.

**Create Compute Instance**
- `gcloud compute instances create [INSTANCE_NAME] --zone [ZONE]`
- Creates a new Compute Engine instance in the specified zone.

**Stop Compute Instance**
- `gcloud compute instances stop [INSTANCE_NAME] --zone [ZONE]`
- Stops a running Compute Engine instance.

**Delete Compute Instance**
- `gcloud compute instances delete [INSTANCE_NAME] --zone [ZONE]`
- Deletes a specified Compute Engine instance.

#### Working with Kubernetes Engine
**List Kubernetes Clusters**
- `gcloud container clusters list`
- Lists all Kubernetes clusters in the current project.

**Create Kubernetes Cluster**
- `gcloud container clusters create [CLUSTER_NAME] --zone [ZONE]`
- Creates a new Kubernetes cluster in the specified zone.

**Get Kubernetes Cluster Credentials**
- `gcloud container clusters get-credentials [CLUSTER_NAME] --zone [ZONE]`
- Fetches credentials for interacting with the Kubernetes cluster.

**Delete Kubernetes Cluster**
- `gcloud container clusters delete [CLUSTER_NAME] --zone [ZONE]`
- Deletes a specified Kubernetes cluster.

#### Managing Cloud Storage
**List Storage Buckets**
- `gsutil ls`
- Lists all Cloud Storage buckets in the current project.

**Create Storage Bucket**
- `gsutil mb gs://[BUCKET_NAME]`
- Creates a new Cloud Storage bucket.

**Copy Files to/from Storage Bucket**
- `gsutil cp [LOCAL_FILE] gs://[BUCKET_NAME]/[OBJECT_NAME]`
- `gsutil cp gs://[BUCKET_NAME]/[OBJECT_NAME] [LOCAL_FILE]`
- Copies files to or from a Cloud Storage bucket.

#### Managing IAM and Service Accounts
**List IAM Roles**
- `gcloud iam roles list`
- Lists all available IAM roles in the current project.

**Create Service Account**
- `gcloud iam service-accounts create [ACCOUNT_NAME]`
- Creates a new service account.

**Assign IAM Role to Service Account**
- `gcloud projects add-iam-policy-binding [PROJECT_ID] --member "serviceAccount:[ACCOUNT_NAME]@[PROJECT_ID].iam.gserviceaccount.com" --role "[ROLE]"`
- Assigns an IAM role to a service account.

#### Networking and VPC
**List VPC Networks**
- `gcloud compute networks list`
- Lists all VPC networks in the current project.

**Create VPC Network**
- `gcloud compute networks create [NETWORK_NAME] --subnet-mode=custom`
- Creates a new VPC network with custom subnet mode.

**Create Firewall Rule**
- `gcloud compute firewall-rules create [RULE_NAME] --network [NETWORK_NAME] --allow [PROTOCOL]:[PORT]`
- Creates a firewall rule in the specified VPC network.

---

#### Tips for Using GCP `gcloud` CLI
- **Regular Updates**: Keep the Google Cloud SDK updated for the latest features and security updates.
- **Useful Flags**: Explore flags like `--format` to customize output and `--help` for command usage.
- **Scripting**: Integrate `gcloud` commands into scripts for automation and efficient cloud management.
