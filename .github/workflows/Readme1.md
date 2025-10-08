## Overview
This Terraform module provisions an **AWS ElastiCache Serverless** cluster (Redis) with:

- VPC networking
- Security groups
- KMS encryption for data at rest
- CloudWatch logging support
- Tagging for resources

It is reusable, configurable, and suitable for secure deployments across multiple environments.

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
|------|---------|
| aws  | >= 5.48.0 |

---

## Modules

No external modules used.

---

## Resources

| Name | Type |
|------|------|
| aws_elasticache_serverless_cache.this | `aws_elasticache_serverless_cache` |
| aws_kms_key.redis | `aws_kms_key` |
| aws_kms_alias.redis | `aws_kms_alias` |
| aws_security_group.redis | `aws_security_group` |
| aws_elasticache_user.redis_user | `aws_elasticache_user` |
| aws_elasticache_user_group.user_group | `aws_elasticache_user_group` |

---

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| name | Name of the ElastiCache Serverless cluster | `string` | n/a | yes |
| engine | Cache engine (`redis` or `memcached`) | `string` | `"redis"` | no |
| description | Description for the ElastiCache cluster | `string` | `null` | no |
| region | AWS region to deploy the cache | `string` | n/a | yes |
| subnet_ids | List of subnet IDs for the cache | `list(string)` | `[]` | no |
| security_group_ids | List of security group IDs for the cache | `list(string)` | `[]` | no |
| user_group_id | Optional user group ID for Redis authentication | `string` | `null` | no |
| major_engine_version | Redis major engine version | `string` | `"7"` | no |
| kms_key_id | Optional KMS key ID for encryption at rest | `string` | `null` | no |
| log_group_name | CloudWatch Log Group for ElastiCache logs | `string` | `null` | no |
| tags | Map of resource tags | `map(string)` | `{}` | no |
| redis_user_secrets | Map of Redis user secrets from AWS Secrets Manager | `map(object({ secret_name = string, access_string = string }))` | `{}` | no |
| redis_ports | List of Redis ports to allow inbound access | `list(number)` | `[6379]` | no |
| redis_ingress_cidr_blocks | List of CIDR blocks allowed to access Redis | `list(string)` | `["10.0.0.0/16"]` | no |

---

## Outputs

| Name | Description |
|------|-------------|
| elasticache_serverless_arn | ARN of the ElastiCache Serverless cluster |
| elasticache_endpoint | Endpoint address of the ElastiCache Serverless cluster |
| redis_cluster_name | Name of the Redis Serverless cluster |
| user_group_id | Redis user group ID containing all users |
| security_group_id | Security group ID of Redis cluster |
| kms_key_id | KMS Key ID used for encryption |
