# DB Schema Explorer

A lightweight single-page database schema explorer UI for visualizing **tables**, **columns**, and **relationships** from a connected database.

It is designed for teams that need quick visibility into:
- Total number of tables in a database
- Total number of columns across the schema
- Columns per table
- Foreign key relationships between tables
- Table-level statistics and metadata

> **Note**
> The current UI is frontend-first and includes a demo dataset. Browsers cannot connect to JDBC directly, so real JDBC connectivity requires a small backend proxy service.

## Features

- Clean one-page UI
- Database type selector
- JDBC connection form
- Overview dashboard with schema statistics
- Table search and navigation sidebar
- Per-table detail view
- Column list with PK/FK indicators
- Incoming and outgoing relationship view
- Light and dark theme support
- Demo mode for quick testing

## Screenshots

> Add your screenshots into `assets/` and keep these file names, or update the paths below.

### Overview Dashboard
<img width="1917" height="952" alt="image" src="https://github.com/user-attachments/assets/60ad1777-6144-4041-b490-0c0a0387b519" />



### Table Detail View

<img width="1902" height="947" alt="image" src="https://github.com/user-attachments/assets/09e1af20-dd52-478e-ae24-d1d6c04c9e1d" />


### Sidebar and Search

<img width="325" height="949" alt="image" src="https://github.com/user-attachments/assets/a5f8efa9-7f98-4eb2-bba8-339c086ff645" />


## What It Shows

### Database-level metrics
- Number of tables
- Total number of columns
- Total foreign key relationships
- Estimated total row count

### Table-level metrics
- Column count
- Estimated row count
- Primary key count
- Foreign key count
- Nullable column count
- Index count

### Relationship visibility
- Outgoing references: which tables a table points to
- Incoming references: which tables point to the selected table

## Project Structure

```text
.
├── db-schema-explorer.html
├── README.md
└── assets/
    ├── db-schema-overview.png
    ├── db-schema-table-detail.png
    └── db-schema-sidebar-search.png
```

## Run Locally

Since this is a static HTML app, you can run it with any simple web server.

### Option 1: Open directly
Open `db-schema-explorer.html` in your browser.

### Option 2: Serve locally with Python

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000/db-schema-explorer.html
```

## JDBC Connection Support

The current UI includes a JDBC connection form, but a browser cannot open a JDBC connection directly.

To connect to a real database, add a backend service that:
1. Accepts database type, JDBC URL, username, and password
2. Connects to the database using JDBC or a server-side driver
3. Extracts metadata from `information_schema`, JDBC metadata APIs, or system catalog tables
4. Returns schema statistics as JSON to the frontend

Typical backend responsibilities:
- List tables
- List columns for each table
- Discover primary keys
- Discover foreign keys
- Count tables and columns
- Build relationship maps
- Optionally estimate row counts

## Recommended Metadata Queries

Typical sources for extracting metadata:
- PostgreSQL: `information_schema`, `pg_catalog`
- MySQL: `information_schema`
- SQL Server: `INFORMATION_SCHEMA`, `sys.tables`, `sys.columns`, `sys.foreign_keys`
- Oracle: `ALL_TABLES`, `ALL_TAB_COLUMNS`, `ALL_CONSTRAINTS`, `ALL_CONS_COLUMNS`

## Suggested Backend Stack

You can pair this UI with any of the following:
- Java + Spring Boot + JDBC
- Node.js + Express + database drivers
- Python + FastAPI + SQLAlchemy / native drivers

## Example API Shape

```json
{
  "name": "ecommerce_prod",
  "dbType": "PostgreSQL",
  "schema": "public",
  "tables": [
    {
      "name": "orders",
      "rowCount": 328940,
      "indexes": 4,
      "columns": [
        {
          "name": "id",
          "type": "BIGSERIAL",
          "nullable": false,
          "pk": true
        }
      ],
      "foreignKeys": [
        {
          "column": "user_id",
          "refTable": "users",
          "refColumn": "id"
        }
      ],
      "referencedBy": ["order_items", "payments"]
    }
  ]
}
```

## Open Source Alternatives

If you want an existing open-source solution instead of maintaining your own UI, these are good options:
- **Azimutt** for interactive schema exploration
- **SchemaSpy** for generated HTML documentation
- **DBeaver** for rich desktop inspection
- **ChartDB** for quick visual schema diagrams

## Roadmap Ideas

- Real backend integration for JDBC connections
- ER diagram canvas view
- Export schema statistics to CSV / JSON
- Filter by schema name
- Search columns, not just tables
- Relationship graph visualization
- Table size heatmap
- Multi-database connection profiles
- Authentication and access control
