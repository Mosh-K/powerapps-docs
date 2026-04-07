---
title: "How to: Add a Dataverse action or function to your code app"
description: "Learn how to add Dataverse actions and functions to a Power Apps code app using the CLI"
ms.author: pakempar
author: pavankm
ms.date: 04/03/2026
ms.topic: how-to
ms.reviewer: 
---

# How to: Add a Dataverse action or function to your code app

This guide walks you through discovering and adding Dataverse operations (actions and functions)
to a Power Apps code app using the `find-dataverse-api` and `add-dataverse-api` CLI commands.

> [!NOTE]
> This feature is available only in the latest npm CLI (preview), not in the classic Power Apps CLI (pac CLI).
> For more information, see [Quickstart with npm CLI](npm-quickstart.md).

## Prerequisites

- A Power Apps code app initialized with `npx power-apps init`
- Authenticated CLI session (`npx power-apps` prompts if needed)
- Access to the Dataverse environment containing the operation you want to use

## Step 1: Discover available operations

Use `find-dataverse-api` to search for operations in your environment by name:

```bash
npx power-apps find-dataverse-api --search "opportunity"
```

The output lists matching operations with their kind (Action or Function), parameters, binding
entity (if any), and return type:

```
====================================================================================================
Dataverse Operations
====================================================================================================

  WinOpportunity  (Action)
  Bound to: mscrm.opportunity
  Parameters:
    - Status: Edm.Int32
    - Target: mscrm.opportunity

  RetrieveOpportunities  (Function)
  Parameters:
    - OpportunityId?: Edm.Guid
  Returns: Collection(mscrm.opportunity)

----------------------------------------------------------------------------------------------------
Total: 2 operation(s)
====================================================================================================
```

To get the raw JSON (useful for scripting and for coding agents), add `--json`:

```bash
npx power-apps find-dataverse-api --search "opportunity" --json
```

The search is a case-insensitive substring match on the operation name.

## Step 2: Add the operation to your app

Once you know the operation name, run:

```bash
npx power-apps add-dataverse-api --api-name WinOpportunity
# or using the short alias:
npx power-apps add-dataverse-api -n WinOpportunity
```

The command:

1. Fetch the operation definition from your environment's Dataverse `$metadata`.
2. Write a schema file at `<schemaPath>/dataverse/WinOpportunity.Schema.json`.
3. Save schema files for any Dataverse entities referenced by the operation's parameters or return
   type (skipped if they already exist).
4. Update `power.config.json`:
   - Add `databaseReferences["default.cds"]` if not already present.
   - For bound operations, add the binding entity to `dataSources` if not already present.
5. Regenerate `dataSourcesInfo.ts` to include the new operation.
6. Generate a TypeScript model and service class under `<codeGenPath>/generated/`.

On success, you see:

```
Dataverse API 'WinOpportunity' added successfully.
Hint: Run 'npx power-apps run' to test locally, or 'npx power-apps push' to deploy.
```

## Step 3: Use the generated service in your app code

The command generates a dedicated `<ApiName>Service` class for the operation. For example,
after adding `WinOpportunity`, import `WinOpportunityService` from the generated services
directory:

```typescript
import { WinOpportunityService } from './generated/services/WinOpportunityService';
```

The service exposes a typed static method named after the operation. For example:

```typescript
const result = await WinOpportunityService.WinOpportunity(
  opportunityId,   // string (GUID of the bound opportunity record)
  status,          // number
  target           // Record<string, unknown>
);
```

Parameter and return types are derived from the Dataverse schema:

- GUID parameters are typed as `string`.
- Lookup parameters that reference a Dataverse entity are typed as `Record<string, unknown>`.
- Operations with no return value produce `Promise<IOperationResult<void>>`.
- Operations that return a scalar (for example, `boolean`, `number`) produce
  `Promise<IOperationResult<T>>` with the corresponding TypeScript type.
- Operations that return an entity or collection produce `Promise<IOperationResult<Record<string, unknown>>>`.

## Rerunning for the same operation

Running `add-dataverse-api` again with the same `--api-name` is safe and idempotent:

- The schema file is overwritten with the latest definition from Dataverse.
- `dataSourcesInfo.ts` is regenerated.
- `power.config.json` entries are deduped—no duplicates are written.
- Entity schema files that already exist are **not** overwritten.

Use this command to pick up changes to an operation's signature after an update in the
environment.

## Files created or modified

| File                                                   | What changes                                                           |
| ------------------------------------------------------ | ---------------------------------------------------------------------- |
| `<schemaPath>/dataverse/<ApiName>.Schema.json`         | Created or overwritten—operation schema                                |
| `<schemaPath>/dataverse/<EntityName>.Schema.json`      | Created (skipped if already exists)—schemas for referenced entities    |
| `<schemaPath>/appschemas/dataSourcesInfo.ts`           | Regenerated to include the new operation                               |
| `power.config.json`                                    | Updated with `default.cds` reference and (if bound) the binding entity |
| `<codeGenPath>/generated/models/<EntityName>Model.ts`  | Generated TypeScript models for referenced entity data sources         |
| `<codeGenPath>/generated/services/<ApiName>Service.ts` | Generated service class (one file per operation)                       |

## Bound vs. unbound operations

**Bound operations** are scoped to a specific entity record. The generated method always takes an
`id: string` parameter as its first argument, which is the GUID of the record to operate on.

**Unbound operations** are environment-wide and do not require a record ID. Their methods take
only the declared parameters.

Both kinds are added with the same command—the CLI detects the operation type automatically.

## Troubleshooting

**"No operations found"**—the search is substring-based and case-insensitive, but only matches
the operation name. Try a shorter or different term. Use `--json` to confirm the raw response.

**Stale generated files**—if you renamed or removed an operation and the old generated files
remain, delete them manually and rerun `add-dataverse-api` to regenerate.

**`pac code add-data-source` skips the action or function**—the schema files
generated by `add-dataverse-api` use a `Microsoft.PowerApps/dataverseOperation` type that is not
recognized by the PAC CLI. If any operation schema files are present in the Dataverse schema
folder, `pac code add-data-source` skips them with:

```
Skipping unsupported Dataverse operation schema '...AddRecurrence.Schema.json':
```

This behavior means the PAC CLI doesn't support the `Microsoft.PowerApps/dataverseOperation`
schema type, so it skips these files instead of failing.
The PAC CLI still supports the other schema files that represent Dataverse entity data sources or
connectors, and you can add them by using `pac code add-data-source`.
