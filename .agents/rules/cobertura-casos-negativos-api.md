# Cobertura de casos negativos en API

## Objetivo
Validar comportamiento de error y bordes de parámetros en endpoints de backend.

## Alcance
Aplica a la suite de pruebas de API en [backend/tests/test_routes.py](backend/tests/test_routes.py).

## Regla
Por cada endpoint probado debe existir al menos un caso inválido esperado, además de casos exitosos.

## Razón
Las pruebas de solo casos felices no detectan regresiones en validación de entrada ni contratos de error.

## Ejemplos
- Válido: probar query params fuera de rango y esperar error de validación.
- No válido: cubrir únicamente status 200 y payload válido.

## Cómo validarla
- Revisar que cada bloque de endpoint tenga caso feliz y caso inválido en [backend/tests/test_routes.py](backend/tests/test_routes.py).
- Ejecutar tests y confirmar que los casos inválidos están asertando códigos y estructura de error esperada.