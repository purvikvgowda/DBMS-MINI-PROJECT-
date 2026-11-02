# PantryPal — DBMS Mini Project

A pantry management and expiry-aware recipe recommendation web application. PantryPal helps users track ingredients, prioritise items nearing expiry, discover recipes based on pantry contents, plan meals, and review SQL and MongoDB reports.

> **Project:** Database Management Systems Mini Project  
> **Institution:** PES University  
> **Team:** Pranav V Menon (PES1UG24AM198) and Purvik V Gowda (PES1UG24AM211)

## Features

- User registration and sign-in with role-aware user/admin access.
- Personal pantry management and expiry-aware ingredient tracking.
- Recipe ranking based on pantry coverage and ingredient urgency.
- Missing-ingredient lists and shopping support.
- Weekly meal planning.
- PostgreSQL-backed SQL reports and optional MongoDB reports.
- Responsive web interface with refreshed sign-in and dashboard styling.

## Technology stack

- **Frontend:** HTML, CSS, vanilla JavaScript
- **Backend:** Node.js and Express
- **Relational database:** PostgreSQL (`pg`)
- **Document database:** MongoDB (`mongodb` Node driver; optional for core PostgreSQL features)
- **Authentication:** Node.js `crypto` scrypt password hashing and database-backed sessions

## Repository structure

```text
.
├── public/                   # Browser app, styles, login/register pages
├── lib/                      # Authentication and password helpers
├── pantrypal_code/            # PostgreSQL schema, seed, reports, Mongo scripts
│   └── migrations/            # Non-destructive authentication migration
├── scripts/                  # Demo-auth setup and ER diagram generation
├── docs/                     # Database design, schema model, changelog
├── PantryPal_Report_Assets/   # Report figures and ER diagrams
├── tests/                    # Automated tests
├── server.js                 # Express application entry point
└── PantryPal_DBMS_Project_Report.md
```

## Prerequisites

- Node.js (npm included)
- PostgreSQL
- MongoDB is optional; without it, the main PostgreSQL features remain available

## Setup

1. Clone this repository and enter its root folder:

   ```bash
   git clone https://github.com/purvikvgowda/DBMS-MINI-PROJECT-.git
   cd DBMS-MINI-PROJECT-
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Configure PostgreSQL using environment variables in your terminal. For example (PowerShell):

   ```powershell
   $env:PGHOST = "localhost"
   $env:PGPORT = "5432"
   $env:PGUSER = "postgres"
   $env:PGDATABASE = "pantrypal"
   $env:PGPASSWORD = "<your-local-postgres-password>"
   ```

4. For an **existing PantryPal database**, apply the authentication migration. It adds authentication/session objects without running the destructive fresh-schema reset:

   ```bash
   psql -d pantrypal -v ON_ERROR_STOP=1 -f pantrypal_code/migrations/001_auth.sql
   ```

5. Start the application:

   ```bash
   npm start
   ```

6. Open [http://localhost:3001](http://localhost:3001). Register a user account or sign in with an existing account.

### Fresh database only

For a brand-new database, create `pantrypal` and apply `pantrypal_code/schema.sql` followed by `pantrypal_code/seed.sql`. **Warning:** `schema.sql` drops and recreates project tables. Do not run it against an existing database unless you intentionally want to reset that database.

```bash
createdb pantrypal
psql -d pantrypal -v ON_ERROR_STOP=1 -f pantrypal_code/schema.sql
psql -d pantrypal -v ON_ERROR_STOP=1 -f pantrypal_code/seed.sql
npm run setup:demo
npm start
```

The demo setup script generates local credentials in `DEMO_LOGIN.local.txt`; that file is gitignored and must never be committed or shared.

## Optional MongoDB setup

Set `MONGO_URL` and `MONGO_DATABASE` if using a non-default MongoDB instance. Defaults are `mongodb://127.0.0.1:27017` and `pantrypal_nosql`. The Mongo initialization script drops and reseeds its project collections, so run it only when you intentionally want to initialize/reset those collections:

```bash
mongosh --file pantrypal_code/mongo_setup.js
```

Run MongoDB reports with:

```bash
mongosh --file pantrypal_code/mongo_reports.js
```

## Tests and diagrams

```bash
npm test
npm run diagrams
```

The automated tests use temporary test databases; review `tests/auth.test.js` and the project report for test setup and coverage.

## Security and configuration notes

- Do not commit `.env` files, database passwords, generated demo credentials, or other secrets.
- PostgreSQL settings are read from standard `PGHOST`, `PGPORT`, `PGUSER`, `PGPASSWORD`, and `PGDATABASE` environment variables.
- `PORT` defaults to `3001`.
- Set `NODE_ENV=production` when deploying behind HTTPS so secure session cookies are enabled.
- This is a student project; review deployment security, rate limiting, backups, and secret management before exposing it publicly.

## Documentation

- [Project report](PantryPal_DBMS_Project_Report.md)
- [Database design](docs/database-design.md)
- [Changelog](docs/CHANGELOG.md)
- [Chen ER diagram (SVG)](PantryPal_Report_Assets/er-diagram.svg)

## License

No license has been specified. All rights are reserved by the project authors unless a license is added.
