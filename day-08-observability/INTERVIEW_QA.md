
---

## 📄 `INTERVIEW_QA.md`  
### Observability – Interview Questions & Answers

### 🔹 Beginner Level

**Q1. What is observability?**  
**A:**  
Observability is the ability to understand a system’s internal state by analyzing its outputs such as logs, metrics, and traces.

---

**Q2. What are the three pillars of observability?**  
**A:**  
Logs, metrics, and traces.

---

**Q3. What is the difference between monitoring and observability?**  
**A:**  
Monitoring tells you when something is wrong, while observability helps you understand why it is wrong.

---

### 🔹 Intermediate Level

**Q4. When would you use metrics instead of logs?**  
**A:**  
Metrics are used to identify trends, trigger alerts, and monitor system health, while logs are used for detailed debugging.

---

**Q5. Why are metrics preferred for autoscaling?**  
**A:**  
Metrics are lightweight, numeric, and continuously available, making them suitable for real-time scaling decisions.

---

**Q6. Why are logs considered expensive?**  
**A:**  
Logs are high-volume, unstructured, and costly to store, search, and index at scale.

---

**Q7. Why are Kubernetes logs considered ephemeral?**  
**A:**  
Because logs are tied to pod lifecycles and are lost when pods restart unless centralized logging is used.

---

### 🔹 Advanced Level (SRE / DevOps)

**Q8. How would you debug a slow service in Kubernetes?**  
**A:**  
Start with metrics to identify latency or resource saturation, then use logs for errors and traces to pinpoint slow dependencies.

---

**Q9. Why is metrics-first a best practice?**  
**A:**  
Metrics scale better, allow alerting, and provide system-wide visibility without overwhelming storage or compute.

---

**Q10. What problems do traces solve that logs cannot?**  
**A:**  
Traces show end-to-end request flow across services, helping identify latency and dependency issues.

---

**Q11. What are common observability anti-patterns?**  
**A:**  
- Logging everything  
- No alerts  
- Alerting on symptoms instead of causes  
- No correlation between signals  

---

**Q12. Can observability prevent outages?**  
**A:**  
Observability does not prevent failures but helps detect, diagnose, and recover from them faster.

---

**Q13. How does observability support SLOs and SLAs?**  
**A:**  
Metrics and alerts help track error budgets, latency, and availability against defined objectives.

---

## 🧠 Interview Tip (Final)

If you remember **one line**, remember this:

> *Metrics for trends and alerts, logs for debugging, traces for latency and dependencies.*

That sentence alone can carry you through multiple interviews.
