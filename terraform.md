# Terraform Command Cheat Sheet

This cheat sheet is designed for DevOps engineers, cloud architects, and anyone who uses Terraform for infrastructure as code. Terraform, by HashiCorp, is an immensely popular tool for building, changing, and versioning infrastructure safely and efficiently.

The commands listed here range from basic setup and initialization to more advanced infrastructure management. This guide serves as a quick reference to essential Terraform commands, facilitating efficient infrastructure deployment and management.
Terraform Commands

## Basic Setup and Initialization

- **terraform init**
  - Initializes a new or existing Terraform configuration.

- **terraform version**
  - Displays the current Terraform version.

- **terraform validate**
  - Validates the Terraform files in a directory.

## Plan and Apply

- **terraform plan**
  - Creates an execution plan.

- **terraform apply**
  - Applies the changes required to reach the desired state of the configuration.

- **terraform apply "planfile"**
  - Applies the changes in a Terraform plan file.

## State Management

- **terraform state list**
  - Lists resources in the state file.

- **terraform state show [resource]**
  - Shows the attributes of a resource in the state file.

- **terraform state rm [resource]**
  - Removes a resource from the state file.

## Workspace Management

- **terraform workspace list**
  - Lists all existing workspaces.

- **terraform workspace new [name]**
  - Creates a new workspace.

- **terraform workspace select [name]**
  - Selects an existing workspace.

## Advanced Commands

- **terraform import [resource.address] [id]**
  - Imports existing infrastructure into Terraform.

- **terraform taint [resource]**
  - Marks a resource for recreation on the next apply.

- **terraform untaint [resource]**
  - Removes the 'taint' from a resource.

- **terraform graph**
  - Generates a visual representation of either a configuration or execution plan.
