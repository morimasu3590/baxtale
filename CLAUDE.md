# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Baxtale is an HR / payroll application. It is a monorepo with two independent projects (both currently close to their scaffold templates):

- `baxtale.webApi/` — ASP.NET Core Web API (.NET 10, minimal APIs, nullable + implicit usings enabled). No solution file; commands target the `.csproj` directory.
- `FE/baxtale-fe/` — React 19 + TypeScript + Vite SPA. Node version pinned in `FE/.tool-versions` (asdf/mise).

There are no test projects or test runners configured yet in either project.

## Commands

Backend (run from `baxtale.webApi/`):

```sh
dotnet build
dotnet run                      # "https" profile first: https://localhost:7166 + http://localhost:5097
dotnet run --launch-profile http  # http only on :5097
```

- OpenAPI document is served only in Development at `/openapi/v1.json` (`AddOpenApi`/`MapOpenApi`; no Swagger UI).
- `baxtale.webApi.http` holds sample requests for the REST Client / IDE HTTP runner.

Frontend (run from `FE/baxtale-fe/`):

```sh
npm install
npm run dev       # Vite dev server
npm run build     # tsc -b (type check) then vite build
npm run lint      # eslint flat config: typescript-eslint, react-hooks, react-refresh
npm run preview
```

## Architecture notes

- API: everything is in `Program.cs` (service registration, middleware pipeline, endpoint mapping via `app.MapGet`...). Only the template `/weatherforecast` endpoint exists so far.
- Frontend → API wiring is not set up yet: `vite.config.ts` has no dev proxy, and the API has no CORS policy. You need one or the other before the SPA can call the API in dev.
- TS config is strict about unused locals/params and uses `verbatimModuleSyntax` + `erasableSyntaxOnly`. That means type-only imports need `import type`, and TS `enum`s / parameter properties are not allowed.

## Git

Work happens on feature branch that take from `develop`; PRs target `develop`.
