# Auto-injected environment variables by service type

These variables are automatically injected into the application when a service is attached. NEVER add these as secrets — they are managed by the platform via ConfigMap.

## MySQL

- `DB_CONNECTION`
- `DB_HOST`
- `DB_PORT`
- `DB_DATABASE`
- `DB_USERNAME`
- `DB_PASSWORD`

WordPress apps get `WORDPRESS_DB_HOST`, `WORDPRESS_DB_NAME`, `WORDPRESS_DB_USER`, `WORDPRESS_DB_PASSWORD` instead.

## PostgreSQL

- `DB_CONNECTION`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`
- `POSTGRES_HOST`, `POSTGRES_PORT`, `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`
- `POSTGRESQL_URL`, `POSTGRES_URL`

## Redis

- `REDIS_URL`
- `REDIS_HOST`
- `REDIS_PORT`
- `REDIS_PASSWORD`

## Valkey

- `VALKEY_URL`
- `VALKEY_HOST`
- `VALKEY_PORT`
- `VALKEY_PASSWORD`

## MongoDB

- `MONGODB_URI`
- `MONGODB_HOST`
- `MONGODB_PORT`
- `MONGODB_DATABASE`
- `MONGODB_USERNAME`
- `MONGODB_PASSWORD`

## RabbitMQ

- `RABBITMQ_URL`
- `RABBITMQ_HOST`
- `RABBITMQ_PORT`
- `RABBITMQ_MANAGEMENT_PORT`
- `RABBITMQ_USERNAME`
- `RABBITMQ_PASSWORD`
- `RABBITMQ_VHOST`

Plugin-specific port variables are also injected when plugins are enabled (e.g. `RABBITMQ_STOMP_PORT`).

## MinIO

- `MINIO_ENDPOINT` (host:port combined)
- `MINIO_PUBLIC_ENDPOINT`
- `MINIO_CDN_URL`
- `MINIO_ROOT_USER`
- `MINIO_ROOT_PASSWORD`
- `MINIO_BUCKET`
- `AWS_ENDPOINT_URL`
- `AWS_PUBLIC_ENDPOINT_URL`
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_DEFAULT_REGION`
- `AWS_USE_PATH_STYLE_ENDPOINT`

## SFTP

- `SFTP_HOST`
- `SFTP_PORT`
- `SFTP_USERNAME`
- `SFTP_PASSWORD`
- `SFTP_APP_PATH`
- `SFTP_VOLUMES_PATH`
- `SFTP_AVAILABLE_PATHS`

## Non-standard variable mapping

`${VAR}` interpolation in secrets only resolves other user secrets, NOT service-injected vars. If an app needs a non-standard env var mapped from a service var, use shell expansion in the start command:

```
DATABASE_URL=$POSTGRESQL_URL npx start
```
