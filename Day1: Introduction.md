# Observability: Understanding the Internal State of a System

Observability helps us understand **what is happening inside a system**, **why it is happening**, and **how to fix it**. It provides deep insights into the internal state of components such as:

- **Application**
- **Infrastructure**
- **Network**

---

## Common System States to Monitor
- Disk and CPU **over-utilized**
- Disk and CPU **under-utilized**
- Disk and CPU **reached threshold**
- Number of **successful HTTP requests**
- Number of **failed HTTP requests**
- **Reasons for API failures**

---

## The Three Pillars of Observability

### 1. **What → Metrics**
Metrics provide **historical data** about system events.  
- Use metrics to **analyze performance over time** (e.g., last 30–60 minutes or a specific period).  
- Create **alerts** and **dashboards** based on metrics to monitor system health.  
- Example: If HTTP requests start failing, check the metrics for trends and anomalies.

---

### 2. **Why → Logging**
Logs give **context** to events and help explain **why** an issue occurred.  
- Log levels include: **Info**, **Debug**, **Error**, **Trace**.  
- Logs provide detailed snapshots of events, helping you **understand application behavior**.  
- Example: Use logs to identify **when** the issue started and the **error messages** generated.

---

### 3. **How → Tracing**
Traces provide **end-to-end visibility** of a request across the system.  
- Track a request from **client → frontend → backend → database**.  
- Measure how long each stage takes to **pinpoint bottlenecks or failures**.  
- Example: Use traces to debug or troubleshoot issues, ensuring all stages perform as expected.

---

## Example Workflow
1. **Identify the Issue (What)**  
   - Monitor metrics for failed HTTP requests.  
   - Check performance data over a specific timeframe (e.g., 30–60 minutes).  

2. **Find the Cause (Why)**  
   - Analyze logs to see **when** and **why** the failure occurred.  

3. **Fix the Issue (How)**  
   - Use traces to follow the request path across all components.  
   - Identify performance bottlenecks or errors and resolve them.  

---

## Summary
Observability is not just about knowing that a problem exists—it’s about **understanding what the problem is**, **why it happened**, and **how to resolve it**. By combining **metrics**, **logs**, and **traces**, teams can quickly detect, analyze, and fix issues to maintain healthy and reliable systems.
