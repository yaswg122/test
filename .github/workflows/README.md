## Overview
---
This Terraform module provisions an **AWS ElastiCache Serverless** cluster (e.g., Redis) with support for VPC networking, security groups, KMS encryption, CloudWatch logging, and tagging.  
It is designed to be reusable, configurable, and suitable for secure deployments across multiple environments.

---

## Requirements

Before deploying this module, ensure the following resources exist:

- **VPC** with at least two private **subnets** in different Availability Zones.
- **Security Group(s)** allowing inbound traffic on port `6379` (for Redis).
- Optional:
  - **KMS Key** for encryption at rest.
  - **CloudWatch Log Group** for logging.
  - **ElastiCache User Group** for Redis authentication.

---

## Providers

| Name | Version |
|------|----------|
| <a name="provider_aws"></a> aws | >= 5.48.0 |

---

## Modules

No external modules used.

---

## Resources

| Name | Type |
|------|------|
| aws_elasticache_serverless_cache.this | `aws_elasticache_serverless_cache` |

---

## Inputs

| Name | Description | Type | Default | Required |
|------|--------------|------|----------|:--------:|
| <a name="input_name"></a> name | Name of the ElastiCache Serverless cluster | `string` | n/a | yes |
| <a name="input_engine"></a> engine | Cache engine (`redis` or `memcached`) | `string` | `"redis"` | no |
| <a name="input_description"></a> description | Description for the ElastiCache cluster | `string` | `null` | no |
| <a name="input_region"></a> region | AWS region to deploy the cache | `string` | n/a | yes |
| <a name="input_subnet_ids"></a> subnet_ids | List of subnet IDs for the cache | `list(string)` | `[]` | no |
| <a name="input_security_group_ids"></a> security_group_ids | List of security group IDs for the cache | `list(string)` | `[]` | no |
| <a name="input_user_group_id"></a> user_group_id | Optional user group ID for Redis authentication | `string` | `null` | no |
| <a name="input_major_engine_version"></a> major_engine_version | Redis major engine version | `string` | `"7"` | no |
| <a name="input_kms_key_id"></a> kms_key_id | Optional KMS key ID for encryption at rest | `string` | `null` | no |
| <a name="input_log_group_name"></a> log_group_name | CloudWatch Log Group for ElastiCache logs | `string` | `null` | no |
| <a name="input_tags"></a> tags | Map of resource tags | `map(string)` | `{}` | no |

---

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_elasticache_serverless_arn"></a> elasticache_serverless_arn | ARN of the ElastiCache Serverless cluster |
| <a name="output_elasticache_endpoint"></a> elasticache_endpoint | Endpoint address of the ElastiCache Serverless cluster |
