# Citrix Xen Command Cheat Sheet for Debian

This cheat sheet is designed for system administrators and IT professionals managing Citrix Xen virtualization on Debian-based systems. Citrix Xen is a powerful virtualization platform used extensively for server virtualization, enabling efficient management of virtual machines.

Below is a collection of essential commands that cover daily operations, virtual machine management, networking, and storage in a Xen environment. This guide aims to provide quick and easy access to common Xen commands, facilitating effective management of Xen hosts and guests.
Citrix Xen Commands

## Basic Host and System Management

- **xe host-list**
  - Lists all hosts in a pool.

- **xe vm-list**
  - Lists all virtual machines.

- **xe vm-start uuid=[VM_UUID]**
  - Starts a specific virtual machine.

- **xe vm-shutdown uuid=[VM_UUID]**
  - Shuts down a specific virtual machine.

- **xe vm-reboot uuid=[VM_UUID]**
  - Reboots a specific virtual machine.

- **xe vm-suspend uuid=[VM_UUID]**
  - Suspends a specific virtual machine.

- **xe vm-resume uuid=[VM_UUID]**
  - Resumes a suspended virtual machine.

## Networking

- **xe network-list**
  - Lists all networks.

- **xe vif-list vm-uuid=[VM_UUID]**
  - Lists virtual interfaces of a specific VM.

- **xe pif-list host-uuid=[HOST_UUID]**
  - Lists physical interfaces on a host.

## Storage Management

- **xe sr-list**
  - Lists all storage repositories.

- **xe vdi-list sr-uuid=[SR_UUID]**
  - Lists virtual disk images in a storage repository.

- **xe vbd-list vm-uuid=[VM_UUID]**
  - Lists virtual block devices of a VM.

## VM Management

- **xe vm-export vm=[VM_NAME] filename=[FILE_PATH]**
  - Exports a VM to a file.

- **xe vm-import filename=[FILE_PATH]**
  - Imports a VM from a file.

- **xe template-list**
  - Lists all VM templates.

- **xe vm-clone uuid=[VM_UUID] new-name-label=[NEW_NAME]**
  - Clones a VM.

- **xe vm-snapshot uuid=[VM_UUID] new-name-label=[SNAPSHOT_NAME]**
  - Takes a snapshot of a VM.

## Resource and Pool Management

- **xe pool-list**
  - Lists all resource pools.

- **xe pool-param-set uuid=[POOL_UUID] name-label=[NEW_NAME]**
  - Sets parameters for a resource pool.

- **xe host-param-set uuid=[HOST_UUID] name-label=[NEW_NAME]**
  - Sets parameters for a host.

## Troubleshooting and Monitoring

- **xe task-list**
  - Lists all running tasks.

- **xe event-list**
  - Lists recent events.

- **xe log-display**
  - Displays the host log.
