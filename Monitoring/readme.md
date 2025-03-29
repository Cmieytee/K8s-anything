
# 📊 Kubernetes Monitoring with Prometheus, Grafana, Node Exporter, Kube-State-Metrics & Alertmanager

This project sets up a **complete observability stack** on a Kubernetes cluster using **Prometheus**, **Grafana**, **Node Exporter**, **kube-state-metrics**, and **Alertmanager** — installed using raw YAML manifests for hands-on understanding and control.

---

## 🧱 Stack Components

| Tool               | Purpose                                      |
|--------------------|----------------------------------------------|
| Prometheus         | Scrapes and stores metrics                   |
| Grafana            | Visualizes metrics with rich dashboards      |
| Node Exporter      | Collects system-level node metrics           |
| Kube-State-Metrics | Exposes Kubernetes object metrics            |
| Alertmanager       | Handles alert routing (email, Slack, etc.)   |

---

## 🚀 Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [kubectl configured](https://minikube.sigs.k8s.io/docs/start/)
- Basic knowledge of Kubernetes

---

## 🛠️ Setup Instructions

### 1. Clone the Repository (or Save YAMLs)

```bash
git clone <repo-url>
cd k8s-monitoring/
```

### 2. Apply Resources in Order

```bash
# Create monitoring namespace
kubectl apply -f monitoring-namespace.yaml

# Prometheus Core Setup
kubectl apply -f prometheus-config.yaml
kubectl apply -f prometheus-deployment.yaml
kubectl apply -f prometheus-service.yaml

# Grafana
kubectl apply -f grafana-deployment.yaml
kubectl apply -f grafana-service.yaml

# kube-state-metrics
kubectl apply -f kube-state-metrics.yaml

# node-exporter
kubectl apply -f node-exporter.yaml

# Alertmanager
kubectl apply -f alertmanager-config.yaml
kubectl apply -f alertmanager-deployment.yaml
```

---

## 🌐 Accessing the Monitoring Tools

### 🔍 Prometheus
```bash
minikube service prometheus-service -n monitoring
```

### 📈 Grafana
```bash
minikube service grafana -n monitoring
```

#### Login:
- **Username:** `admin`
- **Password:** `admin`

#### Setup:
- Add Prometheus as a data source:
  - URL: `http://prometheus-service.monitoring.svc.cluster.local:9090`
- Import dashboards:
  - Recommended IDs:
    - `1860` – Kubernetes Cluster Monitoring
    - `11074` – Node Exporter Full

### 🚨 Alertmanager
```bash
minikube service alertmanager -n monitoring
```

Alerts will appear here and route based on your `alertmanager.yml` config.

---

## 📧 Email Alerts

To enable email alerts:

1. Edit `alertmanager-config.yaml`:
```yaml
global:
  smtp_smarthost: 'smtp.gmail.com:587'
  smtp_from: 'your.email@gmail.com'
  smtp_auth_username: 'your.email@gmail.com'
  smtp_auth_password: 'your-app-password'
  smtp_require_tls: true
```

2. Replace with your actual Gmail + App Password.
3. Re-apply and restart Alertmanager:
```bash
kubectl apply -f alertmanager-config.yaml
kubectl delete pod -l app=alertmanager -n monitoring
```

---

## 📦 Project Structure

```bash
.
├── monitoring-namespace.yaml
├── prometheus-config.yaml
├── prometheus-deployment.yaml
├── prometheus-service.yaml
├── grafana-deployment.yaml
├── grafana-service.yaml
├── kube-state-metrics.yaml
├── node-exporter.yaml
├── alertmanager-config.yaml
└── alertmanager-deployment.yaml
```

---

## 🧠 Extras / To-Do

- ✅ Test alerts with fake rules
- ✅ Create custom Grafana dashboards
- 🔲 Set up Ingress for clean URLs
- 🔲 Add persistent storage for Prometheus
- 🔲 Enable authentication and SSL

---

## 🙌 Credits

- [Prometheus](https://prometheus.io/)
- [Grafana](https://grafana.com/)
- [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/)
- [Kubernetes](https://kubernetes.io/)


