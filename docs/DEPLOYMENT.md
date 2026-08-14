# Deployment Blueprint

This document describes the target deployment topology. The Aegis application image and source are not public yet; PostgreSQL, Redis, and object storage can be started independently from the example Compose file.

## Environments

| Environment | Purpose | Storage | Model policy |
|---|---|---|---|
| Development | Local configuration and integration | Docker volumes | Approved development provider |
| Staging | Workflow and security validation | Managed or isolated | Production-equivalent with synthetic data |
| Production | Controlled contract review | Encrypted managed services | Organization-approved gateway only |

## Prepare configuration

```bash
cp .env.example .env
docker compose -f deploy/docker-compose.example.yml config
```

Replace every `change-me` value. Inject database, storage, model, and identity credentials through a secret manager rather than Git.

## Start dependencies

```bash
docker compose -f deploy/docker-compose.example.yml up -d postgres redis object-storage
docker compose -f deploy/docker-compose.example.yml ps
```

## Start application services

Reserved for the implementation release:

```bash
docker compose -f deploy/docker-compose.example.yml --profile app up -d
```

The default `unreleased` image tag intentionally prevents the deployment blueprint from appearing runnable before the application is published.

## Production controls

- Use managed PostgreSQL, Redis, and S3-compatible storage with backups.
- Encrypt contract documents, artifacts, queues, and database connections.
- Separate tenant storage prefixes, encryption keys, and authorization scopes.
- Restrict model traffic to an approved gateway and selected regions.
- Disable contract-text logging and redact traces.
- Scan uploads and enforce type, size, and page limits.
- Configure retention, deletion, backup, recovery, and legal hold.
- Maintain an immutable audit trail of rules, prompts, models, evidence, and reviewer actions.

## Validation checklist

```text
[ ] Compose configuration resolves successfully
[ ] PostgreSQL, Redis, and storage health checks pass
[ ] Secrets are outside version control
[ ] Upload scanning and file limits are configured
[ ] Model gateway and data-region policies are approved
[ ] Source-text logging is disabled
[ ] Tenant isolation is verified
[ ] Backup and restore are tested
[ ] Retention and deletion jobs are active
[ ] Audit export is available
```

