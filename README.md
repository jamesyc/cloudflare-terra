# cloudflare-terra

Terraform config for managing Cloudflare DNS across two domains:
- `jamesyc.com`
- `002015.xyz`

## Prerequisites

- [Terraform](https://developer.hashicorp.com/terraform/install) >= 1.5
- A Cloudflare API token with DNS edit permissions

## Setup

1. Clone the repo
2. Copy the example vars file and fill in your token:
   ```
   cp terraform.tfvars.example terraform.tfvars
   ```
3. Initialize Terraform (downloads the Cloudflare provider):
   ```
   terraform init
   ```

## Applying changes

Preview changes:
```
terraform plan
```

Apply changes:
```
terraform apply
```

## Adding a new subdomain

1. Add a `cloudflare_record` block to `dns.tf`, for example:
   ```hcl
   resource "cloudflare_record" "my_new_subdomain" {
     zone_id = "1589ca8b5998db003314eb07ce8fb95e"  # jamesyc.com
     name    = "myapp"
     type    = "A"
     content = "144.24.16.36"
     proxied = false
     ttl     = 1
   }
   ```
2. Run `terraform plan` to preview
3. Run `terraform apply` to deploy

**Zone IDs:**
- `jamesyc.com` → `1589ca8b5998db003314eb07ce8fb95e`
- `002015.xyz` → `8e1205e94e1c8d8c5b876aac286f08bd`

## Notes

- `imports.tf` contains import blocks used to bring existing Cloudflare records into Terraform state on first apply
- `terraform.tfvars` is gitignored — never commit your API token
