# Kubernetes and kubectl Cheat Sheet

> **Applies to:** Current supported Kubernetes releases
> **Last reviewed:** 2026-07-14

A quick reference for cluster context, workloads, logs, rollouts, node maintenance, and troubleshooting.

## Context and namespaces

```bash
kubectl version --client
kubectl cluster-info
kubectl config current-context
kubectl config get-contexts
kubectl config use-context <context>
kubectl config set-context --current --namespace=<namespace>
kubectl get namespaces
```

## Discover resources

```bash
kubectl api-resources
kubectl api-versions
kubectl explain deployment
kubectl explain deployment.spec.template.spec.containers
```

## List and inspect

```bash
kubectl get nodes -o wide
kubectl get pods -A -o wide
kubectl get deployments,statefulsets,daemonsets -A
kubectl get services,ingresses -A
kubectl get events -A --sort-by=.metadata.creationTimestamp
kubectl describe pod <pod> -n <namespace>
kubectl get pod <pod> -n <namespace> -o yaml
```

## Apply and diff

```bash
kubectl diff -f <manifest-or-directory>
kubectl apply -f <manifest-or-directory>
kubectl delete -f <manifest-or-directory>
kubectl apply --server-side -f <manifest-or-directory>
```

Prefer declarative manifests over imperative production changes. Run `kubectl diff` before applying when possible.

## Logs and process access

```bash
kubectl logs <pod> -n <namespace>
kubectl logs <pod> -n <namespace> -c <container>
kubectl logs <pod> -n <namespace> --previous
kubectl logs -f <pod> -n <namespace> --since=10m
kubectl exec -it <pod> -n <namespace> -- /bin/sh
kubectl cp <namespace>/<pod>:/remote/path ./local-path
```

## Rollouts and scaling

```bash
kubectl rollout status deployment/<name> -n <namespace>
kubectl rollout history deployment/<name> -n <namespace>
kubectl rollout undo deployment/<name> -n <namespace>
kubectl scale deployment/<name> -n <namespace> --replicas=<count>
kubectl set image deployment/<name> \
  <container>=<image>:<tag> \
  -n <namespace>
```

## Port forwarding and temporary diagnostics

```bash
kubectl port-forward service/<service> 8080:<service-port> -n <namespace>
kubectl port-forward pod/<pod> 8080:<pod-port> -n <namespace>
kubectl run net-debug \
  --rm -it \
  --restart=Never \
  --image=nicolaka/netshoot \
  -- /bin/bash
kubectl debug node/<node> -it --image=ubuntu
```

Use only trusted diagnostic images approved by your organization.

## Resource usage

```bash
kubectl top nodes
kubectl top pods -A
kubectl top pod <pod> -n <namespace> --containers
```

The Metrics API must be available for `kubectl top`.

## Node maintenance

```bash
kubectl cordon <node>
kubectl drain <node> \
  --ignore-daemonsets \
  --delete-emptydir-data
kubectl uncordon <node>
```

> [!WARNING]
> `drain` evicts workloads and `--delete-emptydir-data` discards ephemeral data. Check PodDisruptionBudgets, local storage, singleton workloads, and replacement capacity first.

## Labels, annotations, and taints

```bash
kubectl label node <node> <key>=<value>
kubectl label node <node> <key>-
kubectl annotate <resource>/<name> <key>=<value> -n <namespace>
kubectl taint node <node> <key>=<value>:NoSchedule
kubectl taint node <node> <key>:NoSchedule-
```

## JSONPath and custom columns

```bash
kubectl get pods -A \
  -o custom-columns='NS:.metadata.namespace,NAME:.metadata.name,NODE:.spec.nodeName,PHASE:.status.phase'

kubectl get nodes \
  -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.nodeInfo.kubeletVersion}{"\n"}{end}'
```

## Common troubleshooting sequence

```bash
kubectl get pod <pod> -n <namespace> -o wide
kubectl describe pod <pod> -n <namespace>
kubectl logs <pod> -n <namespace> --all-containers --previous
kubectl get events -n <namespace> --sort-by=.metadata.creationTimestamp
kubectl get endpoints,endpointslices -n <namespace>
kubectl get networkpolicies -n <namespace>
```

Check scheduling events, image pulls, probes, resource limits, service selectors, endpoint population, DNS, and network policy before restarting workloads blindly.

## References

- [kubectl reference](https://kubernetes.io/docs/reference/kubectl/)
- [Debug applications](https://kubernetes.io/docs/tasks/debug/debug-application/)
- [Safely drain a node](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/)
