# No duplicar endpoints por segmento de negocio

## Objetivo
Evitar duplicación de rutas con lógica equivalente.

## Alcance
Aplica a endpoints de métricas segmentadas por tipo de negocio en backend.

## Regla
Las rutas de métricas segmentadas por tipo de negocio deben consolidarse en una ruta con parámetro business_type, evitando mantener rutas duplicadas para cada segmento.

## Razón
La duplicación de endpoints similares incrementa costo de mantenimiento y riesgo de divergencia funcional.

## Ejemplos
- Válido: endpoint único con parámetro business_type opcional.
- No válido: mantener rutas separadas con la misma lógica de filtrado para cada segmento.

## Cómo validarla
- Revisar implementación de rutas en [backend/app/routes.py](backend/app/routes.py#L362) y [backend/app/routes.py](backend/app/routes.py#L378).
- Comprobar que nuevas capacidades de filtro se implementen una sola vez.
- Verificar en tests que un mismo endpoint cubre ambos segmentos vía parámetros.