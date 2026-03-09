# Ploi Cloud Deploy Skill

A [Claude Code](https://claude.ai/code) skill that deploys your project to [Ploi Cloud](https://ploi.cloud) with a single command.

## What it does

Run `/pc-deploy` from any project directory and the skill will:

1. Detect your project type (Laravel, Node.js, WordPress, PHP, Statamic, Craft CMS)
2. Extract runtime versions, extensions, and required services
3. Create or update the application on Ploi Cloud
4. Configure build commands, environment variables, and databases
5. Deploy, monitor, and auto-fix failures
6. Report the live URL when done

## Prerequisites

- A [Ploi Cloud](https://ploi.cloud/register) account
- The Ploi Cloud MCP server connected:
  ```bash
  claude mcp add ploi-cloud --url https://ploi.cloud/mcp
  ```

## Install

Global (available in all projects):

```bash
claude skill add --global https://github.com/ploicloud/skill
```

Per-project:

```bash
claude skill add https://github.com/ploicloud/skill
```

## Usage

From your project directory in Claude Code:

```
/pc-deploy
```

The skill requires a git repository with a remote URL (GitHub, GitLab, or Bitbucket).

## Supported project types

| Type | Detection |
|------|-----------|
| Laravel | `composer.json` with `laravel/framework` |
| Statamic | `composer.json` with `statamic/cms` |
| Craft CMS | `composer.json` with `craftcms/cms` |
| WordPress | `wp-config.php` |
| PHP | `composer.json` (generic) |
| Node.js | `package.json` |

## Auto-detected services

| Dependency | Service |
|------------|---------|
| `mysql2`, Prisma with MySQL | MySQL |
| `pg`, `postgres`, Prisma with PostgreSQL | PostgreSQL |
| `mongodb`, `mongoose` | MongoDB |
| `predis/predis`, `ext-redis`, `redis`, `ioredis` | Redis |
| `php-amqplib`, `amqplib` | RabbitMQ |

## Auto-fix

When a deployment fails, the skill diagnoses the error and applies a fix automatically (up to 5 retries):

- Out of memory &rarr; increases memory allocation
- Missing PHP extension &rarr; adds it to the config
- Build failure &rarr; adjusts build commands
- Health check failure &rarr; changes the health check path
- Missing environment variable &rarr; adds the required secret

## Documentation

- [Getting started with AI deployment](https://ploi.cloud/documentation/ai/getting-started-with-ai)
- [Deploy skill reference](https://ploi.cloud/documentation/ai/deploy-skill)
- [Available AI tools](https://ploi.cloud/documentation/ai/available-tools)
