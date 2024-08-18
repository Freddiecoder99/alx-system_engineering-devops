Postmortem: Outage on `451866-web-01` Web Server

Issue Summary

Duration:
Start: August 17, 2024, 14:00 UTC  
End: August 17, 2024, 16:30 UTC

Impact:
The `451866-web-01` web server experienced a complete outage for 2 hours and 30 minutes. During this period, all hosted services, including the main website and API endpoints, were inaccessible to users. Approximately 85% of users were affected, leading to a significant drop in service availability and user experience.

Root Cause:
A misconfiguration in the server's firewall settings led to blocking inbound HTTP/HTTPS traffic, causing the server to become unreachable.

Timeline

- 14:00 UTC: Issue detected when monitoring tools alerted that the web server was unresponsive.
- 14:05 UTC: On-call engineer verified the alert and confirmed the web server was not responding to HTTP requests.
- 14:10 UTC: Initial investigation focused on potential network issues or server overload.
- 14:30 UTC: Network diagnostics showed no issues; CPU and memory usage were within normal limits, leading to further confusion.
- 14:45 UTC: Misleading path: Assumed the issue was related to a recent software update, rollback initiated.
- 15:00 UTC: Rollback completed, but the issue persisted. Traffic logs indicated all requests were being dropped before reaching the server.
- 15:15 UTC: Incident escalated to the network team to investigate potential firewall or routing issues.
- 15:30 UTC: Network team identified a recent change in firewall settings that inadvertently blocked inbound traffic on ports 80 and 443.
- 15:45 UTC: Firewall rules corrected to allow HTTP/HTTPS traffic.
- 16:00 UTC: Server accessibility restored. Monitoring tools confirmed full recovery.
- 16:30 UTC: Post-incident checks completed, and services fully operational.



Root Cause and Resolution

The root cause of the outage was a misconfiguration in the firewall settings on the `451866-web-01` server. A recent security update included a new firewall rule set, but a typo in the rule caused it to block all inbound HTTP and HTTPS traffic. As a result, users could not access the services hosted on the server, leading to the outage.

To resolve the issue, the firewall settings were reviewed and corrected. The specific rule blocking the traffic was identified and modified to permit HTTP/HTTPS traffic on ports 80 and 443. Once the rule was updated, the server immediately became accessible again.

Corrective and Preventative Measures

Improvements and Fixes:
- Review and standardize firewall configuration change procedures to prevent similar issues in the future.
- Implement automated testing of firewall rules to detect potential misconfigurations before they are applied.
- Enhance monitoring to include checks for service availability at the network layer, ensuring quicker detection of similar issues.

Tasks:
1. Patch Firewall Configuration: Update and standardize the firewall rules across all servers to ensure consistency.
2. Automated Testing: Develop and deploy a script to test firewall configurations before applying them in production.
3. Monitoring Enhancement: Add network-level checks to the monitoring system to detect when services become inaccessible due to firewall or routing issues.
4. Incident Review: Conduct a post-incident review with the network and security teams to analyze the firewall rule change process and identify any further improvements.

By implementing these measures, we aim to reduce the risk of future outages related to firewall misconfigurations and improve overall service reliability.

