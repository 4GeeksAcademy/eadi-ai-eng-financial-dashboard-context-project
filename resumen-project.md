# Resumen del Proyecto (Fase 1 Validada)

## 1) Estructura general del repositorio

El proyecto está organizado como un monorepo con dos servicios principales:

- frontend (React + TypeScript + Vite)
- backend (FastAPI)

En la raíz se centraliza la orquestación con Docker Compose y la documentación base en inglés y español.

## 2) Servicios identificados

### Frontend

- Construcción definida en frontend/Dockerfile.
- Expone el puerto 5173.
- Consume datos financieros desde la ruta /api/metrics.

### Backend

- Construcción definida en backend/Dockerfile.
- Expone los puertos 8000 (API) y 5678 (debug).
- Implementa endpoints de salud y métricas financieras.

## 3) Módulos y componentes principales

### Backend

- backend/app/main.py: inicializa FastAPI, configura CORS e incluye el router principal.
- backend/app/routes.py: define modelos de datos, lógica de generación/filtrado/agregación de movimientos y endpoints de métricas.
- backend/tests/test_routes.py: pruebas de endpoints y comportamiento esperado.
- backend/tests/conftest.py: configuración de path para importaciones en tests.

### Frontend

- frontend/src/main.tsx: punto de entrada de React.
- frontend/src/App.tsx: flujo principal de carga de datos, cálculo de KPIs y render del dashboard.
- frontend/src/components/dashboard/: componentes principales de presentación del dashboard (header, KPIs, gráficos).
- frontend/src/components/ui/: componentes de UI reutilizables (card, skeleton).
- frontend/src/lib/: tipos de dominio y utilidades financieras.

## 4) Entry points

- Frontend:
  - frontend/index.html (contiene el contenedor root y carga /src/main.tsx)
  - frontend/src/main.tsx (monta App en React DOM)

- Backend:
  - backend/app/main.py (declara la instancia app de FastAPI)

- Arranque por contenedores:
  - docker-compose.yml (servicios frontend y backend)
  - backend/Dockerfile (comando uvicorn con debugpy)
  - frontend/Dockerfile (npm run dev)

## 5) Flujo principal de ejecución

1. docker-compose.yml levanta los servicios frontend y backend.
2. El backend inicia FastAPI en backend/app/main.py e incorpora las rutas de backend/app/routes.py.
3. El frontend monta App desde frontend/src/main.tsx.
4. App ejecuta fetch a /api/metrics (frontend/src/App.tsx).
5. Vite redirige /api al backend mediante proxy (frontend/vite.config.ts).
6. Con la respuesta del backend, frontend calcula KPIs y series mensuales con utilidades de frontend/src/lib/financial-utils.ts y renderiza tarjetas y gráficos.

## 6) Validación y correcciones aplicadas

La versión anterior del resumen fue contrastada con código real en backend, frontend y configuración de entorno.  
No se mantienen afirmaciones sin evidencia en archivos del repositorio.

También se detectó una limitación de lectura en el entorno del agente para frontend/.env.example, por lo que no se usó como soporte de afirmaciones críticas del resumen.

## Evidencia utilizada

Archivos principales usados para respaldar este resumen:

- Estructura y ejecución:
  - README.md
  - README.es.md
  - docker-compose.yml
  - backend/Dockerfile
  - frontend/Dockerfile

- Backend:
  - backend/app/main.py
  - backend/app/routes.py
  - backend/tests/test_routes.py
  - backend/tests/conftest.py
  - backend/requirements.txt

- Frontend:
  - frontend/index.html
  - frontend/src/main.tsx
  - frontend/src/App.tsx
  - frontend/vite.config.ts
  - frontend/package.json
  - frontend/src/components/dashboard/dashboard-header.tsx
  - frontend/src/components/dashboard/kpi-row.tsx
  - frontend/src/components/dashboard/kpi-card.tsx
  - frontend/src/components/dashboard/income-outcome-chart.tsx
  - frontend/src/components/dashboard/profit-percent-chart.tsx
  - frontend/src/components/ui/card.tsx
  - frontend/src/components/ui/skeleton.tsx
  - frontend/src/lib/financial-types.ts
  - frontend/src/lib/financial-utils.ts
  - frontend/src/lib/financial-utils.test.ts
  - frontend/src/index.css
