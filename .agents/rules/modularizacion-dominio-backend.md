# Modularización del dominio backend

## Objetivo
Reducir acoplamiento entre capa HTTP, modelos y lógica de negocio.

## Alcance
Aplica al diseño de [backend/app/routes.py](backend/app/routes.py) y a nuevos módulos del backend.

## Regla
La lógica de generación, filtrado y agregación de métricas debe residir en módulos de servicio separados; el archivo de rutas debe enfocarse en request/response y composición.

## Razón
Hoy se concentran modelos, lógica y endpoints en un solo archivo, lo que dificulta mantenimiento y evolución.

## Ejemplos
- Válido: funciones de negocio en módulo de servicio y rutas llamando a esas funciones.
- No válido: agregar nuevas reglas de cálculo directamente dentro del archivo de rutas.

## Cómo validarla
- Revisar que endpoints en [backend/app/routes.py](backend/app/routes.py#L248) deleguen en servicios.
- Verificar que funciones de agregación no crezcan dentro del archivo de rutas.
- En PRs, comprobar separación entre cambios HTTP y cambios de lógica de negocio.