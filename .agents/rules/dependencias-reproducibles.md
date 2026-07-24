# Dependencias reproducibles

## Objetivo
Asegurar builds y entornos consistentes en el tiempo.

## Alcance
Aplica a dependencias de backend y proceso de instalación del frontend en Docker.

## Regla
Las dependencias de [backend/requirements.txt](backend/requirements.txt) deben estar versionadas explícitamente y la imagen de frontend debe instalar con npm ci en lugar de npm install.

## Razón
Dependencias no fijadas o instalaciones no deterministas producen resultados distintos entre máquinas y pipelines.

## Ejemplos
- Válido: versiones fijadas en requirements y uso de npm ci en Docker.
- No válido: requerimientos sin versión y uso de npm install en builds reproducibles.

## Cómo validarla
- Revisar [backend/requirements.txt](backend/requirements.txt#L1).
- Revisar comando de instalación en [frontend/Dockerfile](frontend/Dockerfile#L6).
- Ejecutar build en limpio dos veces y verificar resultados consistentes.