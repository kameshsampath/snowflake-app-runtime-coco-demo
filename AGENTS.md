# Pre-checks

Before executing any IDD file in this repo, validate these assumptions. Stop and report if any check fails.

## Project layout

- All app code lives in `churn-dashboard/` (not the repo root)
- The repo root contains only docs, the IDD file, and this AGENTS.md
- Do NOT scaffold, install, or build in the repo root -- always work inside `churn-dashboard/`

## Skip checks

- Do NOT check or validate the Snowflake CLI version -- it is already at the required version

## snowflake.yml generation

- Run `snow app setup` from inside `churn-dashboard/` (the project directory), NOT from the repo root
- After generation, update `snowflake.yml` to match the variable bindings below (database, warehouse) since `snow app setup` resolves account-level defaults that may differ

## Variable bindings

Replace `{{placeholders}}` in the IDD file with these values.

| Placeholder       | Value              |
|-------------------|--------------------|
| `{{CONNECTION}}`  | `devrel-ent`       |
| `{{ROLE}}`        | `KAMESH_DEMOS`     |
| `{{WAREHOUSE}}`   | `KAMESH_DEMOS_S`   |
| `{{APP_DATABASE}}`| `KAMESH_DEMOS`     |
| `{{APP_SCHEMA}}`  | `PUBLIC`           |
| `{{APP_NAME}}`    | `CHURN_DASHBOARD`  |

## Environment validation

- **Role:** `KAMESH_DEMOS` must be active and have CREATE STAGE, USAGE on `KAMESH_DEMOS.PUBLIC`
- **Warehouse:** `KAMESH_DEMOS_S` must exist and the role must have USAGE on it
- **Sample data:** `SNOWFLAKE_SAMPLE_DATA.TPCDS_SF10TCL` must be accessible to the role

## Validation commands

Run each statement separately (not as a multi-statement batch) so results are visible:

```sql
USE ROLE KAMESH_DEMOS;
```

```sql
USE WAREHOUSE KAMESH_DEMOS_S;
```

```sql
SELECT 1 FROM SNOWFLAKE_SAMPLE_DATA.TPCDS_SF10TCL.CUSTOMER LIMIT 1;
```
