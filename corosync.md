### Corosync Cheat Sheet

#### Introduction to Corosync
Corosync is an open-source clustering service and a group communication system. It's used to implement high availability within applications and create redundant clusters.

- **Purpose**: Provides a fault-tolerant layer for cluster communication.

#### Key Concepts
- **Cluster Engine**: Manages cluster membership and facilitates consistent messaging.
- **Quorum**: Ensures that actions are taken only when a majority (quorum) of cluster members agree.
- **Totem Protocol**: Manages cluster membership and messaging.
- **Ring**: A logical network structure used by Corosync for communication.

#### Basic Corosync Commands
- **corosync-cfgtool -s**: Displays the status of the Corosync ring.
- **corosync-quorumtool**: Displays the current quorum status of the cluster.
- **corosync-cmapctl**: Displays the Corosync configuration and runtime statistics.

#### Configuration
- **corosync.conf**: The main configuration file for Corosync.
- **Nodes Configuration**: Define nodes with their node ID and ring addresses.
- **Totem Configuration**: Configure network and consensus settings.
- **Quorum Configuration**: Define quorum policies and settings.

#### Cluster Management
- **Starting Corosync**: `systemctl start corosync`
- **Stopping Corosync**: `systemctl stop corosync`
- **Enabling Corosync at Boot**: `systemctl enable corosync`
- **Disabling Corosync at Boot**: `systemctl disable corosync`

#### Troubleshooting
- **Logs**: Check Corosync logs in `/var/log/corosync/corosync.log`.
- **Ring Status**: Use `corosync-cfgtool -s` to check the status of the communication ring.
- **Quorum Status**: Use `corosync-quorumtool` to check if the cluster has quorum.

#### Tips for Using Corosync
- **Regular Configuration Backup**: Keep backups of your `corosync.conf` file.
- **Monitoring**: Continuously monitor Corosync logs and status.
- **Integration with Pacemaker**: Often used in conjunction with Pacemaker for complete high availability solutions.
