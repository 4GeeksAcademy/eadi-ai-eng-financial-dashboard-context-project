# Preservación de validaciones de calidad local

## Objetivo
Mantener una base mínima de calidad antes de integrar cambios.

## Alcance
Aplica al flujo de trabajo de contribución sobre frontend y documentación técnica del repositorio.

## Regla
Antes de merge deben ejecutarse los scripts de lint, test y build definidos en [frontend/package.json](frontend/package.json).

## Razón
El proyecto ya incluye herramientas de validación y omitirlas incrementa riesgo de regresiones.

## Ejemplos
- Válido: ejecutar lint, test y build antes de abrir o actualizar PR.
- No válido: subir cambios sin pasar validaciones locales mínimas.

## Cómo validarla
- Revisar scripts en [frontend/package.json](frontend/package.json#L6).
- Confirmar en PR o checklist que se ejecutaron lint, test y build.
- Si hay fallos, corregirlos antes de merge.