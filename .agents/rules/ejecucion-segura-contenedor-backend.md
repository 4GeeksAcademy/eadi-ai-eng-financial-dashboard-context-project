# Ejecución segura del contenedor backend

## Objetivo
Separar claramente el modo desarrollo del modo estándar para evitar exposición de capacidades de depuración.

## Alcance
Aplica a [backend/Dockerfile](backend/Dockerfile) y [docker-compose.yml](docker-compose.yml).

## Regla
Debugpy, reload y el puerto 5678 deben habilitarse solo en un perfil de desarrollo, nunca en ejecución estándar.

## Razón
El arranque con depuración activa y puerto de debug expuesto incrementa riesgo operativo fuera de entorno de desarrollo.

## Ejemplos
- Válido: perfil de desarrollo con debugpy y perfil estándar sin debugpy ni puerto 5678.
- No válido: un único comando por defecto con debugpy y reload para cualquier entorno.

## Cómo validarla
- Revisar [backend/Dockerfile](backend/Dockerfile#L12).
- Revisar exposición de puertos en [docker-compose.yml](docker-compose.yml#L18).
- Confirmar que la ejecución estándar no abre 5678 ni usa reload.