# terraform-azurerm-app-service-ssl-binding

Terraform module designed to binds an existing certificate to an existing Azure PaaS Service Plan from the associated it assocuated Service Plan.

## Usage

### Sample
Include this repository as a module in your existing terraform code:

```hcl

data "azurerm_app_service" "test01" {
  name                = "testing-app-service-01"
  resource_group_name = "testing-service-rg"
}

data "azurerm_app_service" "test02" {
  name                = "testing-app-service-02"
  resource_group_name = "testing-service-rg"
}

module "eg_bind_certificate_to_services_apps" {
  source     = "git::https://github.com/transactiveltd/terraform-azurerm-app-service-ssl-binding.git?ref=v0.1.0"

  app_services_and_fqdns = {
      "${data.azurermazurerm_app_service.test01.name}" = "webapp01.example.com,spacialName01.example.com"
      "${data.azurermazurerm_app_service.test02.name}" = "webapp02.example.com"
  }
  count_of_app_services      = 2
  certificate_thumbprint = "a909502dd82ae41433e6f83886b00d4277a32a7b"
  resource_group_name    = "testing-service-rg"
}
```

This will run an arm template deployment on the given resource group, add bind the certifcate to each Services App for each of the FQDNs. The the certificate is for the FQDNs
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| app_services_and_fqdns | key/value pair app_service = fqdn | map | - | yes |
| count_of_app_services | Must match the number keys (App Services) in the `app_services_and_fqdns` map above | number | - | yes |
| certificate_thumbprint | Certifcate thumbprint eg: `a909502dd82ae41433e6f83886b00d4277a32a7b` | string | - | yes |
| resource_group_name | Resource Group name, e.g. `testing-service-rg` | string | - | yes |


## Outputs

| Name | Description |
|------|-------------|
| thumprint | Certificate Thumbprint |
