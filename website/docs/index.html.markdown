---
layout: "tfe"
page_title: "Provider: Terraform Enterprise"
sidebar_current: "docs-tfe-index"
description: |-
  The Terraform Enterprise provider is used to interact with the many resources supported by Terraform Enterprise. The provider needs to be configured with the proper credentials before it can be used.
---

# Terraform Enterprise Provider

The Terraform Enterprise provider is used to interact with the many resources
supported by [Terraform Enterprise](https://www.hashicorp.com/products/terraform).
It supports both the SaaS version of Terraform Enterprise
([app.terraform.io](https://app.terraform.io)) and private instances.

Use the navigation to the left to read about the available resources.

~> **Important:** For production use, you should constrain the acceptable provider versions via configuration,
to ensure that new versions with breaking changes will not be automatically installed. 
For more information, see [Versions](#versions).

## Authentication

This provider requires a Terraform Enterprise API token in order to manage
resources.

To manage the full selection of resources, provide a
[user token](/docs/cloud/users-teams-organizations/users.html#api-tokens)
from an account with appropriate permissions. This user should belong to the
"owners" team of every Terraform Enterprise organization you wish to manage.

-> **Note:** You can use [an organization or team token](/docs/cloud/users-teams-organizations/api-tokens.html)
instead of a user token, but it will limit which resources you can manage.
Organization and team tokens cannot manage resources across multiple
organizations, and organization tokens cannot manage certain resource types
(like SSH keys). See the
[Terraform Enterprise API documentation](/docs/cloud/api/index.html)
for more details about access to specific resources.

There are three ways to provide the required token:

- On the CLI, omit the `token` argument and set a `credentials` block in your
  [CLI config file](/docs/commands/cli-config.html#credentials).
- Set the `TFE_TOKEN` environment variable.
- In a Terraform Enterprise workspace, set `token` in the provider
  configuration. Use an input variable for the token and mark the corresponding
  variable in the workspace as sensitive.
  
## Versions

For production use, you should constrain the acceptable provider versions via configuration,
to ensure that new versions with breaking changes will not be automatically installed by 
`terraform init` in future. As this provider is still at version zero, you should constrain 
the acceptable provider versions on the minor version.

If you are using Terraform CLI version 0.12.x, you can constrain this provider to 0.15.x versions 
by adding a `required_providers` block inside a `terraform` block.
```
terraform {
  required_providers {
    tfe = "~> 0.15.0"
  }
}
```

If you are using Terraform CLI version 0.11.x, you can constrain this provider to 0.15.x versions 
by adding the version constraint to the tfe provider block.
```
provider "tfe" {
  version = "~> 0.15.0"
  ...
}
```

For more information on constraining provider versions, see the 
[provider versions documentation](https://www.terraform.io/docs/configuration/providers.html#provider-versions).
  

## Example Usage

```hcl
# Configure the Terraform Enterprise Provider
provider "tfe" {
  hostname = "${var.hostname}"
  token    = "${var.token}"
  version  = "~> 0.15.0"
}

# Create an organization
resource "tfe_organization" "org" {
  # ...
}
```

## Argument Reference

The following arguments are supported:

* `hostname` - (Optional) The Terraform Enterprise hostname to connect to.
  Defaults to `app.terraform.io`. Can be overridden by setting the
  `TFE_HOSTNAME` environment variable.
* `token` - (Optional) The token used to authenticate with Terraform Enterprise.
  Only set this argument when running in a Terraform Enterprise workspace; for
  CLI usage, omit the token from the configuration and set it as `credentials`
  in the [CLI config file](/docs/commands/cli-config.html#credentials) or set
  the `TFE_TOKEN` environment variable. See [Authentication](#authentication)
  above for more.
