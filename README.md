![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/umotif-public/terraform-aws-ses-domain?style=social)

# terraform-aws-ses-domain

Terraform module to configure a domain hosted on Route53 to work with AWS Simple Email Service (SES). Module supports both single and multi regional deployments.

These types of resources are created:

* TXT record for SPF validation
* Custom TXT record for SPF validation
* Custom MAIL FROM domain
* CNAME records for DKIM verification
* SES Verfication for the domain
* MX record pointing to AWS's SMTP endpoint

## Terraform versions

Terraform 0.12 and 0.13. Pin module version to `~> v2.0`. Submit pull-requests to `master` branch.

## Usage

### Application Load Balancer

```hcl
module "ses-domain" {
  source = "umotif-public/ses-domain/aws"
  version = "~> 2.0.0"

  zone_id               = "Z2CBQCNDG7YEWJA"
  enable_verification   = true
  enable_incoming_email = true
}
```

## Assumptions

Module is to be used with Terraform > 0.12.

## Examples

* [SES Domain- single region](https://github.com/umotif-public/terraform-aws-ses-domain/tree/master/examples/core)
* [SES Domain- multi regional](https://github.com/umotif-public/terraform-aws-ses-domain/tree/master/examples/multi-regional)

## Authors

Module managed by [Marcin Cuber](https://github.com/marcincuber) [LinkedIn](https://www.linkedin.com/in/marcincuber/).

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Requirements

| Name | Version |
|------|---------|
| terraform | >= 0.12.6 |
| aws | >= 2.41 |

## Providers

| Name | Version |
|------|---------|
| aws | >= 2.41 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| additional\_incoming\_email\_records | List of additional records to be added to mx\_receive Route53 record, e.g. "10 inbound-smtp.us-east-1.amazonses.com" | `list(string)` | `[]` | no |
| additional\_mail\_from\_records | List of additional records to be added to mail\_from\_domain Route53 record, e.g. "10 feedback-smtp.us-east-1.amazonses.com" | `list(string)` | `[]` | no |
| additional\_verification\_tokens | List of additional verification SES domain tokens that will added to verfication Route53 record. | `list(string)` | `[]` | no |
| enable\_dkim\_record | Control whether or not to create route53 DNS record for SES DKIM. | `bool` | `true` | no |
| enable\_from\_domain\_record | Control whether or not to create route53 DNS record for SES mail from domain. | `bool` | `true` | no |
| enable\_incoming\_email\_record | Control whether or not to handle incoming emails | `string` | `true` | no |
| enable\_spf\_record | Control whether or not to set SPF records. | `bool` | `true` | no |
| enable\_verification | Control whether or not to verify SES DNS records. | `bool` | `true` | no |
| enable\_verification\_record | Control whether or not to create route53 DNS record for SES domain verification. | `bool` | `true` | no |
| spf\_txt\_record | DNS TXT record for SPF to tell email providers which servers are allowed to send email from their domains. Variable is portion of the SPF TXT record. | `string` | `"v=spf1 include:amazonses.com -all"` | no |
| zone\_id | Route53 Zone ID belonging to the desired domain | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| ses\_domain\_identity\_verification\_token | SES domain verification token. |
| ses\_identity\_arn | SES identity ARN. |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

## License

See LICENSE for full details.

## Pre-commit hooks

### Install dependencies

* [`pre-commit`](https://pre-commit.com/#install)
* [`terraform-docs`](https://github.com/segmentio/terraform-docs) required for `terraform_docs` hooks.
* [`TFLint`](https://github.com/terraform-linters/tflint) required for `terraform_tflint` hook.

#### MacOS

```bash
brew install pre-commit terraform-docs tflint

brew tap git-chglog/git-chglog
brew install git-chglog
```
