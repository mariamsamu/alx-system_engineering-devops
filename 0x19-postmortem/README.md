# Web Stack Outage Postmortem: The Case of the Vanishing Database

## Issue Summary

* **Duration**: 2 hours, 37 minutes (11:23 PM PST - 1:00 AM PST)
* **Impact**: Our primary web application was intermittently unavailable, resulting in user errors and frustration. Approximately 60% of users were affected.
* **Root Cause**: A misconfigured database connection pool led to resource exhaustion and intermittent database timeouts.

## Timeline

* **11:23 PM**: Monitoring alerts triggered, indicating high database connection wait times and application errors.
* **11:25 PM**: On-call engineer acknowledged the alert and began investigating system logs and metrics.
* **11:30 PM**: Initial assumption was heavy user load; however, traffic patterns seemed normal.
* **11:45 PM**: Database server metrics showed high CPU and memory usage, leading to the investigation of database-related issues.
* **12:15 AM**: Misleading path: Attempted database server restart, but the issue persisted after the restart.
* **12:30 AM**: Incident escalated to senior database administrator and development team lead.
* **1:00 AM**: Root cause identified: Database connection pool misconfiguration limited the number of available connections. Pool configuration adjusted, resolving the issue.

## Root Cause and Resolution

The application's database connection pool was configured with a low maximum connection limit. Under normal load, this wasn't a problem, but a slight increase in traffic caused the application to exhaust available connections. This resulted in new requests waiting for a connection, leading to timeouts and errors.

The resolution involved increasing the maximum connection limit in the pool configuration to a value that could comfortably handle peak traffic. This change allowed the application to scale and handle the increased load without exhausting connections.

## Corrective and Preventative Measures

* **Improvements**:
    * Review and optimize all connection pool configurations across the application.
    * Enhance monitoring and alerting for connection pool metrics and resource usage.
    * Implement load testing to identify performance bottlenecks under various traffic patterns.
* **Tasks**:
    * Audit and update connection pool configurations for all databases.
    * Add monitoring for connection pool metrics (e.g., active connections, wait times).
    * Configure alerts for connection pool thresholds and resource exhaustion.
    * Schedule regular load testing exercises.
    * Create a runbook for troubleshooting database connection issues.

## Conclusion

While this outage caused disruption, it highlighted the importance of proper resource management and configuration. By implementing the corrective and preventative measures outlined above, we aim to prevent similar incidents in the future and ensure a smoother experience for our users.

**Remember:** Even the most well-engineered systems can encounter unexpected hiccups. The key is to learn from these incidents, continuously improve, and always be prepared!
