# Puppet Command Cheat Sheet

This document is a handy reference for system administrators and DevOps engineers using Puppet, a popular configuration management tool. Puppet automates the process of enforcing and maintaining system configurations across a variety of hardware and software platforms.

The commands listed here cover everything from basic operations and system checks to more complex tasks like module management and environment configuration. This cheat sheet is designed to help you efficiently manage your infrastructure with Puppet.

---

## System Checks and Operations

- **puppet agent --test**
  - Runs a Puppet agent manually and applies the latest configuration.

- **puppet agent --enable**
  - Enables the Puppet agent if it's been disabled.

- **puppet agent --disable**
  - Temporarily disables the Puppet agent.

- **puppet config print [setting]**
  - Prints the value of a Puppet configuration setting.

## Managing Puppet Code and Environments

- **puppet apply [manifest.pp]**
  - Applies a standalone Puppet manifest to the local system.

- **puppet module install [module-name]**
  - Installs a module from the Puppet Forge.

- **puppet module list**
  - Lists installed modules.

- **puppet module upgrade [module-name]**
  - Upgrades a module to the latest version.

- **puppet environment list**
  - Lists all available environments.

## Node and Resource Management

- **puppet resource [resource-type] [resource-title]**
  - Manages a resource.

- **puppet resource service**
  - Lists all services known to Puppet.

- **puppet cert list**
  - Lists all pending certificate signing requests.

- **puppet cert sign [certname]**
  - Signs a pending certificate request.

- **puppet node purge [node-name]**
  - Completely removes a node from PuppetDB and cleans up exported resources.

## Misc Puppet Tasks

- **puppet db export [file.tar.gz]**
  - Exports PuppetDB data to a file.

- **puppet db import [file.tar.gz]**
  - Imports data into PuppetDB from a file.

- **puppet lookup [key] --node [node-name] --environment [environment]**
  - Looks up data in Hiera.

- **puppet parser validate [manifest.pp]**
  - Validates the syntax of a Puppet manifest.

- **puppet report show [report]**
  - Displays a specific report from PuppetDB.

## Debugging and Troubleshooting

- **puppet agent --debug**
  - Runs the agent in debug mode for verbose output.

- **puppet agent --fingerprint**
  - Prints the certificate fingerprint.

- **puppet master --compile [node-name]**
  - Compiles a catalog for a specific node.

- **puppet facts find [node-name]**
  - Retrieves facts from a specified node.

