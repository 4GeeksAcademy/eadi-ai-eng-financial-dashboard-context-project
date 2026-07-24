# Tech Stack

## Frontend
- React: ^19.2.4 en [frontend/package.json](frontend/package.json)
- React DOM: ^19.2.4 en [frontend/package.json](frontend/package.json)
- TypeScript: ~6.0.2 en [frontend/package.json](frontend/package.json)
- Vite: ^8.0.4 en [frontend/package.json](frontend/package.json)
- Recharts: ^3.8.1 en [frontend/package.json](frontend/package.json)
- Tailwind CSS: ^4.2.2 en [frontend/package.json](frontend/package.json)

## Backend
- FastAPI en [backend/requirements.txt](backend/requirements.txt)
- Uvicorn con extras standard en [backend/requirements.txt](backend/requirements.txt)
- Pydantic BaseModel usado en [backend/app/routes.py](backend/app/routes.py)
- Debugpy en [backend/requirements.txt](backend/requirements.txt)

## Base de datos
- No hay base de datos configurada en el repositorio.
- Evidencia:
  - [docker-compose.yml](docker-compose.yml) define solo frontend y backend.
  - [backend/app/routes.py](backend/app/routes.py) genera datos mock en memoria con generate_mock_movements.
  - [backend/requirements.txt](backend/requirements.txt) no incluye cliente/ORM de base de datos.

## Infraestructura
- Docker Compose para orquestación local en [docker-compose.yml](docker-compose.yml)
- Servicio frontend con bind mount y puerto 5173 en [docker-compose.yml](docker-compose.yml)
- Servicio backend con bind mount y puertos 8000/5678 en [docker-compose.yml](docker-compose.yml)

## Docker
- Imagen backend: python:3.13-slim en [backend/Dockerfile](backend/Dockerfile)
- Imagen frontend: node:24-alpine en [frontend/Dockerfile](frontend/Dockerfile)
- Backend ejecuta uvicorn vía debugpy en [backend/Dockerfile](backend/Dockerfile)
- Frontend ejecuta vite dev en [frontend/Dockerfile](frontend/Dockerfile)

## Dependencias importantes
- Frontend:
  - UI y utilidades: class-variance-authority, clsx, tailwind-merge en [frontend/package.json](frontend/package.json)
  - Iconografía: lucide-react en [frontend/package.json](frontend/package.json)
  - Testing: vitest y @vitest/coverage-v8 en [frontend/package.json](frontend/package.json)
- Backend:
  - API y servidor: fastapi, uvicorn[standard] en [backend/requirements.txt](backend/requirements.txt)
  - Testing: pytest, pytest-cov, httpx en [backend/requirements.txt](backend/requirements.txt)

## Frameworks
- Frontend: React + Vite + Tailwind CSS, definidos en [frontend/package.json](frontend/package.json) y [frontend/vite.config.ts](frontend/vite.config.ts)
- Backend: FastAPI, definido en [backend/requirements.txt](backend/requirements.txt) y [backend/app/main.py](backend/app/main.py)

## Herramientas
- Lint frontend con ESLint en [frontend/eslint.config.js](frontend/eslint.config.js)
- Testing frontend con Vitest en scripts de [frontend/package.json](frontend/package.json)
- Testing backend con pytest en [backend/requirements.txt](backend/requirements.txt) y pruebas en [backend/tests/test_routes.py](backend/tests/test_routes.py)

## Estado de versiones
- Con versión explícita:
  - Frontend dependencies/devDependencies en [frontend/package.json](frontend/package.json)
  - Base images Docker en [backend/Dockerfile](backend/Dockerfile) y [frontend/Dockerfile](frontend/Dockerfile)
- Sin versión explícita:
  - Dependencias backend en [backend/requirements.txt](backend/requirements.txt) están sin pin de versión.