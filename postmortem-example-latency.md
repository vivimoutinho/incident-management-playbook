# Incident Postmortem: Payment Latency Spike (SEV2)
**Incident Date:** 2025-05-15  
**Incident ID:** INC-2025-05-15-PAY-01  
**Severity:** SEV2 (Major Degradation)  
**Incident Commander:** @vivimoutinho  
**Status:** Completed & Reviewed

---

## 1. Executive Summary
During peak transaction hours, the core Payment API experienced a 400% increase in latency (p95), affecting approximately 15% of checkout attempts. The incident lasted 45 minutes from detection to full resolution.

## 2. Impact
* **Services Affected:** Payment Gateway Service, Checkout API.
* **User Impact:** Delayed transactions and increased 504 Gateway Timeout errors.
* **Duration:** 45 minutes (14:00 UTC - 14:45 UTC).

## 3. Timeline (UTC)
* **14:00** - Datadog alert triggered: Payment API Latency > 500ms.
* **14:03** - Incident declared; SRE and Engineering teams paged.
* **14:10** - Initial stakeholder communication sent via Slack #incident-channel.
* **14:20** - Root cause identified: Resource saturation in the Auto-Scaling Group (ASG) due to a sudden traffic spike.
* **14:30** - Mitigation: Manually increased instance count and adjusted HPA (Horizontal Pod Autoscaler) thresholds.
* **14:45** - Latency returned to baseline (<100ms). Monitoring phase initiated.

## 4. Root Cause Analysis (RCA)
**The "Five Whys":**
1. **The API was slow.** Because the pods were CPU-saturated.
2. **The pods were saturated.** Because the traffic spike exceeded the current scaling capacity.
3. **The auto-scaler didn't react fast enough.** Because the HPA "Cool-down" period was set too high.
4. **The cool-down was too high.** Because it was configured during low-traffic testing and never optimized for peak-hour volatility.

## 5. Lessons Learned
* **What went well?** Monitoring detected the issue instantly; Incident Commander response was swift.
* **What went wrong?** Manual intervention was required to scale because the automation was too conservative.
* **Where did we get lucky?** The database layer remained stable under the increased load.

## 6. Action Items
| Task ID | Description | Owner | Status |
| :--- | :--- | :--- | :--- |
| **ACTION-01** | Tune HPA target CPU and cool-down periods | @sre-team | Done |
| **ACTION-02** | Implement predictive scaling based on historical peak data | @infra | Backlog |
| **ACTION-03** | Update Dashboard to include "Time to Scale" metric | @vivimoutinho | In Progress |

---
*Blameless Review: We identified that our automation thresholds were outdated for our current growth rate.*
