# CORS por entorno

## Objetivo
Restringir el acceso cross-origin de la API según el entorno de ejecución.

## Problema que resuelve
En la Fase 2 se identificó una configuración CORS demasiado permisiva con wildcard, lo que amplía la superficie de exposición.

## Regla
En [backend/app/main.py](backend/app/main.py), la configuración de CORS debe usar una lista explícita de orígenes permitidos por entorno y no permitir wildcard combinado con credenciales.

## Justificación
Este repositorio expone endpoints de métricas y su consumo debe quedar acotado a orígenes controlados, especialmente fuera de desarrollo local.

## Evidencia
- [backend/app/main.py](backend/app/main.py#L7)
- [backend/app/main.py](backend/app/main.py#L9)
- [backend/app/main.py](backend/app/main.py#L10)
- [backend/app/main.py](backend/app/main.py#L11)

# Ejecución segura del contenedor backend

## Objetivo
Separar claramente el modo desarrollo del modo estándar para evitar exposición de capacidades de depuración.

## Problema que resuelve
Se detectó que el backend arranca con debugpy y reload por defecto, además de exponer puerto de depuración.

## Regla
En [backend/Dockerfile](backend/Dockerfile) y [docker-compose.yml](docker-compose.yml), debugpy, reload y el puerto 5678 deben habilitarse solo en un perfil de desarrollo, nunca en ejecución estándar.

## Justificación
El arranque seguro reduce riesgo operativo y evita dependencias de herramientas de debug en entornos que no lo requieren.

## Evidencia
- [backend/Dockerfile](backend/Dockerfile#L10)
- [backend/Dockerfile](backend/Dockerfile#L12)
- [docker-compose.yml](docker-compose.yml#L20)

# Modularización del dominio backend

## Objetivo
Reducir acoplamiento entre capa HTTP, modelos y lógica de negocio.

## Problema que resuelve
Se observó concentración de responsabilidades en un único archivo de rutas.

## Regla
La lógica de generación, filtrado y agregación de métricas debe residir en módulos de servicio separados; [backend/app/routes.py](backend/app/routes.py) debe enfocarse en request/response y composición.

## Justificación
Separar responsabilidades mejora mantenibilidad, revisabilidad de cambios y capacidad de evolución del dominio financiero.

## Evidencia
- [backend/app/routes.py](backend/app/routes.py#L1)
- [backend/app/routes.py](backend/app/routes.py#L94)
- [backend/app/routes.py](backend/app/routes.py#L248)
- [backend/app/routes.py](backend/app/routes.py#L342)

# No duplicar endpoints por segmento de negocio

## Objetivo
Evitar duplicación de rutas con lógica equivalente.

## Problema que resuelve
Se identificaron endpoints paralelos para B2B y B2C con comportamiento casi idéntico.

## Regla
Las rutas de métricas segmentadas por tipo de negocio deben consolidarse en una ruta con parámetro business_type, evitando mantener rutas duplicadas para cada segmento.

## Justificación
Disminuye divergencias funcionales y simplifica mantenimiento cuando cambian filtros o reglas de cálculo.

## Evidencia
- [backend/app/routes.py](backend/app/routes.py#L362)
- [backend/app/routes.py](backend/app/routes.py#L378)
- [backend/app/routes.py](backend/app/routes.py#L277)
- [backend/app/routes.py](backend/app/routes.py#L350)

# Dependencias reproducibles

## Objetivo
Asegurar builds y entornos consistentes en el tiempo.

## Problema que resuelve
Se detectaron dependencias Python sin fijación de versión y uso de instalación no determinista en frontend.

## Regla
Las dependencias en [backend/requirements.txt](backend/requirements.txt) deben estar versionadas explícitamente y la imagen de frontend debe instalar con npm ci en lugar de npm install.

## Justificación
La reproducibilidad reduce errores intermitentes y facilita depuración entre máquinas y pipelines.

## Evidencia
- [backend/requirements.txt](backend/requirements.txt#L1)
- [backend/requirements.txt](backend/requirements.txt#L2)
- [frontend/Dockerfile](frontend/Dockerfile#L6)

# Cobertura de casos negativos en API

## Objetivo
Validar comportamiento de error y bordes de parámetros en endpoints de backend.

## Problema que resuelve
Predominan pruebas de casos felices con respuestas 200 y faltan validaciones negativas sistemáticas.

## Regla
Por cada endpoint probado en [backend/tests/test_routes.py](backend/tests/test_routes.py), se debe incluir al menos un caso inválido esperado (por ejemplo 422 o error de contrato) además de los casos exitosos.

## Justificación
Los casos negativos detectan regresiones de validación y mejoran la confiabilidad del contrato API.

## Evidencia
- [backend/tests/test_routes.py](backend/tests/test_routes.py#L29)
- [backend/tests/test_routes.py](backend/tests/test_routes.py#L46)
- [backend/tests/test_routes.py](backend/tests/test_routes.py#L107)
- [backend/tests/test_routes.py](backend/tests/test_routes.py#L163)

# Documentación mínima de endpoints

## Objetivo
Alinear documentación operativa con la implementación real de la API.

## Problema que resuelve
El README explica arranque, pero no inventaria de forma explícita los endpoints implementados.

## Regla
[README.md](README.md) y [README.es.md](README.es.md) deben incluir una sección breve de endpoints disponibles con ruta y propósito de una línea, alineada con [backend/app/routes.py](backend/app/routes.py).

## Justificación
Reduce tiempo de onboarding y evita depender exclusivamente de inspección de código o Swagger para entender alcance.

## Evidencia
- [README.md](README.md#L39)
- [README.md](README.md#L50)
- [backend/app/routes.py](backend/app/routes.py#L262)
- [backend/app/routes.py](backend/app/routes.py#L305)

# Fuente de datos única o explícita en frontend

## Objetivo
Eliminar ambigüedad sobre la fuente real de datos en el dashboard.

## Problema que resuelve
Existe un dataset local grande que no participa del flujo principal de la aplicación.

## Regla
Si [frontend/src/lib/mock-data.ts](frontend/src/lib/mock-data.ts) es solo fixture de pruebas, debe ubicarse en contexto de test y documentarse; si es fallback de desarrollo, debe integrarse explícitamente en [frontend/src/App.tsx](frontend/src/App.tsx).

## Justificación
Evita desalineación entre datos de demo y contrato real de backend.

## Evidencia
- [frontend/src/lib/mock-data.ts](frontend/src/lib/mock-data.ts#L1)
- [frontend/src/App.tsx](frontend/src/App.tsx#L15)

# Preservación de tipado de dominio entre backend y frontend

## Objetivo
Mantener consistencia del contrato de datos en todo el sistema.

## Problema que resuelve
El dominio está tipado en ambos lados, pero sin una regla explícita existe riesgo de deriva entre modelos.

## Regla
Cualquier cambio de campos en modelos backend debe incluir, en el mismo cambio, la actualización correspondiente de tipos frontend y su validación en tests afectados.

## Justificación
Evita incompatibilidades en runtime y fallos de serialización/consumo en el dashboard.

## Evidencia
- [backend/app/routes.py](backend/app/routes.py#L22)
- [frontend/src/lib/financial-types.ts](frontend/src/lib/financial-types.ts#L5)
- [frontend/src/App.tsx](frontend/src/App.tsx#L7)

# Preservación de validaciones de calidad local

## Objetivo
Mantener una base mínima de calidad antes de integrar cambios.

## Problema que resuelve
Sin regla de ejecución previa, lint/tests/build pueden omitirse y aumentar regresiones.

## Regla
Antes de merge, deben ejecutarse los scripts de calidad definidos en [frontend/package.json](frontend/package.json): lint, test y build.

## Justificación
El repositorio ya dispone de herramientas de verificación; formalizar su uso convierte capacidad técnica en gobernanza efectiva.

## Evidencia
- [frontend/package.json](frontend/package.json#L6)
- [frontend/package.json](frontend/package.json#L9)
- [frontend/package.json](frontend/package.json#L11)
- [frontend/package.json](frontend/package.json#L8)
- [frontend/eslint.config.js](frontend/eslint.config.js#L8)
- [frontend/tsconfig.app.json](frontend/tsconfig.app.json#L22)
