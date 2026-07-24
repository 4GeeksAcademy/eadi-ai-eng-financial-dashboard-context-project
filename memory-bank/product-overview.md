# Product Overview

## Propósito del producto
- Proveer un dashboard de métricas financieras con frontend en React + TypeScript y backend en FastAPI, según la definición del proyecto en [README.md](README.md).
- Exponer datos de movimientos financieros y agregaciones para visualización en UI, mediante endpoints definidos en [backend/app/routes.py](backend/app/routes.py).

## Objetivos
- Mostrar indicadores clave de desempeño financiero (ingresos, egresos, utilidad y margen) en el frontend, implementado en [frontend/src/App.tsx](frontend/src/App.tsx) y componentes de [frontend/src/components/dashboard](frontend/src/components/dashboard).
- Entregar una API de métricas con filtros y vistas analíticas (facets, summary, top categories, comparison, alerts), implementada en [backend/app/routes.py](backend/app/routes.py).
- Permitir ejecución local de frontend y backend en contenedores, definida en [docker-compose.yml](docker-compose.yml), [frontend/Dockerfile](frontend/Dockerfile) y [backend/Dockerfile](backend/Dockerfile).

## Módulos principales
- Frontend dashboard:
  - Orquestación de carga y render: [frontend/src/App.tsx](frontend/src/App.tsx)
  - Componentes KPI y charts: [frontend/src/components/dashboard/kpi-row.tsx](frontend/src/components/dashboard/kpi-row.tsx), [frontend/src/components/dashboard/income-outcome-chart.tsx](frontend/src/components/dashboard/income-outcome-chart.tsx), [frontend/src/components/dashboard/profit-percent-chart.tsx](frontend/src/components/dashboard/profit-percent-chart.tsx)
  - Tipos y utilidades financieras: [frontend/src/lib/financial-types.ts](frontend/src/lib/financial-types.ts), [frontend/src/lib/financial-utils.ts](frontend/src/lib/financial-utils.ts)
- Backend API:
  - Bootstrap de app FastAPI: [backend/app/main.py](backend/app/main.py)
  - Modelos, lógica de métricas y endpoints: [backend/app/routes.py](backend/app/routes.py)
  - Pruebas de endpoints: [backend/tests/test_routes.py](backend/tests/test_routes.py)
- Infraestructura local:
  - Orquestación de servicios: [docker-compose.yml](docker-compose.yml)

## Flujo general
1. Se levantan los servicios frontend y backend con [docker-compose.yml](docker-compose.yml).
2. El backend inicializa FastAPI e incluye el router en [backend/app/main.py](backend/app/main.py).
3. El frontend monta la app desde [frontend/src/main.tsx](frontend/src/main.tsx) y renderiza [frontend/src/App.tsx](frontend/src/App.tsx).
4. La app solicita datos a /api/metrics en [frontend/src/App.tsx](frontend/src/App.tsx), y el proxy de Vite redirige al backend según [frontend/vite.config.ts](frontend/vite.config.ts).
5. El backend responde desde [backend/app/routes.py](backend/app/routes.py) y el frontend calcula KPIs/series mensuales con [frontend/src/lib/financial-utils.ts](frontend/src/lib/financial-utils.ts).