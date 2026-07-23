/snowflake-apps

[GOAL]
Build a customer churn risk dashboard as a Snowflake App (React/Next.js).

[REQUIREMENTS]
- Use SNOWFLAKE_SAMPLE_DATA.TPCDS_SF10TCL tables:
  - CUSTOMER (customer identity, linked to address and demographics)
  - STORE_SALES (transaction history -- declining frequency signals churn)
  - CUSTOMER_ADDRESS (state/region for geographic filtering)
  - DATE_DIM (resolves date keys to actual dates/quarters)
- Define churn risk as customers with declining quarter-over-quarter purchase frequency
- Summary metric cards: total customers, at-risk count, churn rate %
- Bar chart: churn risk by state/region
- Line chart: churn trend over time (quarterly)
- Filterable by state and time range
- Clean, modern UI with pleasing data visualization
- Scaffold the app into a subdirectory named `churn-dashboard/` (not the repo root)

[CONSTRAINTS]
- Use the {{CONNECTION}} Snowflake connection
- Deploy to {{APP_DATABASE}} with {{ROLE}} role
- Use {{WAREHOUSE}} warehouse for runtime queries
- Deploy as a Snowflake App Runtime app
- No external data sources
- Must work with sample data any Snowflake user already has

[CONSTRAINTS - PERFORMANCE]
- TPCDS_SF10TCL.STORE_SALES has billions of rows; never scan it without tight filters
- Limit customer subset to 50K using a CTE on the small CUSTOMER table first, then join STORE_SALES only for those customers
- Default year range to 2001-2003 (where TPC-DS sales data is concentrated)
- Use querySnowflakeLongRunning for the churn query (warehouse-heavy, 10+ seconds)

[CONSTRAINTS - SPCS RUNTIME]
- Snowflake SDK returns column names in UPPERCASE regardless of SQL aliases; API routes must normalize response keys to lowercase before sending to the frontend
- The SPCS service identity has no default warehouse. You MUST hardcode the warehouse in `lib/snowflake.ts` `baseConfig()` as a fallback: `base.warehouse = process.env.SNOWFLAKE_WAREHOUSE || "{{WAREHOUSE}}"`. Without this, every query fails with "No active warehouse selected in the current session."
- Set `query_warehouse` in `snowflake.yml` to `{{WAREHOUSE}}` (not the account-default `SNOWFLAKE_APPS_QUERY_WH`) so the role running the service has USAGE on it
- Do NOT pass `warehouse` as an option to `querySnowflake()` or `querySnowflakeLongRunning()` -- the helper does not accept it as a per-call option. It must be set globally in `baseConfig()`

[OUTCOME]
A fully deployed, shareable React dashboard. Deploy using `snow app deploy --connection {{CONNECTION}}`, then poll the service status until it reaches RUNNING or errors out. Provide the live URL when ready.

At the end, compute and display the ICR (Intent Compression Ratio):

1. **Intents** -- List each distinct intent statement (numbered, one per line)
2. **Ops** -- List operations grouped by category as a short table:

| Category | Count |
|----------|-------|
| SQL queries | n |
| Files created/modified | n |
| Shell commands | n |
| Deploy pipeline steps | n |
| **Total** | **n** |

3. **Score** -- One line:

**ICR = <ops> / <intents> = <score>** | Tier: <tier name> | E2E: <duration>

Tier scale: 1 = command relay, 4-8 = automation wrapper, 9+ = architectural partner.h the 