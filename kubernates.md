# Kubernetes Command Cheat Sheet

This cheat sheet is a comprehensive guide to Kubernetes commands, designed for DevOps engineers, system administrators, and anyone working with Kubernetes. Kubernetes is a powerful container orchestration system that automates the deployment, scaling, and management of containerized applications.

The commands are categorized into sections for easy reference, covering everything from basic interactions to more advanced management and troubleshooting. This guide aims to streamline Kubernetes operations and provide quick access to common commands necessary for effective cluster management and deployment troubleshooting.

## Basic Interactions
- **kubectl get nodes**
  - Lists all nodes in the cluster.

- **kubectl get pods**
  - Lists all pods in the current namespace.

- **kubectl get pods -n [namespace]**
  - Lists all pods in a specific namespace.

- **kubectl get services**
  - Lists all services in the current namespace.

- **kubectl get deployments**
  - Lists all deployments in the current namespace.

- **kubectl describe node [node_name]**
  - Shows detailed information about a node.

- **kubectl describe pod [pod_name]**
  - Shows detailed information about a pod.

## Resource Management
- **kubectl create -f [file.yaml]**
  - Creates a resource from a YAML file.

- **kubectl apply -f [file.yaml]**
  - Applies changes to a resource from a YAML file.

- **kubectl delete -f [file.yaml]**
  - Deletes a resource defined in a YAML file.

- **kubectl scale deployment [deployment_name] --replicas=[num]**
  - Scales a deployment to a specific number of replicas.

- **kubectl rollout status deployment/[deployment_name]**
  - Checks the rollout status of a deployment.

- **kubectl set image deployment/[deployment_name] [container_name]=[image:tag]**
  - Updates the image of a deployment.

- **kubectl exec -it [pod_name] -- [command]**
  - Executes a command in a running pod.

- **kubectl logs [pod_name]**
  - Fetches logs from a pod.

- **kubectl port-forward [pod_name] [local_port]:[pod_port]**
  - Forwards a local port to a port on the pod.

## Cluster Administration
- **kubectl cluster-info**
  - Displays cluster info.

- **kubectl top node**
  - Shows metrics for nodes.

- **kubectl top pod**
  - Shows metrics for pods.

- **kubectl cordon [node_name]**
  - Marks a node as unschedulable.

- **kubectl drain [node_name]**
  - Drains a node in preparation for maintenance.

- **kubectl uncordon [node_name]**
  - Marks a node as schedulable.

- **kubectl taint nodes [node_name] key=value:effect**
  - Applies a taint to a node.

- **kubectl edit [resource] [name]**
  - Edits a resource in the default editor.

## Networking and Debugging
- **kubectl run [pod_name] --image=[image]**
  - Runs a temporary pod with a specific image.

- **kubectl attach [pod_name] -i**
  - Attaches to a running container.

- **kubectl expose deployment [deployment_name] --type=[type] --port=[port]**
  - Exposes a deployment as a new Kubernetes service.

- **kubectl get events**
  - Lists cluster events, useful for debugging.

- **kubectl get namespaces**
  - Lists all namespaces in the cluster.

- **kubectl config view**
  - Shows the Kubernetes configuration.
