Here’s a polished **Markdown (.md)** version of your content with better structure, formatting, and explanations:

```markdown
# Metrics and Prometheus in Monitoring

## 📊 What Are Metrics?
Metrics are **periodic or historical data points** that help us understand the **health of a system**.

---

### 🏥 **Medical Field Analogy**
Imagine a patient admitted to a hospital. A nurse collects key information every 15 minutes and records it:  

```

10:00 AM → HB → 76
10:15 AM → HB → 81
... (continues throughout the day)

````

At the end of the day, the nurse gives this information to the doctor to determine whether the patient’s condition is **critical** or **safe**.

➡ **In summary:**  
Metrics are simply **historical data of events**, collected periodically, to assess system health.

---

### ⚠️ **Drawback**
- Metrics may result in **large volumes of numerical data** stored in plain text (e.g., Notepad).  
- This makes it hard to **identify patterns** or **spot issues** quickly.  

✅ **Solution:** Send metrics to a **monitoring system**.  
- Monitoring systems visualize metrics in **dashboards** (e.g., graphs).  
- We can configure **alerts** (e.g., notify on WhatsApp if HB goes above 90).

---

## 💻 Metrics in IT Systems
In IT, metrics are equally crucial. Suppose an application is deployed using **AWS**, **VPC**, and **Kubernetes**. Metrics can help us monitor and understand the health of the system, such as:  
- CPU usage of nodes  
- Memory usage of nodes  
- Deployment status  
- Number of deployment replicas  
- HTTPS request counts  
- User behavior (e.g., sign-ups in the last 30 days, login/logout times)

---

## 🔎 What Is Prometheus?
Prometheus is a **top monitoring tool** for collecting and managing metrics.  
- It **pulls metrics** from various sources.  
- **Represents metrics** on a dashboard.  
- Allows you to **configure alerts** and send them to the respective team or person.

---

## ⚙️ Prometheus Installation (via Helm)

### 1. **Install Helm (if not already installed)**
```powershell
choco install kubernetes-helm -y
````

---

### 2. **Add Prometheus Helm Repository**

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

---

### 3. **Create a Custom Configuration**

Create a file: `custom_kube_prometheus_stack.yml`

```yaml
alertmanager:
  alertmanagerSpec:
    # ✅ Select Alertmanager configuration based on labels
    # Prevents issues where misconfigured selectors could miss alerts.
    alertmanagerConfigSelector:
      matchLabels:
        release: monitoring

    # ✅ Set replicas for high availability (default: 2)
    replicas: 2

    # ✅ Match strategy set to None for flexibility
    alertmanagerConfigMatcherStrategy:
      type: None
```

---

### 4. **Install Prometheus Stack**

```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
-n monitoring \
-f ./custom_kube_prometheus_stack.yml
```

---

### 5. **Verify Pods**

```bash
kubectl --namespace monitoring get pods -l "release=monitoring"
```

---

### 6. **Port Forward Services**

**Prometheus Dashboard (Port 9090)**

```bash
kubectl port-forward service/prometheus-operated -n monitoring 9090:9090
```

Open: `http://127.0.0.1:9090`

**Grafana Dashboard (Port 8080)**

```bash
kubectl port-forward service/monitoring-grafana -n monitoring 8080:80
```

Open: `http://127.0.0.1:8080`
Login:

* Username: `admin`
* Password: `prom-operator`

**Alertmanager (Port 9093)**

```bash
kubectl port-forward service/alertmanager-operated -n monitoring 9093:9093
```

Open: `http://127.0.0.1:9093`

---

## ✅ **Summary**

* **Metrics** provide historical and periodic data for understanding system health.
* **Monitoring systems** (e.g., Prometheus) make metrics easier to visualize and act upon.
* Prometheus, paired with Grafana and Alertmanager, provides a **complete observability solution** with dashboards and alerts.

```

Would you like me to generate this as a **downloadable `.md` file**?
```
