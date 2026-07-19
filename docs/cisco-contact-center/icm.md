# Cisco Intelligent Contact Management

> **Status:** Legacy reference — technical validation pending

Cisco Intelligent Contact Management (ICM) is the routing and orchestration layer used in Cisco Unified Contact Center Enterprise environments. It coordinates routing decisions across contact-center components, agents, queues, IVR systems, and external data sources.

## Typical components

- Router for real-time routing decisions;
- Logger for configuration and historical data;
- Administrative Workstation and Distributor services;
- Peripheral Gateways for integration with telephony, agent, and media systems;
- CTI services for agent-desktop and call-control integrations;
- routing scripts, skill groups, services, and precision-routing objects;
- duplex and geographically redundant components where supported.

Exact roles, service names, database behavior, and redundancy requirements are release-specific.

## Operational checks

Before changing a production ICM environment, identify:

- the exact UCCE/ICM release and patch level;
- duplex side and component health;
- Router, Logger, Distributor, and Peripheral Gateway relationships;
- current routing-script version and deployment history;
- database, DNS, NTP, certificate, and Active Directory dependencies;
- call-volume and capacity impact;
- current backups and rollback process.

## Troubleshooting sequence

1. Capture the affected call or agent identifiers and timestamps.
2. Confirm whether the issue affects routing, agent state, reporting, CTI, or one peripheral.
3. Check component and duplex health using the release-specific diagnostic tools.
4. Trace the routing script and identify the first unexpected branch or missing object.
5. Verify Peripheral Gateway and CTI connectivity.
6. Correlate Router, Logger, PG, and application logs by timestamp.
7. Validate any correction with a controlled test call before broad deployment.

## Common symptom areas

| Symptom | Checks |
|---|---|
| Calls route to the wrong target | Script logic, object configuration, schedules, skill state |
| Calls enter an error path | Missing labels, unavailable services, database or peripheral failure |
| Peripheral appears offline | Network path, service health, side selection, certificates, ports |
| Agent state is stale | Agent PG, CTI services, Unified CM integration, synchronization |
| Reporting does not match routing | Logger health, replication, reporting interval, data-source status |
| Router utilization is high | Script complexity, external lookups, call volume, component health |

## Related references

- [CUIC](cuic.md)
- [CVP](cvp.md)

> [!NOTE]
> This page intentionally avoids version-specific commands and architectural limits until they are revalidated against the documentation set for the deployed Cisco release.
