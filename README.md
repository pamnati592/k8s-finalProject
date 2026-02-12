# Todo List Kubernetes Project

Full-stack Todolist application deployed on Kubernetes using Helm.

**Student:** Pam-Netanel (211878558)

## Architecture

- **Frontend** - Vue.js web app (LoadBalancer Service)
- **Backend** - Node.js REST API (ClusterIP Service)
- **Database** - MariaDB (StatefulSet + Headless Service)

## Prerequisites

- Kubernetes cluster (Docker Desktop / Minikube)
- Helm 3.x
- kubectl

## Install from GHCR

```bash
helm install my-todo-app oci://ghcr.io/pamnati592/k8s-finalproject/todo-chart --set secret.rootPassword=YOUR_PASSWORD
```

Or from local source:

```bash
helm install my-todo-app ./todo-chart --set secret.rootPassword=YOUR_PASSWORD
```

## Access the Application

1. Start backend port-forward:
```bash
kubectl port-forward svc/todo-api 3001:8080
```

2. Start frontend port-forward:
```bash
kubectl port-forward svc/todolist-vue 8080:80
```

3. Open http://localhost:8080

## Configurable Values

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount.frontend` | Frontend replicas | `1` |
| `replicaCount.backend` | Backend replicas | `1` |
| `service.frontendType` | Frontend service type | `LoadBalancer` |
| `database.mysqlHost` | Database host | `backend` |
| `secret.rootPassword` | MySQL root password | `""` |
| `apiBaseUrl` | API base URL for frontend | `http://localhost:3001/todos` |
| `user` | Application user name | `Pam-Netanel` |

## Kubernetes Resources

- 2 Deployments (frontend, backend) with InitContainers, Probes, and Resource Limits
- 1 StatefulSet (MariaDB) with Headless Service
- ConfigMap for environment variables
- Secret for database credentials
- NetworkPolicy restricting DB access to backend only
- PersistentVolumeClaim for database storage

## Uninstall

```bash
helm uninstall my-todo-app
```
