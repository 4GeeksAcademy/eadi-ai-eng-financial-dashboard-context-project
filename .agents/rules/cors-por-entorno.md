# CORS por entorno

## Objetivo
Restringir el acceso cross-origin de la API según el entorno de ejecución.

## Alcance
Aplica a la configuración de middleware de CORS del backend FastAPI.

## Regla
En [backend/app/main.py](backend/app/main.py), la configuración de CORS debe usar una lista explícita de orígenes permitidos por entorno y no permitir wildcard combinado con credenciales.

## Razón
El backend expone endpoints de métricas y la configuración actual admite todos los orígenes, lo que amplía la superficie de exposición.

## Ejemplos
- Válido: allow_origins con una lista concreta de dominios esperados por entorno.
- No válido: allow_origins con * y allow_credentials habilitado.

## Cómo validarla
- Revisar [backend/app/main.py](backend/app/main.py#L7).
- Confirmar que allow_origins no usa wildcard en combinación con credenciales.
- Ejecutar backend y verificar desde un origen no permitido que el navegador bloquea la solicitud CORS.