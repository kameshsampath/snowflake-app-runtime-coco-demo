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
- Deploy as a Snowflake App Runtime app
- No external data sources
- Must work with sample data any Snowflake user already has

[OUTCOME]
A fully deployed, shareable React dashboard. Deploy using `snow app deploy` and provide the live URL when ready.
