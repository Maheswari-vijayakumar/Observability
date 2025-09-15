Here’s a polished Markdown file based on your content. I’ve added some extra context, formatting improvements, and minor clarifications to make it more readable and beginner-friendly.

---

````markdown
# Python App Metrics with Prometheus

This guide shows how to expose custom metrics from a Python application using **Prometheus**, containerize the app with **Docker**, and deploy it to **Kubernetes** with monitoring using **ServiceMonitor**.

---

## Metric Types

| **Metric Type** | **When to Use**                                              |
| --------------- | ------------------------------------------------------------ |
| **Counter**     | Counting events (e.g., requests, errors) that only increase. |
| **Gauge**       | Values that go **up and down** (e.g., memory, temperature).  |
| **Histogram**   | Measuring **distributions** like latency or request size.    |
| **Summary**     | When you need **quantiles** (e.g., 95th percentile latency). |

---

## Install Prometheus Client

```bash
pip install prometheus-client
````

---

## Python Application Example

**app.py**

```python
from prometheus_client import start_http_server, Counter, Gauge, Histogram
import random, time

# Define metrics
REQUESTS = Counter('app_requests_total', 'Total number of requests')
TEMPERATURE = Gauge('app_temperature_celsius', 'Current temperature in Celsius')
RESPONSE_TIME = Histogram('app_response_time_seconds', 'Response time in seconds')

def process_request():
    REQUESTS.inc()
    TEMPERATURE.set(random.uniform(20, 30))
    with RESPONSE_TIME.time():
        time.sleep(random.uniform(0.1, 0.5))

if __name__ == '__main__':
    start_http_server(8000)  # Expose metrics
    print("Serving metrics on :8000/metrics")
    while True:
        process_request()
        time.sleep(1)
```

**requirements.txt**

```
prometheus-client
```

---

## Dockerize the Application

**Dockerfile**

```dockerfile
FROM python:3.10-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .

EXPOSE 8000
CMD ["python", "app.py"]
```

**Build and Push Docker Image**

```bash
docker build -t mahi2029/python-metrics-app:latest .
docker login
docker push mahi2029/python-metrics-app:latest
```

---

## Deploy to Kubernetes

**Create Deployment and Service (dev namespace)**

**app-deployment.yaml**

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: python-metrics-app
  namespace: dev
spec:
  replicas: 1
  selector:
    matchLabels:
      app: python-metrics-app
  template:
    metadata:
      labels:
        app: python-metrics-app
    spec:
      containers:
        - name: python-metrics-app
          image: mahi2029/python-metrics-app:latest
          ports:
            - containerPort: 8000
---
apiVersion: v1
kind: Service
metadata:
  name: python-metrics-svc
  namespace: dev
  labels:
    app: python-metrics-app
spec:
  selector:
    app: python-metrics-app
  ports:
    - name: http-metrics
      port: 8000
      targetPort: 8000
```

**Apply Deployment**

```bash
kubectl apply -f app-deployment.yaml
```

---

## Set Up ServiceMonitor (monitoring namespace)

**python-app-servicemonitor.yaml**

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: python-app-sm
  namespace: monitoring
  labels:
    release: monitoring
spec:
  selector:
    matchLabels:
      app: python-metrics-app
  namespaceSelector:
    matchNames:
      - dev
  endpoints:
    - port: http-metrics
      path: /metrics
      interval: 15s
```

**Apply ServiceMonitor**

```bash
kubectl apply -f python-app-servicemonitor.yaml
kubectl get servicemonitor -n monitoring
```

---

## Test Metrics Locally

```bash
kubectl port-forward svc/python-metrics-svc -n dev 8000:8000
curl http://127.0.0.1:8000/metrics
```

**Access Prometheus UI**

```bash
kubectl port-forward svc/prometheus-operated -n monitoring 9090:9090
```

Once everything is applied, your custom metrics:

* `app_requests_total`
* `app_temperature_celsius`
* `app_response_time_seconds`

will appear in Prometheus for monitoring and alerting.

---

### Notes

* **Histogram vs Summary:** Use Histogram for aggregation across multiple instances in Prometheus; Summary is best for single-instance quantiles.
* **ServiceMonitor:** Ensure the `ServiceMonitor` labels match Prometheus selectors.
* **Scaling:** Increase `replicas` in the deployment for load testing; Prometheus can aggregate metrics from multiple pods.

<img width="587" height="880" alt="image" src="https://github.com/user-attachments/assets/bc18db6b-5176-4b5a-8b5f-7ed7922c0910" />




