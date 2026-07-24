# Fuente de datos única o explícita en frontend

## Objetivo
Eliminar ambigüedad sobre la fuente real de datos del dashboard.

## Alcance
Aplica a datos usados por la app en [frontend/src/App.tsx](frontend/src/App.tsx) y a datasets locales en [frontend/src/lib](frontend/src/lib).

## Regla
Si [frontend/src/lib/mock-data.ts](frontend/src/lib/mock-data.ts) es solo fixture de pruebas, debe ubicarse en contexto de test y documentarse; si es fallback de desarrollo, debe integrarse explícitamente en el flujo de App.

## Razón
Mantener datasets paralelos no declarados como fuente oficial genera desalineación con el contrato real del backend.

## Ejemplos
- Válido: mock-data usado explícitamente en tests o fallback documentado.
- No válido: archivo de datos grande sin uso en flujo principal ni documentación.

## Cómo validarla
- Revisar referencias a [frontend/src/lib/mock-data.ts](frontend/src/lib/mock-data.ts) en el código.
- Confirmar que la fuente de datos principal de [frontend/src/App.tsx](frontend/src/App.tsx#L15) esté documentada.