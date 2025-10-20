# Application & Service Issue — Identification and Response Plan
**Category:** CySA+  
**Date:** 20/10/2025  
**Author:** Nnamso Mkpong

---

## Purpose
This document provides a practical, repeatable process to identify, diagnose, and respond to application or service outages. It’s written so an on-call engineer or support team can follow the steps, collect evidence, and produce a clear incident report for management.

---

## Target system / service
**Example service:** Internal web application — `intranet.example.local`  
(Replace with the real service name you want to document.)

---

## 1 — How we identify potential issues
**Monitoring sources**
- Application logs (application-specific log files, e.g., `/var/log/app/*.log`)  
- System logs (Windows Event Viewer, `/var/log/syslog`, `/var/log/messages`)  
- Web server logs (IIS or Apache/Nginx access and error logs)  
- Monitoring/observability tools (example: Zabbix, Prometheus, Nagios, CloudWatch)  
- Synthetic checks (HTTP uptime checks, API health endpoints)

**Alert triggers to watch for**
- Spike in HTTP 5xx errors or a sudden rise in error rate  
- High latency (p95/p99 above SLA threshold)  
- Failed health checks or probe failures from synthetic monitors  
- Service process not running, frequent crashes, or OOM (out of memory) events  
- Unusual authentication failures or repeated timeouts

---

## 2 — Initial triage and data collection
When an alert comes in, follow these steps in order:

1. **Acknowledge and create an incident record**
   - Open the incident ticket (PagerDuty/Jira/ServiceNow) and note time and source of alert.

2. **Identify scope and impact**
   - Which users, regions, or endpoints are affected?  
   - Is it affecting all traffic or a subset (one datacenter, one cluster, one service)?

3. **Quick health checks**
   - Ping or curl the service endpoint: `curl -I https://intranet.example.local/health`
   - Check process status: `systemctl status app.service` or `ps aux | grep app`
   - Confirm recent deployments or configuration changes (check CI/CD dashboard and deployment logs)

4. **Gather logs and metrics**
   - Pull relevant logs (last 30 minutes) from app, web server, and database.  
   - Check monitoring dashboards for CPU, memory, disk I/O, network, and request latency trends.  
   - Save snippets of relevant log entries to the incident ticket (timestamped).

5. **Record any immediate mitigation taken**
   - E.g., restarted service, rolled back deployment, increased instance count — include exact commands and times.

---

## 3 — Deeper diagnosis steps
If the quick triage doesn’t reveal a clear fix, escalate to deeper checks:

- **Application errors**
  - Search logs for stack traces, uncaught exceptions, or repeated error codes.
  - Check for failing dependencies (e.g., authentication provider, third-party APIs).

- **Database issues**
  - Check connection counts, slow queries, locks, and replication lag.
  - Confirm database availability and errors in DB logs.

- **Resource exhaustion**
  - Look at CPU, memory, and disk usage. Check for spikes or sustained high usage.
  - Check for file descriptor exhaustion or thread pool exhaustion.

- **Network problems**
  - Use `traceroute`/`mtr` and packet captures (tcpdump) between tiers to find connectivity issues.
  - Check firewall or load balancer logs for dropped connections.

- **Configuration / deployment issues**
  - Review recent code commits and configuration changes (time window matching incident).
  - If rollback is acceptable and quick, prepare rollback procedure.

---

## 4 — Response actions (playbook)
Use the least disruptive action that resolves the customer impact quickly, then follow up with root-cause actions.

**Common immediate actions**
- Restart the affected service:  
  `sudo systemctl restart app.service` (only after checking logs)  
- Roll back the last deployment if evidence points to a bad release.  
- Scale out temporarily (add instances) to relieve pressure while investigating.  
- Apply a temporary configuration change (e.g., increase DB connection pool) with approval.

**When to escalate**
- If the outage hits critical SLA or data loss is possible, escalate to on-call senior engineer and notify management immediately.  
- Engage vendor support for third-party outages.

---

## 5 — Communication plan
- **Initial notice (within 15 minutes):** short summary on the incident channel (Slack/email) — impact, systems affected, initial mitigation, ETA for next update.  
- **Updates:** provide status updates every 30 minutes (or as agreed) until resolved.  
- **Post-incident:** deliver an executive summary and a technical report within 48 hours.

---

## 6 — Post-incident actions and reporting
After service is restored:

1. **Prepare an incident report** including:
   - Timeline of events (with timestamps)  
   - Root cause analysis (root cause, contributing factors)  
   - Actions taken to mitigate and restore service  
   - Recommended long-term fixes and owners  
2. **Add monitoring and alert improvements** to prevent reoccurrence (examples: new alerts, dashboard widgets).  
3. **Update runbooks and run a post-mortem** with involved engineers to agree on remediation and preventive tasks.

---

## 7 — Example walk-through (scenario)
**Scenario:** After a deployment, users report intermittent 500 errors on the web app.

- 09:05 — Alert: 5xx error rate spikes (monitoring). Incident ticket opened.  
- 09:07 — Quick checks: `systemctl status app.service` shows service running; recent deploy finished at 09:02.  
- 09:10 — Check logs: repeated `DatabaseConnectionTimeout` entries around 09:03–09:06.  
- 09:12 — Action: roll back deployment to previous version (checked with deploy tool).  
- 09:16 — Service steady; error rate returns to normal.  
- 09:20 — Post-incident: root cause = deployment introduced a DB connection leak. Fix = revert + patch; preventive = add DB connection pool metrics and alert.

**Deliverable:** Incident report with timeline, root cause, remediation, and tasks to prevent recurrence (assign tasks and deadlines).

---

## 8 — Quick checklist (for the on-call engineer)
- [ ] Create incident ticket with timestamp and initial notes.  
- [ ] Confirm service health (ping, curl, process status).  
- [ ] Pull and save relevant logs to the ticket.  
- [ ] Check recent deployments/config changes.  
- [ ] Take action (restart/rollback/scale) and record commands used.  
- [ ] Communicate status updates and escalate if needed.  
- [ ] Produce post-incident report and update runbooks.

---


---


---

## Suggested commit message
`Add lab03: application & service issue response plan (initial)`

