# Current State

## Features implementadas
- Dashboard frontend con carga de datos desde API, estado de loading y estado de error en [frontend/src/App.tsx](frontend/src/App.tsx).
- Visualización de KPIs (ingresos, egresos, utilidad, margen) mediante [frontend/src/components/dashboard/kpi-row.tsx](frontend/src/components/dashboard/kpi-row.tsx) y utilidades de [frontend/src/lib/financial-utils.ts](frontend/src/lib/financial-utils.ts).
- Gráficos de ingresos vs egresos y margen de utilidad con Recharts en [frontend/src/components/dashboard/income-outcome-chart.tsx](frontend/src/components/dashboard/income-outcome-chart.tsx) y [frontend/src/components/dashboard/profit-percent-chart.tsx](frontend/src/components/dashboard/profit-percent-chart.tsx).
- API backend con endpoint health y endpoints de métricas, facets, summary, top categories, comparison, alerts, b2b y b2c en [backend/app/routes.py](backend/app/routes.py).
- Pruebas backend de endpoints principales y filtros en [backend/tests/test_routes.py](backend/tests/test_routes.py).
- Pruebas frontend de utilidades financieras en [frontend/src/lib/financial-utils.test.ts](frontend/src/lib/financial-utils.test.ts).

## Features parcialmente implementadas
- Capacidades analíticas avanzadas están implementadas en backend (summary, top categories, comparison, alerts) pero el frontend actual consume solo /api/metrics en [frontend/src/App.tsx](frontend/src/App.tsx).
- Segmentación por business_type tiene endpoints dedicados b2b y b2c en backend, pero no hay controles de filtro B2B/B2C en la UI actual de [frontend/src/App.tsx](frontend/src/App.tsx).

## Gaps encontrados
- No hay base de datos persistente en la arquitectura actual; los datos se generan como mock en [backend/app/routes.py](backend/app/routes.py), y [docker-compose.yml](docker-compose.yml) no define servicio de base de datos.
- Existe dataset local [frontend/src/lib/mock-data.ts](frontend/src/lib/mock-data.ts) sin evidencia de uso en el flujo principal de [frontend/src/App.tsx](frontend/src/App.tsx).
- README principal documenta ejecución local, pero no presenta inventario explícito de endpoints implementados en [backend/app/routes.py](backend/app/routes.py).

## Deuda técnica
- Configuración CORS permisiva en [backend/app/main.py](backend/app/main.py) con wildcard.
- Arranque backend de desarrollo (debugpy y reload) como comando por defecto en [backend/Dockerfile](backend/Dockerfile).
- Concentración de lógica de negocio y endpoints en un único archivo [backend/app/routes.py](backend/app/routes.py).
- Dependencias backend sin versión fija en [backend/requirements.txt](backend/requirements.txt).
- Predominio de pruebas de caso feliz en API según [backend/tests/test_routes.py](backend/tests/test_routes.py), con ausencia visible de casos negativos sistemáticos.

## Prioridades sugeridas
1. Reducir riesgo operativo y de seguridad: ajustar CORS en [backend/app/main.py](backend/app/main.py) y separar perfil de ejecución estándar vs desarrollo en [backend/Dockerfile](backend/Dockerfile) y [docker-compose.yml](docker-compose.yml).
2. Mejorar mantenibilidad backend: separar lógica de dominio desde [backend/app/routes.py](backend/app/routes.py) y evitar duplicación de rutas segmentadas.
3. Aumentar robustez de calidad: ampliar pruebas negativas en [backend/tests/test_routes.py](backend/tests/test_routes.py).
4. Cerrar brecha producto API/UI: decidir si el frontend incorporará endpoints analíticos existentes en [backend/app/routes.py](backend/app/routes.py) y documentar el alcance en [README.md](README.md).
5. Mejorar reproducibilidad: fijar versiones en [backend/requirements.txt](backend/requirements.txt) y revisar estrategia de instalación en [frontend/Dockerfile](frontend/Dockerfile).