# Terraform Module: Redis Serverless (AWS ElastiCache)

This Terraform module provisions a **Redis Serverless cluster** in AWS using ElastiCache, including:

- KMS key for encryption
- Security group for controlled access
- Redis users and user groups from Secrets Manager
- Redis Serverless cluster with configurable snapshot and engine version

---

## Resources Created

| Resource | Description |
|----------|-------------|
| `aws_kms_key.redis` | KMS key for encrypting Redis cluster data |
| `aws_kms_alias.redis` | Alias for the Redis KMS key |
| `aws_security_group.redis` | Security group controlling inbound/outbound access to Redis |
| `aws_elasticache_user` | Redis users created from Secrets Manager secrets |
| `aws_elasticache_user_group` | User group containing all Redis users |
| `aws_elasticache_serverless_cache.redis_serverless` | Serverless Redis cluster |

---

## Variables

| Name | Type | Default | Description |
|------|------|---------|-------------|
| `environment` | string | n/a | Deployment environment name (e.g., dev, prod, stage) |
| `redis_service_name` | string | n/a | Logical name for the Redis service (e.g., oms, cart, session) |
| `vpc_id` | string | n/a | VPC ID for Redis security group |
| `subnet_ids` | list(string) | n/a | Subnet IDs for the Redis cluster |
| `redis_user_secrets` | map(object) | n/a | Map of Redis user names to Secrets Manager paths and access strings |
| `redis_sg_name` | string | `"redis-sg"` | Name of the Redis security group |
| `redis_ports` | list(number) | `[6379]` | Redis ports to allow inbound access |
| `redis_ingress_cidr_blocks` | list(string) | `["10.0.0.0/16"]` | CIDR blocks allowed to access Redis |

---

## Outputs

| Name | Description |
|------|-------------|
| `redis_cluster_name` | The name of the Redis Serverless cluster |
| `user_group_id` | Redis user group ID containing all users |

---


