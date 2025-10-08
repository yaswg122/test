# Terraform Module: ElastiCache Serverless (AWS)

This Terraform module provisions an **ElastiCache Serverless cluster** (Redis) in AWS, including:

- KMS key for encryption
- Security group for controlled access
- ElastiCache users and user groups from Secrets Manager
- Serverless cluster with configurable snapshot and engine version

---

## Requirements

Before deploying this module, the following resources must exist:

- **VPC** with at least two private **subnets** in different Availability Zones.

---

## Providers

| Name | Version |
|------|---------|
| aws  | >= 5.48.0 |

---

## Resources Created

| Resource | Description |
|----------|-------------|
| `aws_kms_key.redis` | KMS key for encrypting ElastiCache cluster data |
| `aws_kms_alias.redis` | Alias for the KMS key |
| `aws_security_group.redis` | Security group controlling inbound/outbound access to the cluster |
| `aws_elasticache_user.redis_user` | Users created from AWS Secrets Manager secrets |
| `aws_elasticache_user_group.user_group` | User group containing all users |
| `aws_elasticache_serverless_cache.redis_serverless` | Serverless ElastiCache cluster |

---

## Variables

| Name | Type | Default | Description |
|------|------|---------|-------------|
| `environment` | string | n/a | Deployment environment name (e.g., dev, prod, stage) |
| `redis_service_name` | string | n/a | Logical name for the ElastiCache service (e.g., oms, cart, session) |
| `vpc_id` | string | n/a | VPC ID for ElastiCache security group |
| `subnet_ids` | list(string) | n/a | Subnet IDs for the ElastiCache cluster |
| `redis_user_secrets` | map(object) | n/a | Map of ElastiCache user names to Secrets Manager paths and access strings |
| `redis_sg_name` | string | `"redis-sg"` | Name of the ElastiCache security group |
| `redis_ports` | list(number) | `[6379]` | Ports to allow inbound access to the cluster |
| `redis_ingress_cidr_blocks` | list(string) | `["10.0.0.0/16"]` | CIDR blocks allowed to access the cluster |

---

## Outputs

| Name | Description |
|------|-------------|
| `redis_cluster_name` | The name of the ElastiCache Serverless cluster |
| `user_group_id` | ElastiCache user group ID containing all users |

---
