## Scenario: Payment System Latency Incident

### Context
Increased latency detected in a payment processing system during peak transaction hours.

### Impact
- Delayed transaction processing
- Increased error rates in API responses
- Potential financial and customer experience impact

### Detection
- Datadog alert triggered: latency above threshold for 5 minutes
- Spike in response time and error rate observed in dashboards

### Incident Classification
Severity 2 (High) — partial degradation with significant user impact

### Response
- Incident declared and incident commander assigned
- Cross-functional teams engaged (application and infrastructure)
- Initial communication sent to stakeholders
- Investigation initiated on recent deployments and infrastructure load

### Mitigation
- Traffic load balanced across additional resources
- Non-critical processes temporarily limited
- System performance stabilized

### Communication
- Regular updates shared every 15 minutes
- Clear status and next steps communicated to stakeholders

### Resolution
- Root cause identified as resource saturation during peak load
- System stabilized after scaling adjustments

### Post-Incident Actions
- Post-incident review scheduled
- Action items defined to improve scaling strategy and alert thresholds
