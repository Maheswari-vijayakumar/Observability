# 📊 Prometheus Monitoring Workflow

---

## 🏗 **Key Components**

### 1. **Node Exporter**
- Collects **system-level metrics** such as:
  - CPU usage  
  - Memory usage  
  - Infrastructure health  
- Runs on each node to expose hardware and OS-level metrics for Prometheus.

---

### 2. **Kube-State Metrics**
- Collects **Kubernetes cluster state information**, including:
  - Pod statuses  
  - Deployment replicas  
  - Service states  
- Provides these metrics for Prometheus scraping.

---

### 3. **Custom Metrics**
- Applications expose their own metrics endpoints (e.g., HTTP `/metrics`).  
- **Developers** define these metrics (e.g., login counts, request latency).  
- Prometheus scrapes these endpoints for application-specific insights.

---

### 4. **MySQL Exporter**
- Dedicated exporter to **monitor database performance and health**.  
- Prometheus scrapes MySQL metrics for storage and analysis.

---

## 🔗 **Prometheus Core**
Prometheus is the **central monitoring system**:  
- **Scrapes metrics** from Node Exporter, Kube-State Metrics, custom metrics, and database exporters.  
- Stores data in **Time Series Database (TSDB)**.  
- Provides a **HTTP API** for queries and external integrations.  
- Uses **PromQL (Prometheus Query Language)** for querying stored metrics.

---

## 👥 **Users and Visualization**
- **Users** (developers, SREs, operators) query Prometheus data using **PromQL**.  
- Data can be visualized as **graphs or dashboards** using Prometheus or tools like Grafana.  
- Enables performance analysis, debugging, and capacity planning.

---

## 📢 **Alerting**
- **Alertmanager** works with Prometheus to:
  - Trigger alerts based on metric thresholds.  
  - Send notifications via **Slack**, **email**, or other channels.  
- Ensures that critical events are promptly reported to the right teams.

---

## ⚙ **Workflow Summary**
1. **Exporters** collect system, cluster, and database metrics.  
2. **Prometheus scrapes** metrics at intervals.  
3. Data is stored in **TSDB** for historical analysis.  
4. **PromQL** allows flexible querying of this data.  
5. **Alertmanager** notifies teams of critical issues.  
6. **Users** visualize trends and take action based on insights.

---


Here’s a **polished Markdown (.md)** document with clear explanations and improved formatting for your content:

````markdown
# 🔎 Kubernetes Metrics with Prometheus

This guide explains how to use **Prometheus metrics** to monitor Kubernetes pod and container statuses, restarts, and configuration changes.

---

## 📌 **Key Metrics**

### 1. `kube_pod_container_status_restarts_total`
- **Description:**  
  Counts the total number of times a **container within a pod** has restarted.  
- **Usage:**  
  Monitor container stability. Frequent restarts may indicate crashing containers or misconfigurations.  
- **Example:**  
  ```promql
  kube_pod_container_status_restarts_total{namespace="default"}
````

* If nothing is running in the `default` namespace, **no data** will be displayed.

---

### 🧪 **Testing Container Restarts**

1. **Run a Pod That Crashes Immediately**

   ```bash
   kubectl run busy-box-crash --image=busybox -- /bin/sh -c "exit 1"
   ```

   * This creates a pod (`busy-box-crash`) in the `default` namespace that exits immediately.

2. **Check Pod Status**

   ```bash
   kubectl get pods
   ```

3. **Query Restart Counts**

   ```promql
   kube_pod_container_status_restarts_total{namespace="default"}
   ```

   * Now the metric will display **restart counts** for the crashed pod.

---

### 2. `kube_pod_init_container_status_restarts_total`

* **Description:**
  Shows the total restarts of **init containers** within pods.
* **Usage:**
  Helps diagnose initialization issues (e.g., misconfigured volumes or network dependencies).

---

### 3. `kube_configmap_created`

* **Description:**
  Provides timestamps for when **ConfigMaps** were created.
* **Example:**

  ```promql
  kube_configmap_created{namespace="kube-system"}
  ```

  * This query lists creation times for ConfigMaps in the `kube-system` namespace.
* **Usage:**
  Useful for auditing changes or troubleshooting configuration-related issues.

---

### Grafana

# 📊 Grafana Integration with Prometheus

Grafana is a **powerful visualization tool** that works with Prometheus to provide **better dashboards** and access control for different user groups.

---

## 🔗 **Grafana + Prometheus**
- **Prometheus** collects and stores metrics.  
- **Grafana** connects to Prometheus as a **data source**.  
- Provides **interactive and customizable dashboards** for visualizing metrics.

---

## 🔑 **Authentication & Authorization (AuthN & AuthZ)**
Grafana supports:
- **Authentication (AuthN)** → Verifies user identity.  
- **Authorization (AuthZ)** → Controls what actions users can perform.  
- Integrates with **IAM (Identity and Access Management)** systems for secure access.

---

## 👥 **User Roles & Access**
Grafana dashboards can be shared across multiple teams:
1. **Management (View Only)**  
   - Can view dashboards for high-level insights.  
   - Useful for executives or stakeholders.

2. **DevOps Team**  
   - Full access to create, edit, and manage dashboards.  
   - Responsible for infrastructure and application monitoring.

3. **Developers**  
   - View and create dashboards related to application performance.  

4. **QA (Quality Assurance)**  
   - Access to dashboards to monitor testing environments and application stability.

---

## ✅ **Summary**
- **Grafana + Prometheus** = Enhanced observability with powerful dashboards.  
- **AuthN & AuthZ** ensure secure and role-based access.  
- Teams such as **management**, **DevOps**, **developers**, and **QA** benefit from tailored access and insights.






---
