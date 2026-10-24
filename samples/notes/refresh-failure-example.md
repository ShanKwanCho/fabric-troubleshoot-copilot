# Incident notes — SYNTHETIC EXAMPLE (all names and IDs are fake)

Workspace: Contoso Sales Analytics
Semantic model: Contoso Sales (Import mode, scheduled refresh 06:00 UTC daily)
Gateway: CONTOSO-GW-01 (standard mode, cluster of 1)
Data source: Oracle 19c, host oracle-dev.contoso.example, service SALESPDB

2026-10-20 06:00 UTC  Scheduled refresh starts.
2026-10-20 06:00:41   Refresh fails. Error shown in refresh history:
  "DM_GWPipeline_Gateway_DataSourceAccessError"
  Underlying: ORA-01017: invalid username/password; logon denied
  Activity ID ending ...9f3a
2026-10-20 06:05      Second automatic retry fails, same error.
2026-10-20 08:12      Report owner notices yesterday's numbers on the dashboard.
2026-10-20 08:30      Gateway admin confirms the Oracle service account password
                      was rotated on 2026-10-19 by the DBA team (planned change,
                      gateway team not notified).
2026-10-20 08:41      Data source credentials updated in gateway settings.
2026-10-20 08:44      Test connection succeeds.
2026-10-20 08:47      Manual refresh succeeds (duration 6m 12s).

Impact: Contoso Sales report showed data from 2026-10-19 until 08:53 UTC.
~40 report viewers affected. No data loss.

Open question: is there a way to alert on refresh failure before users notice?
