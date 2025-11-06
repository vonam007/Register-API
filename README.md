# Register-API

Lightweight Node.js registration API (Express + Sequelize) used to manage users, teams, participants, pins and whitelist entries. This repository implements server endpoints, migrations, seeders and email templates for registration and password reset flows.

## Repository layout

- `src/`
  - `server.js` — app entrypoint
  - `configs/` — configuration helpers (CORS, DB connection, config.json)
  - `controllers/` — request handlers (`apiController.js`, `JWTActions.js`, `checkWhiteList.js`)
  - `services/` — business logic (`apiService.js`) and email templates under `Mail/`
  - `models/` — Sequelize model definitions and `index.js`
  - `migrations/` — Sequelize migrations
  - `seeders/` — database seed files (example: admin user)
  - `Routes/` — API routes (`apiRoutes.js`)

Other files:
- `package.json` — project metadata and scripts
- `user_data.csv` — sample / input data (if used by import scripts)
- `.env_template` — template for environment variables (copy to `.env`)

## Quick overview

Core features
- User registration and authentication via JWT
- Team, participant and process entities
- Pin and whitelist management
- Email templates for registration and resets (in `src/services/Mail/`)
- Sequelize-based migrations and seeders

## Requirements

- Node.js 14+ (recommend latest LTS)
- A relational DB supported by Sequelize (Postgres, MySQL, SQLite). The project uses Sequelize migrations and models (see `src/models/`).

## Setup (Windows, cmd.exe)

1. Clone the repo and open a command prompt in project root.

2. Install dependencies:

```cmd
npm install
```

3. Create `.env` from the template and edit it:

```cmd
copy .env_template .env
rem:: open `.env` in your editor and fill values
```

Recommended environment variables (these live in `.env` — exact names are in `.env_template` in the repo):
- NODE_ENV (development|production)
- PORT (server port)
- DB_HOST, DB_PORT, DB_NAME, DB_USER, DB_PASS (database connection)
- JWT_SECRET (secret used to sign JWTs)
- EMAIL_HOST, EMAIL_PORT, EMAIL_USER, EMAIL_PASS (SMTP for outgoing email)


## Database migrations & seeding (Sequelize)

```cmd
npx sequelize-cli db:migrate
npx sequelize-cli db:seed:all
```

If the project doesn't yet include `sequelize-cli` in devDependencies, add it:

```cmd
npm install --save-dev sequelize-cli
```

## Run the app

Start the server (uses `src/server.js`):

```cmd
node src/server.js
```

If `package.json` has a `start` script, you can also run:

```cmd
npm start
```

The server will listen on the port defined in `.env` or the default port in `src/configs/config.json` (if present).

## API routes and controllers

- Routes are registered in `src/Routes/apiRoutes.js`.
- Handlers live in `src/controllers/apiController.js` (authentication, register, requests, etc.).
- `src/controllers/JWTActions.js` handles JWT verification/creation.
- `src/controllers/checkWhiteList.js` contains whitelist logic.

For exact endpoints and payloads, open `src/Routes/apiRoutes.js` and match to controller methods. If you want, I can extract a full list of endpoints and example requests.

## Email templates

HTML email templates are in `src/services/Mail/`:
- `RegisterTemplate.html` — registration confirmation
- `ResetMailTemplate.html` — password reset

Make sure SMTP settings in `.env` are correct before testing email flows.

## Testing and development notes

- Add a development-only DB or use SQLite for quick local testing (change DB config in `.env`/`src/configs/config.json`).
- When changing models, create migrations and run `npx sequelize-cli db:migrate`.
- Seeders exist under `seeders/` for initial data (admin user, etc.).

## Troubleshooting

- DB connection errors: confirm credentials and that the DB is reachable from your machine.
- JWT issues: ensure `JWT_SECRET` is set and consistent.
- Email not sending: verify SMTP credentials and port, check firewall.

## Next steps / suggestions

- Add a `README`-scripted `npm` scripts in `package.json` for common tasks (migrate, seed, start).
- Provide Postman collection or OpenAPI spec for the routes.
- Add a small test suite (Jest + supertest) for core endpoints (auth, register).

## Where to look in the codebase

- App entry: `src/server.js`
- Routes: `src/Routes/apiRoutes.js`
- Controllers: `src/controllers/`
- Models: `src/models/`
- Migrations: `migrations/`
- Seeders: `seeders/`
- Email templates: `src/services/Mail/`

