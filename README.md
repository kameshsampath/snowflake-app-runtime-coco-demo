# Customer Churn Dashboard with CoCo and Snowflake App Runtime

Build a customer churn risk dashboard from scratch using Cortex Code (CoCo) and Snowflake App Runtime. No frontend experience required -- just describe what you want using an IDD-structured prompt.

## Prerequisites

- [Cortex Code Desktop](https://docs.snowflake.com/en/user-guide/ui-cortex-code/cortex-code-desktop) installed and signed in to your Snowflake account
- [Snowflake CLI](https://docs.snowflake.com/en/developer-guide/snowflake-cli/installation/installation) v3.19 or later
- Node.js 22+
- A paid Snowflake account (trial accounts do not support App Runtime)

## Setup

Mount `SNOWFLAKE_SAMPLE_DATA` if it is not already in your account:

```sql
-- Run as ACCOUNTADMIN
CREATE DATABASE IF NOT EXISTS SNOWFLAKE_SAMPLE_DATA
  FROM SHARE SFSALESSHARED.SFC_SAMPLES_AWSAPSOUTH1.SAMPLE_DATA;

GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE_SAMPLE_DATA TO ROLE <your_role>;
```

## The IDD Prompt

Open CoCo Desktop, type `/snowflake-apps`, and paste the contents of [`idd-churn-dashboard.md`](idd-churn-dashboard.md) included in this gist.

CoCo will scaffold the app, wire up Snowflake data access, build, and deploy.

## Follow-Up (iterative refinement)

Once the app is live, send this follow-up message:

```
Add a "Download Report" button that exports the currently filtered churn data as CSV. Redeploy the app.
```

## Troubleshooting

Check deploy status:

```bash
snow app events --last 100
```

Check if the Application Service is running:

```sql
SHOW APPLICATION SERVICES IN SCHEMA <your_database>.<your_schema>;
DESCRIBE APPLICATION SERVICE <your_app_name>;
```

View container logs:

```bash
snow app events --last 500
```

Retry a failed deploy phase:

```bash
snow app deploy --upload-only   # re-upload source
snow app deploy --build-only    # re-trigger build
snow app deploy --deploy-only   # re-create/upgrade service
```

Tear down and start fresh:

```bash
snow app teardown --force
```

## Notes

- First deploy takes 3-5 minutes (SPCS cold start). Subsequent deploys are faster (service upgrade).
- `TPCDS_SF10TCL.STORE_SALES` has 28B rows. If queries are slow, scope to a subset of years (2001-2003 have concentrated data).
- The SPCS service identity has no default warehouse. Ensure your app code passes the warehouse explicitly in the Snowflake connection config.

## Reference Links

- [Snowflake App Runtime docs](https://docs.snowflake.com/en/developer-guide/snowflake-app-runtime/about-snowflake-app-runtime)
- [Getting started with Snowflake App Runtime](https://docs.snowflake.com/en/developer-guide/snowflake-app-runtime/getting-started)
- [Cortex Code Desktop](https://docs.snowflake.com/en/user-guide/ui-cortex-code/cortex-code-desktop)
- [Snowflake CLI install](https://docs.snowflake.com/en/developer-guide/snowflake-cli/installation/installation)
- [Account admin setup for App Runtime](https://docs.snowflake.com/en/developer-guide/snowflake-app-runtime/account-admin-setup)

## IDD Blog Series

Intent-Driven Development (IDD) -- the prompting pattern used in this demo: <https://dev.to/kameshsampath/series/41588>
