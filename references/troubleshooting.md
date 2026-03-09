# Error patterns and MCP fixes

## OOMKilled (exit code 137)

Memory limit exceeded. Increase memory allocation.

```
applications_settings_update → memory: 1Gi (then 2Gi if it happens again)
```

Use `applications_resources_update` with `memory_request` parameter.

Escalation path: 1Gi → 2Gi → 4Gi

## Missing PHP extension

Build fails with "requires ext-xxx". Add the extension.

```
applications_php-config_update → extensions: [xxx]
```

Built-in extensions (do not add): opcache, pcntl, pdo_mysql, pdo_pgsql, redis, sodium, zip.

Common missing extensions: bcmath, gd, gmp, imagick, intl, exif, sockets.

## Build failure — composer

- Wrong PHP version → `applications_php-config_update` with correct version
- Missing system dependency → add extension that provides it
- Memory exhausted during install → increase memory

## Build failure — npm/node

- Wrong Node.js version → check `engines.node` in package.json
- `npm ci` fails → try `npm install` instead
- pnpm project → use `/tmp` prefix approach:
  ```
  npm install --prefix /tmp pnpm && export PATH="/tmp/node_modules/.bin:$PATH" && pnpm install && pnpm build
  ```
- `corepack enable` fails → not available in build image, use the `/tmp` approach above

## Health check failure

App builds and starts but health check at `/` returns non-200.

```
applications_settings_update → health_check_path: /health (or /api/health, /up, etc.)
```

Common paths: `/`, `/health`, `/up`, `/api/health`, `/healthz`

## Missing environment variable

App crashes because a required env var is not set.

```
applications_secrets_store → key: VAR_NAME, value: appropriate_value
```

Check `.env.example` for expected variables. Never add variables that are auto-injected by services.

## Database connection error

App can't connect to database.

1. Verify the service exists: `v1_applications_services_index`
2. If missing: `v1_applications_services_store`
3. If exists: check `applications_secrets_injected` to confirm env vars are injected
4. Redeploy after adding service — env vars are injected on deploy

## Deployment status vs application status

- Deployment `running` = build is still in progress (not live yet)
- Application `running` = pods are up and serving traffic
- Always check `applications_status` for actual app state, not deployment status

## Build logs truncated

Use `tail: 2000` with `applications_deployments_logs` to get more context. The actual error is usually above the "did not complete successfully" line.

## Build vs runtime user

Build commands run as root (full access). The application runtime runs as www-data (non-root, workdir `/var/www`).

`npm install -g` and `corepack enable` work during the build phase. For pnpm projects, you can either use `corepack enable` or install via `/tmp`.
