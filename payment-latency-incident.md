# Case Study: Payment System Latency Incident

### Scenario Overview
Increased latency detected in a payment processing system during peak transaction hours.

> **Incident Classification:** Severity 2 (High)  
> **Status:** Resolved  
> **Detection Source:** Datadog Service Level Objective (SLO) Breach

---

### 1. Detection & Impact
* **Detection:** Datadog alert triggered: latency above 500ms threshold for 5 consecutive minutes.
* **Impact:** Delayed transaction processing affecting ~15% of users; increased 5xx error rates in API responses.

### 2. Response & Mitigation Strategy
1. **Incident Declaration:** Incident Commander (IC) assigned within 3 minutes of alert.
2. **Engagement:** Paged On-call Engineering and Infrastructure teams.
3. **Investigation:** Analysis of recent CI/CD deployments and cloud resource utilization.
4. **Mitigation:** * Traffic load balanced across additional auto-scaling groups.
   * Rate-limiting applied to non-critical background jobs to prioritize payment flow.

### 3. Communication Log
* **T+10min:** Initial internal notification sent to Stakeholders.
* **T+25min:** Status update: "Mitigation in progress via horizontal scaling."
* **T+45min:** Resolution: "System stabilized. Monitoring for 15 minutes before closing."

### 4. Post-Incident & Resolution
* **Root Cause:** Resource saturation (CPU Steal) due to unexpected spike in transaction volume.
* **Action Items:**
  * [ ] Adjust HPA (Horizontal Pod Autoscaler) thresholds for the Payment Gateway.
  * [ ] Implement predictive scaling based on historical peak data.
  * [ ] Refine alerting to trigger *before* SLO breach (Early Warning System).
