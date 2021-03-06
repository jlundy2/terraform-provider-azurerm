---
subcategory: "API Management"
layout: "azurerm"
page_title: "Azure Resource Manager: azurerm_api_management_api"
sidebar_current: "docs-azurerm-resource-api-management-api-x"
description: |-
  Manages an API within an API Management Service.
---

# azurerm_api_management_api

Manages an API within an API Management Service.

## Example Usage

```hcl
resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West Europe"
}

resource "azurerm_api_management" "example" {
  name                = "example-apim"
  location            = "${azurerm_resource_group.example.location}"
  resource_group_name = "${azurerm_resource_group.example.name}"
  publisher_name      = "My Company"
  publisher_email     = "company@terraform.io"

  sku_name = "Developer_1"
}

resource "azurerm_api_management_api" "example" {
  name                = "example-api"
  resource_group_name = "${azurerm_resource_group.example.name}"
  api_management_name = "${azurerm_api_management.example.name}"
  revision            = "1"
  display_name        = "Example API"
  path                = "example"
  protocols           = ["https"]

  import {
    content_format = "swagger-link-json"
    content_value  = "http://conferenceapi.azurewebsites.net/?format=json"
  }
}
```

## Argument Reference

The following arguments are supported:

* `name` - (Required) The name of the API Management API. Changing this forces a new resource to be created.

* `api_management_name` - (Required) The Name of the API Management Service where this API should be created. Changing this forces a new resource to be created.

* `resource_group_name` - (Required) The Name of the Resource Group where the API Management API exists. Changing this forces a new resource to be created.

* `revision` - (Required) The Revision which used for this API.

* `display_name` - (Required) The display name of the API.

* `path` - (Required) The Path for this API Management API, which is a relative URL which uniquely identifies this API and all of it's resource paths within the API Management Service.

* `protocols` - (Required) A list of protocols the operations in this API can be invoked. Possible values are `http` and `https`.

---

* `description` - (Optional) A description of the API Management API, which may include HTML formatting tags.

* `import` - (Optional) A `import` block as documented below.

* `service_url` - (Optional) Absolute URL of the backend service implementing this API.

* `soap_pass_through` - (Optional) Should this API expose a SOAP frontend, rather than a HTTP frontend? Defaults to `false`.

* `subscription_key_parameter_names` - (Optional) A `subscription_key_parameter_names` block as documented below.

* `version` - (Optional) The Version number of this API, if this API is versioned.

* `version_set_id` - (Optional) The ID of the Version Set which this API is associated with.

-> **NOTE:** When `version` is set, `version_set_id` must also be specified

---

A `import` block supports the following:

* `content_format` - (Required) The format of the content from which the API Definition should be imported. Possible values are: `swagger-json`, `swagger-link-json`, `wadl-link-json`, `wadl-xml`, `wsdl` and `wsdl-link`.

* `content_value` - (Required) The Content from which the API Definition should be imported. When a `content_format` of `*-link-*` is specified this must be a URL, otherwise this must be defined inline.

* `wsdl_selector` - (Optional) A `wsdl_selector` block as defined below, which allows you to limit the import of a WSDL to only a subset of the document. This can only be specified when `content_format` is `wsdl` or `wsdl-link`.

---

A `subscription_key_parameter_names` block supports the following:

* `header` - (Required) The name of the HTTP Header which should be used for the Subscription Key.

* `query` - (Required) The name of the QueryString parameter which should be used for the Subscription Key.

---

A `wsdl_selector` block supports the following:

* `service_name` - (Required) The name of service to import from WSDL.

* `endpoint_name` - (Required) The name of endpoint (port) to import from WSDL.

---

## Attributes Reference

In addition to all arguments above, the following attributes are exported:

* `id` - The ID of the API Management API.

* `is_current` - Is this the current API Revision?

* `is_online` - Is this API Revision online/accessible via the Gateway?

* `version` - The Version number of this API, if this API is versioned.

* `version_set_id` - The ID of the Version Set which this API is associated with.

## Import

API Management API's can be imported using the `resource id`, e.g.

```shell
terraform import azurerm_api_management_api.example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mygroup1/providers/Microsoft.ApiManagement/service/instance1/apis/api1
```
