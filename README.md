# Terraform AWS VPC Lab Module

Terraform resources for a VPC with public, private, and database subnet tiers, an internet gateway, a NAT gateway, route tables, and optional VPC peering.

## Structure

| File | Purpose |
|---|---|
| [main.tf](main.tf) | VPC, subnet tiers, gateways, routes, and associations |
| [variables.tf](variables.tf) | CIDRs, project labels, tags, and peering toggle |
| [locals.tf](locals.tf) | Shared naming and availability-zone selection |
| [data.tf](data.tf) | AWS data lookups |
| [peering.tf](peering.tf) | Optional peering resources |
| [outputs.tf](outputs.tf) | Currently contains no active outputs |

## Inputs

`project` and `environment` are required. Review the subnet CIDR lists, selected availability zones, and optional tag maps before planning.

The VPC CIDR must contain every subnet CIDR. For the default `10.0.x.0/24` subnet ranges, use `vpc_cidr = "10.0.0.0/16"`.

## Validate locally

```bash
terraform init -backend=false
terraform fmt -check
terraform validate
```

Use an AWS provider configuration in the consuming root module and inspect its plan before applying. This repository does not currently declare a tested provider-version range.

## Design limits

- A single NAT gateway serves the private and database tiers; this is not a per-AZ NAT design.
- NAT gateways and public IPv4 addresses can incur charges.
- Consumers need explicit outputs added before they can reference resource IDs through the module interface.
- Peering requires compatible, non-overlapping address spaces and appropriate routing.

This is a learning module; it has not been validated as production infrastructure.
