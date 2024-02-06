# Pacemaker Command Cheat Sheet

This cheat sheet is designed for system administrators who manage high-availability clusters using Pacemaker on Debian-based systems. Pacemaker is widely used for its robustness and flexibility in managing cluster resources and ensuring service availability.

The commands listed here cover various aspects of cluster management, including checking cluster status, managing resources, nodes, and constraints. This guide aims to provide quick access to common Pacemaker commands, facilitating effective cluster management and troubleshooting.
Pacemaker Commands

## Cluster Status and Management

- **pcs status**
  - Displays the current status of the cluster.

- **pcs cluster start --all**
  - Starts all cluster services on all nodes.

- **pcs cluster stop --all**
  - Stops all cluster services on all nodes.

- **pcs cluster enable --all**
  - Enables cluster services to start at boot on all nodes.

- **pcs cluster disable --all**
  - Disables cluster services from starting at boot.

## Resource Management

- **pcs resource show**
  - Lists all cluster resources.

- **pcs resource create [resource_id] [agent] [options]**
  - Creates a new cluster resource.

- **pcs resource delete [resource_id]**
  - Deletes a cluster resource.

- **pcs resource manage [resource_id]**
  - Manages a resource (enables cluster management).

- **pcs resource unmanage [resource_id]**
  - Unmanages a resource (disables cluster management).

## Node Management

- **pcs status nodes**
  - Shows the status of all nodes in the cluster.

- **pcs cluster standby [node]**
  - Sets a cluster node to standby mode.

- **pcs cluster unstandby [node]**
  - Removes a cluster node from standby mode.

- **pcs cluster cib**
  - Outputs the current cluster configuration.

## Constraint Management

- **pcs constraint list**
  - Lists all constraints in the cluster.

- **pcs constraint create [constraint_options]**
  - Creates a new constraint.

- **pcs constraint remove [constraint_id]**
  - Removes a constraint.

## Cluster Configuration

- **pcs property list**
  - Lists all cluster properties.

- **pcs property set [property_name]=[value]**
  - Sets a cluster property.

- **pcs stonith show**
  - Displays STONITH (fencing) devices and status.

- **pcs stonith create [stonith_id] [agent] [options]**
  - Creates a new STONITH device.

- **pcs stonith delete [stonith_id]**
  - Deletes a STONITH device.

## Troubleshooting and Logs

- **pcs status failures**
  - Shows all failed actions in the cluster.

- **corosync-cfgtool -s**
  - Displays the status of the Corosync ring.

- **crm_mon -1**
  - Displays a one-time status of the current cluster.
