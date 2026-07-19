# Cisco Unified Intelligence Center

> **Status:** Legacy reference — technical validation pending

Cisco Unified Intelligence Center (CUIC) provides reporting and dashboards for Cisco contact-center deployments. Exact architecture, supported data sources, licensing, browser requirements, and administrative workflows vary substantially by CUIC and UCCE/UCCX release.

## Typical responsibilities

- real-time and historical contact-center reporting;
- supervisor and operations dashboards;
- report definitions, value lists, and collections;
- user roles, permissions, and report access;
- integration with contact-center historical and real-time data sources;
- export, scheduling, and distribution of reports where supported.

## Operational checks

Before changing a production CUIC environment, identify:

- the exact CUIC and contact-center release;
- deployment model and node roles;
- data sources and database connectivity;
- replication or high-availability state;
- certificate and authentication dependencies;
- report ownership, permissions, and scheduling impact;
- current backups and rollback procedure.

## Troubleshooting sequence

1. Confirm whether the problem affects one user, one report, one data source, or the entire cluster.
2. Check node and service health using the version-specific administration tools.
3. Compare browser, authentication, role, and permission behavior across users.
4. Verify data-source reachability and credentials.
5. Compare report query timing with database and application logs.
6. Check certificates, DNS, time synchronization, and recent changes.
7. Validate fixes against a noncritical report before changing shared definitions.

## Common symptom areas

| Symptom | Checks |
|---|---|
| Report does not load | Data source, query, permissions, service health |
| Data appears stale | Data-source status, replication, collection interval, clock |
| User cannot see a report | Role, collection membership, object permissions |
| Dashboard is slow | Query design, database load, report filters, node resources |
| Scheduled report fails | Scheduler service, destination, credentials, storage, email relay |

## Related references

- [CVP](cvp.md)
- [ICM](icm.md)

> [!NOTE]
> This page intentionally avoids version-specific procedures until they are revalidated against current Cisco documentation. Use the documentation set matching the exact deployed release before applying configuration or upgrade guidance.
