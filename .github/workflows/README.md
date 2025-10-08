## Overview
---
This Terraform module provisions an **AWS ElastiCache Serverless for Redis** environment.  
It creates a KMS key, security group, Redis users (from AWS Secrets Manager), a user group, and the Redis Serverless cluster itself.  
It is designed to simplify secure, multi-user Redis deployments across multiple environments.

---

## Requirements

Before using this module, ensure the following AWS resources already exist:
- A **VPC** with at least two **private subnets**.
- **AWS Secrets Manager** entries for each Redis user credential you plan to reference.

---

## Providers

| Name | Version |
|------|----------|
| <a name="provider_aws"></a> aws | >= 5.48.0 |

---

## Modules

No submodules used.

---

## Resources

| Name | Type |
|------|------|
| aws_kms_key.redis | aws_kms_key |
| aws_kms_alias.redis | aws_kms_alias |
| aws_security_group.redis | aws_security_group |
| aws_elasticache_user.redis_user | aws_elasticache_user |
| aws_elasticache_user_group.user_group | aws_elasticache_user_group |
| aws_elasticache_serverless_cache.redis_serverless | aws_elasticache_serverless_cache |
| data.aws_secretsmanager_secret.redis_user | data source: aws_secretsmanager_secret |
| data.aws_secretsmanager_secret_version.redis_user_version | data source: aws_secretsmanager_secret_version |

---

## Inputs

| Name | Description | Type | Default | Required |
|------|--------------|------|----------|:--------:|
| <a name="input_environment"></a> environment | Environment name (e.g. `dev`, `stage`, `prod`) | `string` | n/a | yes |
| <a name="input_redis_service_name"></a> redis_service_name | Redis service name used in naming resources | `string` | n/a | yes |
| <a name="input_vpc_id"></a> vpc_id | VPC ID for Redis security group | `string` | n/a | yes |
| <a name="input_subnet_ids"></a> subnet_ids | List of subnet IDs for the Redis serverless cluster | `list(string)` | n/a | yes |
| <a name="input_redis_ports"></a> redis_ports | List of TCP ports allowed in Redis SG | `list(number)` | `[6379]` | no |
| <a name="input_redis_ingress_cidr_blocks"></a> redis_ingress_cidr_blocks | List of CIDR blocks allowed inbound access | `list(string)` | `["0.0.0.0/0"]` | no |
| <a name="input_redis_user_secrets"></a> redis_user_secrets | Map of Redis users, each referencing a Secrets Manager secret and access string | `map(object({ secret_name = string, access_string = string }))` | `{}` | no |

---

## Outputs

| Name | Description |
|------|--------------|
| <a name="output_redis_serverless_arn"></a> redis_serverless_arn | ARN of the Redis Serverless cluster |
| <a name="output_redis_serverless_endpoint"></a> redis_serverless_endpoint | Endpoint address of the Redis Serverless cluster |


---
