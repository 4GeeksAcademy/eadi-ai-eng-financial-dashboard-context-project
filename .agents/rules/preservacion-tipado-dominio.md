# Preservación de tipado de dominio entre backend y frontend

## Objetivo
Mantener consistencia del contrato de datos en todo el sistema.

## Alcance
Aplica a modelos de backend y tipos de frontend para movimientos y métricas.

## Regla
Cualquier cambio de campos en modelos backend debe incluir en el mismo cambio la actualización correspondiente de tipos frontend y pruebas afectadas.

## Razón
El contrato está tipado en ambos lados y una deriva de campos rompe consumo del dashboard.

## Ejemplos
- Válido: añadir campo en backend y actualizar tipos y consumidores frontend en el mismo PR.
- No válido: cambiar payload backend sin sincronizar [frontend/src/lib/financial-types.ts](frontend/src/lib/financial-types.ts).

## Cómo validarla
- Revisar modelos en [backend/app/routes.py](backend/app/routes.py#L22).
- Revisar tipos en [frontend/src/lib/financial-types.ts](frontend/src/lib/financial-types.ts#L5).
- Verificar que tests relacionados pasen tras cambios de contrato.