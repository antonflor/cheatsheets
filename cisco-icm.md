# Cisco Intelligent Contact Management (ICM)

- **Purpose**: ICM is a sophisticated call routing system used in contact centers to direct incoming calls to the best available agent or service.
- **Functionality**: It includes features like skills-based routing, queue management, and network-to-desktop Computer Telephony Integration (CTI). ICM helps in reducing call handling times and improving customer satisfaction.
- **Integration**: ICM typically integrates with other systems like Automatic Call Distributors (ACD), Interactive Voice Response (IVR) systems including CVP, and CRM software.

> **Related Cheat Sheets**: See [cisco-cuic.md](cisco-cuic.md) for Cisco Unified Intelligence Center (CUIC) reporting details, and [cisco-cvp.md](cisco-cvp.md) for Cisco Unified Customer Voice Portal (CVP) details.

---

## Implementation

1. **Network and Contact Center Integration**: ICM is implemented as a central component of a larger contact center solution. It integrates with various network elements, including Automatic Call Distributors (ACD), Interactive Voice Response (IVR) systems like CVP, and databases.
2. **Configuration and Scripting**: ICM's setup involves extensive configuration and scripting to define call routing logic and business rules. This includes setting up agents, queues, and routing strategies.
3. **Data Center Approach**: Typically, ICM is deployed in a data center environment, taking advantage of robust server and network infrastructure for reliability and scalability.

## Licensing

- ICM licensing is based on the size and complexity of the deployment, typically involving concurrent user licenses or licenses based on the volume of interactions managed.
- Contact your Cisco account team for current licensing models, as these may change between versions.

## Use Cases

1. **Advanced Call Routing**: Determines the best destination for incoming calls based on predefined criteria, such as agent skills, caller data, time of day, and business hours.
2. **Multi-Channel Integration**: Manages interactions across various channels, including voice, email, chat, and social media.
3. **Real-Time and Historical Reporting**: Provides comprehensive reporting capabilities to analyze contact center performance. (Reporting is surfaced via CUIC — see [cisco-cuic.md](cisco-cuic.md).)
4. **Skills-Based Routing**: Routes callers to agents with the specific skills required to handle their inquiry.
5. **Network-to-Desktop CTI**: Delivers caller data to the agent's desktop (screen pop) at the time the call is connected.

## Management

- **Routine Maintenance**: Regular updates and patches from Cisco are necessary to keep the system running efficiently and securely.
- **Configuration Management**: Ongoing management of routing scripts and contact center parameters to adapt to changing business needs.
- **Performance Monitoring**: Continuous monitoring of system performance and call handling metrics.
- **Script Administration**: ICM routing scripts define how calls are handled. Regular audits of routing logic are recommended to ensure scripts reflect current business requirements.

---

## Architecture

### Core Components

1. **Router**
   - The central component that makes real-time decisions on routing customer interactions.
   - Processes call routing requests and determines the best target (queue, agent, or IVR) based on configured logic.
   - Operates in a duplex (active/standby) configuration for high availability.

2. **Logger**
   - Records all routing decisions and transaction data for historical reporting and analysis.
   - Maintains a database of routing activity used by reporting tools such as CUIC.
   - Also operates in a duplex configuration.

3. **Administrative Workstation (AW)**
   - Provides the interface for configuration, scripting, and real-time monitoring.
   - Used by administrators to create and maintain routing scripts, agent configurations, and system settings.
   - Includes the Script Editor, Configuration Manager, and Distributor components.

4. **Peripheral Gateways (PG)**
   - Connect ICM to various contact center components like ACDs, IVRs, and databases.
   - Act as the bridge between the ICM Router and telephony or media components.
   - Types include ACD PG, IVR PG (for CVP integration), and Agent PG.

5. **CTI Server**
   - Provides real-time call and agent data to desktop applications via the CTI protocol.
   - Enables screen pops, agent state monitoring, and call control from agent desktops.

6. **CTI OS (CTI Object Server)**
   - Middleware layer that simplifies integration between agent desktop applications and the CTI Server.
   - Provides APIs and toolkits for building custom agent desktop applications.

### Network Considerations

- **Scalability**: ICM can scale to support large, distributed contact center environments spanning multiple sites and thousands of agents.
- **High Availability**: Designed for high availability with duplex components (dual Router, dual Logger) to minimize downtime.
- **Latency Sensitivity**: ICM is latency-sensitive. The network between ICM components (Router, Logger, PG) should have low latency — typically within the same data center or connected via low-latency WAN.

### Integration Points

- **CVP (Customer Voice Portal)**: ICM sends call routing instructions to CVP, which handles the IVR/VoiceXML interaction with the caller.
- **Unified CM (CUCM)**: ICM integrates with Cisco Unified Communications Manager for agent phone control and call routing to agent extensions.
- **UCCX/UCCE**: ICM is the routing engine within Unified Contact Center Enterprise (UCCE). UCCX has its own built-in routing engine.
- **Multi-Channel Integration**: Integrates with Email, Chat, and Social Media channels via Cisco's multichannel portfolio.
- **CRM Integration**: Can integrate with Salesforce, Microsoft Dynamics, and other CRM platforms via CTI adapters for screen pop and customer data lookup.

### Security

- **Data Security and Compliance**: Includes features to ensure the security and privacy of customer data.
- **Authentication**: ICM administrative interfaces integrate with Active Directory for user authentication.
- **Role-Based Access**: Granular role-based permissions control who can view or modify configurations and routing scripts.
- **Encrypted Communications**: Support for TLS encryption on inter-component communications.

---

## Common Problems and Fixes

### Call Routing Failures

- **Problem**: Calls not routing correctly, going to wrong queues, or being dropped.
- **Fix**: Check ICM routing scripts for logic errors in Script Editor. Verify peripheral gateway connectivity. Review Router logs for routing decision details. Confirm that agents are logged in and in the correct skill groups.

### Peripheral Gateway Connectivity Issues

- **Problem**: PG reports disconnected or ICM loses visibility to ACDs or IVR systems.
- **Fix**: Check network connectivity between PG and ICM Router. Verify PG service health on the PG server. Review PG logs for error messages. Ensure firewall rules permit ICM-to-PG traffic on required ports.

### CTI / Screen Pop Not Working

- **Problem**: Agent desktops not receiving call data or screen pops failing.
- **Fix**: Verify CTI Server connectivity from agent workstations. Check CTI OS Server service health. Confirm the agent's desktop application is configured with the correct CTI Server address and port. Review agent login state in ICM.

### Agent Reporting Discrepancies

- **Problem**: Agent state or call counts in reports do not match actual activity.
- **Fix**: Verify Logger connectivity and database health. Check for data synchronization issues between ICM Logger and CUIC. Ensure peripheral gateways are reporting agent events correctly.

### High Router CPU or Memory

- **Problem**: ICM Router experiencing high resource utilization leading to routing delays.
- **Fix**: Review routing scripts for inefficient logic (e.g., excessive database lookups in the call path). Monitor call volume versus system capacity. Consider load balancing across multiple Router instances in large deployments.

### Script Errors / Failed Routing Steps

- **Problem**: Routing scripts failing at a specific step, resulting in calls going to error paths.
- **Fix**: Use Script Editor's simulation mode to trace script execution. Check external database connections referenced by the script. Verify that all referenced labels, skill groups, and services are correctly configured.
