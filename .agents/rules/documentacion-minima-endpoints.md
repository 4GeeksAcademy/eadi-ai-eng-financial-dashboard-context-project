# Documentación mínima de endpoints

## Objetivo
Alinear documentación operativa con la implementación real de la API.

## Alcance
Aplica a documentación principal en [README.md](README.md) y [README.es.md](README.es.md).

## Regla
Los README deben incluir una sección breve de endpoints disponibles con ruta y propósito de una línea, alineada con [backend/app/routes.py](backend/app/routes.py).

## Razón
Sin inventario mínimo de endpoints, el onboarding depende de inspección manual de código o Swagger.

## Ejemplos
- Válido: tabla corta con endpoint, filtros principales y descripción.
- No válido: documentar solo cómo levantar servicios sin describir API implementada.

## Cómo validarla
- Revisar que el README enumere endpoints reales definidos en [backend/app/routes.py](backend/app/routes.py#L248).
- Verificar que los nombres de ruta documentados coincidan con el código.