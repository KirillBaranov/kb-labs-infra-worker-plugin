# KB Labs Infra Worker Plugin

Infra worker plugin for capability-first infrastructure operations in KB Labs.

It is intended to be invoked by an orchestrator plugin. The worker performs:
- workspace materialization
- optional environment provisioning
- snapshot capture/restore

## Package Layout

- `packages/plugin-template-core` (`@kb-labs/infra-worker-core`): manifest + handlers/commands
- `packages/plugin-template-contracts` (`@kb-labs/infra-worker-contracts`): shared invoke contracts

## Plugin ID

`@kb-labs/infra-worker`

## Main Commands

- `infra-worker:prepare`
  - materialize workspace
  - optional environment create + attach
  - optional snapshot capture
  - returns IDs (`workspaceId`, `environmentId`, `snapshotId`)
- `infra-worker:capture-snapshot`
- `infra-worker:restore-snapshot`

## Development

```bash
pnpm install
pnpm build
```

Build a single package:

```bash
pnpm --filter @kb-labs/infra-worker-core build
pnpm --filter @kb-labs/infra-worker-contracts build
```

## Orchestrator Flow

1. Orchestrator gets task payload.
2. Orchestrator calls `ctx.api.invoke.call('@kb-labs/infra-worker', payload)`.
3. Infra worker executes capability APIs (`ctx.api.workspace`, `ctx.api.environment`, `ctx.api.snapshot`).
4. Infra worker returns stable IDs.
5. Orchestrator continues business pipeline using these IDs.
