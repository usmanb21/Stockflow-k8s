# StockFlow Kubernetes Deployment 🚀

Production-style Kubernetes deployment of the StockFlow ASP.NET Core Web API with SQL Server, running on a local Docker Desktop cluster.

## 🏗️ Architecture

![Architecture](docs/stockflow-k8s-architecture.svg)

```
Client
  ↓
Kubernetes Service (LoadBalancer)
  ↓
API Pod (inventory-api)
  ↓
SQL Server Pod
```

## ⚙️ Tech Stack

|Layer        |Technology                        |
|-------------|----------------------------------|
|Orchestration|Kubernetes (Docker Desktop)       |
|API          |ASP.NET Core 8 (Docker Container) |
|Database     |SQL Server 2022 (Docker Container)|
|Config       |Kubernetes Secrets & ConfigMaps   |
|Networking   |Kubernetes Services               |

## 📁 Project Structure

```
k8s/
├── namespace.yaml       # inventory-app namespace
├── deployment.yaml      # API deployment config
├── service.yaml         # Service exposure
├── secret.yaml          # Database credentials (gitignored)
└── sqlserver.yaml       # SQL Server deployment
└── ingress.yaml         # NGINEX ingress configuration
```

## 🚀 Getting Started

### Prerequisites

- Docker Desktop with Kubernetes enabled
- kubectl CLI

### Enable Kubernetes

Docker Desktop → Settings → Kubernetes → Enable Kubernetes

### Deploy

1. Clone the repo

```bash
git clone https://github.com/usmanb21/Stockflow-k8s.git
cd Stockflow-k8s
```

1. Create your secrets file

```bash
cp k8s/secret.yaml.example k8s/secret.yaml
# Edit secret.yaml with your actual credentials
```

1. Apply all manifests

```bash
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/secret.yaml
kubectl apply -f k8s/sqlserver.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

1. Verify pods are running

```bash
kubectl get pods -n inventory-app
```

1. Access the API

```bash
kubectl port-forward service/inventory-api-service 8080:80 -n inventory-app
```

Then open: <http://localhost:8080/swagger>

## 🔒 Secrets

`secret.yaml` is gitignored for security. Create it locally:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: sql-secret
  namespace: inventory-app
type: Opaque
stringData:
  SA_PASSWORD: "<your-sa-password>"
  ConnectionString: "Server=sqlserver;Database=InventoryDb;User Id=sa;Password=<your-sa-password>;TrustServerCertificate=True"
```

## ✅ Features

- Kubernetes namespace isolation
- Kubernetes Secrets for credential management
- SQL Server running as a pod
- API auto-connects and runs EF Core migrations on startup
- Port-forward for local access

## 👤 Author

**Usman**
[LinkedIn](https://www.linkedin.com/in/usman-zahid-butt-353a9430)
