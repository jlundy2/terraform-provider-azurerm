---
subcategory: "Security Center"
layout: "azurerm"
page_title: "Azure Resource Manager: azurerm_security_center_workspace"
sidebar_current: "docs-azurerm-security-center-workspace"
description: |-
    Manages the subscription's Security Center Workspace.
---

# azurerm_security_center_workspace

Manages the subscription's Security Center Workspace.

~> **NOTE:** Owner access permission is required.

~> **NOTE:** The subscription's pricing model can not be `Free` for this to have any affect.

## Example Usage

```hcl
resource "azurerm_resource_group" "example" {
  name     = "tfex-security-workspace"
  location = "westus"
}

resource "azurerm_log_analytics_workspace" "example" {
  name                = "tfex-security-workspace"
  location            = "${azurerm_resource_group.example.location}"
  resource_group_name = "${azurerm_resource_group.example.name}"
  sku                 = "PerGB2018"
}

resource "azurerm_security_center_workspace" "example" {
  scope        = "/subscriptions/00000000-0000-0000-0000-000000000000"
  workspace_id = "${azurerm_log_analytics_workspace.example.id}"
}
```

## Argument Reference

The following arguments are supported:

* `scope` - (Required) The scope of VMs to send their security data to the desired workspace, unless overridden by a setting with more specific scope.

* `workspace_id` - (Required) The ID of the Log Analytics Workspace to save the data in.

## Attributes Reference

The following attributes are exported:

* `id` - The Security Center Workspace ID.

## Import

The contact can be imported using the `resource id`, e.g.

```shell
terraform import azurerm_security_center_workspace.example /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Security/workspaceSettings/default
```
