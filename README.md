# The ai school agent

## What this is built with

The whole product: an API and the screens that use it.

- **FastAPI** for the API, served by **uvicorn**.
- **SQLAlchemy** for the models, **Alembic** for the migrations in `migrations/`, and **Pydantic** for request and response shapes.
- **JWT** for authentication, in `app/routers/auth.py`.
- The OpenAPI schema FastAPI writes itself, served at `/api/schema/` and browsable at `/api/docs/`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, database, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the secret key, the database URL, the allowed origins and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── app/
│   ├── routers/
│   │   └── auth.py      # register, log in, refresh
│   ├── config.py        # settings, read from the environment
│   ├── db.py            # the engine and the session
│   └── main.py          # where routers are mounted
├── migrations/          # Alembic, one file per change to the tables
├── tests/
├── alembic.ini
├── scripts.py           # export-openapi
└── requirements.txt

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a Pydantic schema or a route. |
| `blacksmith make:resource Post` | Scaffold a SQLAlchemy model, Pydantic schemas, a service, a router and its migration, plus the hooks and pages that use them. |
| `blacksmith backend <module>` | Run a Python module in the backend, e.g. `blacksmith backend alembic upgrade head`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain FastAPI and React project. Nothing here is a dependency on us. |
