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

[CONSTRAINTS]
- Use the "<your-connection-name>" Snowflake connection
- Deploy to <YOUR_DATABASE> with <YOUR_ROLE> role
- Use <YOUR_WAREHOUSE> warehouse for runtime queries
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
- The SPCS service identity has no default warehouse injected via environment variable. Ensure the querySnowflake helper passes `warehouse: "<YOUR_WAREHOUSE>"` in the Snowflake connection config (do NOT pass undefined or omit it, as that bypasses the QUERY_WAREHOUSE fallback on the Application Service)

[OUTCOME]
A fully deployed, shareable React dashboard. Deploy using `snow app deploy --connection <your-connection-name>`, then poll the service status until it reaches RUNNING or errors out. Provide the live URL when ready.
