# Análisis de prácticas del repositorio

## Objetivo
Documentar, de forma estructurada, las buenas prácticas y los riesgos identificados previamente en la Fase 2 para que sirvan como base de gobernanza técnica del repositorio.

## Buenas prácticas

### Arquitectura

#### 1) Separación clara de servicios frontend/backend
- Nombre: Separación de servicios por responsabilidad
- Categoría: Arquitectura
- Descripción: El repositorio está organizado como dos servicios principales (frontend y backend) con orquestación centralizada.
- Evidencia:
  - [docker-compose.yml](docker-compose.yml#L1)
  - [docker-compose.yml](docker-compose.yml#L14)
  - [README.md](README.md#L18)
- Beneficio para el proyecto: Facilita onboarding, despliegue local y límites claros entre capas.

### Organización

#### 2) Estructura modular en frontend
- Nombre: Separación entre componentes y utilidades
- Categoría: Organización
- Descripción: El frontend separa componentes de dashboard, componentes UI y utilidades de dominio.
- Evidencia:
  - [frontend/src/App.tsx](frontend/src/App.tsx#L2)
  - [frontend/src/components/dashboard/kpi-row.tsx](frontend/src/components/dashboard/kpi-row.tsx#L1)
  - [frontend/src/lib/financial-utils.ts](frontend/src/lib/financial-utils.ts#L21)
- Beneficio para el proyecto: Mejora mantenibilidad y reduce acoplamiento entre presentación y lógica.

### Testing

#### 3) Pruebas en backend y frontend
- Nombre: Cobertura de API y utilidades financieras
- Categoría: Testing
- Descripción: Existen pruebas para endpoints backend y pruebas unitarias para utilidades de cálculo/formato en frontend.
- Evidencia:
  - [backend/tests/test_routes.py](backend/tests/test_routes.py#L29)
  - [backend/tests/test_routes.py](backend/tests/test_routes.py#L104)
  - [frontend/src/lib/financial-utils.test.ts](frontend/src/lib/financial-utils.test.ts#L35)
- Beneficio para el proyecto: Reduce regresiones en contrato API y cálculos de negocio.

### Configuración

#### 4) Tooling de calidad en frontend
- Nombre: Scripts y configuración de lint/test/build
- Categoría: Configuración
- Descripción: El proyecto define scripts de validación y reglas de TypeScript/ESLint para control de calidad local.
- Evidencia:
  - [frontend/package.json](frontend/package.json#L6)
  - [frontend/package.json](frontend/package.json#L11)
  - [frontend/eslint.config.js](frontend/eslint.config.js#L8)
  - [frontend/tsconfig.app.json](frontend/tsconfig.app.json#L22)
- Beneficio para el proyecto: Detecta problemas tempranamente y mejora consistencia del código.

### Developer Experience

#### 5) Proxy de Vite para API
- Nombre: Resolución de backend transparente en desarrollo
- Categoría: Developer Experience
- Descripción: El frontend usa proxy para enrutar /api hacia backend sin configuración adicional en entorno local.
- Evidencia:
  - [frontend/vite.config.ts](frontend/vite.config.ts#L11)
  - [frontend/vite.config.ts](frontend/vite.config.ts#L13)
  - [README.md](README.md#L45)
- Beneficio para el proyecto: Reduce fricción de arranque y errores de integración en desarrollo.

### Mantenibilidad

#### 6) Tipado explícito de dominio en ambas capas
- Nombre: Contrato tipado backend/frontend
- Categoría: Mantenibilidad
- Descripción: El dominio financiero está modelado con tipos explícitos en backend y frontend.
- Evidencia:
  - [backend/app/routes.py](backend/app/routes.py#L22)
  - [backend/app/routes.py](backend/app/routes.py#L30)
  - [backend/app/routes.py](backend/app/routes.py#L248)
  - [frontend/src/lib/financial-types.ts](frontend/src/lib/financial-types.ts#L5)
- Beneficio para el proyecto: Reduce ambigüedad del payload y errores de integración.

## Malas prácticas o riesgos

### Seguridad

#### 1) CORS demasiado permisivo
- Nombre: Configuración CORS amplia
- Categoría: Seguridad
- Descripción: CORS permite todos los orígenes con credenciales habilitadas.
- Evidencia:
  - [backend/app/main.py](backend/app/main.py#L9)
  - [backend/app/main.py](backend/app/main.py#L10)
  - [backend/app/main.py](backend/app/main.py#L11)
- Impacto: Amplía superficie de exposición para consumo no controlado.
- Recomendación: Restringir allow_origins por entorno y evitar wildcard con credenciales.

### Docker / Seguridad

#### 2) Debug y reload en arranque por defecto del backend
- Nombre: Runtime de desarrollo expuesto por defecto
- Categoría: Docker
- Descripción: El backend inicia con debugpy, reload y puerto de depuración expuesto.
- Evidencia:
  - [backend/Dockerfile](backend/Dockerfile#L10)
  - [backend/Dockerfile](backend/Dockerfile#L12)
  - [docker-compose.yml](docker-compose.yml#L20)
- Impacto: Riesgo operativo y de seguridad fuera de contexto de desarrollo.
- Recomendación: Separar perfiles dev/estándar y no exponer depuración en ejecución estándar.

### Arquitectura

#### 3) Concentración de responsabilidades en routes.py
- Nombre: Acoplamiento de capas en un único archivo
- Categoría: Arquitectura
- Descripción: Modelos, lógica de dominio y endpoints conviven en el mismo módulo.
- Evidencia:
  - [backend/app/routes.py](backend/app/routes.py#L1)
  - [backend/app/routes.py](backend/app/routes.py#L94)
  - [backend/app/routes.py](backend/app/routes.py#L248)
  - [backend/app/routes.py](backend/app/routes.py#L342)
- Impacto: Dificulta escalabilidad y mantenimiento evolutivo.
- Recomendación: Separar esquemas, servicios de dominio y capa HTTP en módulos distintos.

### Mantenibilidad

#### 4) Duplicación de lógica por business_type
- Nombre: Endpoints duplicados para B2B/B2C
- Categoría: Mantenibilidad
- Descripción: Existen rutas separadas con lógica prácticamente equivalente.
- Evidencia:
  - [backend/app/routes.py](backend/app/routes.py#L277)
  - [backend/app/routes.py](backend/app/routes.py#L295)
  - [backend/app/routes.py](backend/app/routes.py#L311)
  - [backend/app/routes.py](backend/app/routes.py#L350)
  - [backend/app/routes.py](backend/app/routes.py#L362)
  - [backend/app/routes.py](backend/app/routes.py#L378)
- Impacto: Aumenta costo de cambio y riesgo de inconsistencias.
- Recomendación: Consolidar en rutas parametrizadas por business_type.

### Configuración

#### 5) Reproducibilidad incompleta de dependencias
- Nombre: Dependencias no completamente deterministas
- Categoría: Configuración
- Descripción: Backend sin pines explícitos y frontend con estrategia de instalación mejorable para builds reproducibles.
- Evidencia:
  - [backend/requirements.txt](backend/requirements.txt#L1)
  - [backend/requirements.txt](backend/requirements.txt#L2)
  - [frontend/Dockerfile](frontend/Dockerfile#L6)
- Impacto: Variaciones de entorno y fallos difíciles de reproducir.
- Recomendación: Fijar versiones backend y usar instalación determinista en imagen frontend.

### Testing

#### 6) Predominio de casos felices en pruebas API
- Nombre: Falta de cobertura negativa sistemática
- Categoría: Testing
- Descripción: Predominan validaciones de respuestas exitosas sin suficiente cobertura de entradas inválidas.
- Evidencia:
  - [backend/tests/test_routes.py](backend/tests/test_routes.py#L32)
  - [backend/tests/test_routes.py](backend/tests/test_routes.py#L46)
  - [backend/tests/test_routes.py](backend/tests/test_routes.py#L107)
  - [backend/tests/test_routes.py](backend/tests/test_routes.py#L163)
- Impacto: Menor confianza ante cambios en validaciones y contratos de error.
- Recomendación: Agregar casos negativos y de borde por endpoint.

### Documentación

#### 7) Falta de inventario explícito de endpoints en README
- Nombre: Documentación API incompleta en onboarding
- Categoría: Documentación
- Descripción: README explica ejecución, pero no lista de forma mínima los endpoints implementados.
- Evidencia:
  - [README.md](README.md#L39)
  - [README.md](README.md#L50)
  - [backend/app/routes.py](backend/app/routes.py#L262)
  - [backend/app/routes.py](backend/app/routes.py#L305)
- Impacto: Aumenta tiempo de descubrimiento funcional para nuevos desarrolladores.
- Recomendación: Añadir sección breve de endpoints y propósito.

### Organización / Mantenibilidad

#### 8) Dataset local no integrado al flujo principal
- Nombre: Ambigüedad de fuente de datos en frontend
- Categoría: Organización
- Descripción: Existe archivo de mock data amplio sin uso en el flujo principal actual.
- Evidencia:
  - [frontend/src/lib/mock-data.ts](frontend/src/lib/mock-data.ts#L1)
  - [frontend/src/App.tsx](frontend/src/App.tsx#L15)
- Impacto: Posible confusión sobre fuente de verdad y riesgo de desalineación.
- Recomendación: Definir rol explícito (fixture de tests o fallback documentado) o retirarlo del flujo activo.

## Conclusión
El análisis de Fase 2 confirmó una base técnica sólida en separación de servicios, organización del frontend, tipado y cobertura funcional inicial de pruebas.  
También identificó riesgos concretos en seguridad (CORS y runtime de debug), mantenibilidad (duplicación y acoplamiento en backend), reproducibilidad de configuración, cobertura de pruebas negativas y claridad documental.

Estos hallazgos sirvieron directamente como fundamento para las reglas de gobernanza propuestas y documentadas posteriormente en [reglas.md](reglas.md) y en [/.agents/rules](.agents/rules), manteniendo trazabilidad con evidencia real del código.