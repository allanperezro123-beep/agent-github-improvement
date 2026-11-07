# Guía de Batching de Commits y Tagging Semántico

## 1. El Problema del "Micro-Commit Spam" (AI Slop)

Uno de los síntomas más evidentes de un agente de IA operando sin disciplina es el historial de Git saturado de micro-commits triviales:
- `fix typo in test`
- `add import`
- `change variable name`
- `update readme`
- `fix syntax error`
- `try again`

Este patrón degrada la confianza del equipo humano, complica auditorías con `git bisect`, ensucia los changelogs y hace imposible un rollback limpio.

---

## 2. La Regla de Oro: Máximo 5 Commits por Lote

En cualquier sesión o bloque de trabajo, un agente **NO puede exceder 5 commits** para un proyecto. 

Si la tarea es grande:
1. Se planifican hasta **5 unidades de trabajo cohesivas**.
2. Si la tarea excede esa capacidad de empaquetado, significa que el alcance debe dividirse a nivel de PRs encadenados (Chained PRs) o entregas por fases, NO creando 20 commits desordenados.

### ¿Qué es una Unidad de Trabajo Cohesiva (Work Unit)?
Un commit debe representar un **cambio funcionalmente independiente y desplegable**:
- Incluye el código productivo + sus pruebas asociadas + su documentación relacionada.
- No separa la prueba del código: **Las pruebas pertenecen al mismo commit que la lógica que validan.**
- El repositorio debe compilar y pasar tests en cada commit individual.

---

## 3. Taxonomía Estricta de Tags

Todo commit debe iniciar con una de las siguientes etiquetas en mayúsculas:

| Tag | Definición | Alcance del Commit | Ejemplo Correcto |
|---|---|---|---|
| `[FEAT]` | Feature | Nueva funcionalidad, endpoints, componentes o capacidades. | `[FEAT] auth: add jwt rotation and session validation middleware` |
| `[FIX]` | Bugfix | Corrección de fallos, excepciones, memory leaks o regresiones. | `[FIX] scraper: resolve playwright chromium timeout on dynamic dom` |
| `[ARCH]` | Arquitectura | Refactor estructural, Clean Architecture, desacoplamiento de capas. | `[ARCH] core: isolate repository interfaces from postgres adapter` |
| `[DOCS]` | Documentación | Manuales, diagramas de arquitectura, árboles de proyecto, normas ISO. | `[DOCS] architecture: add system sequence diagram and ISO 25010 specs` |
| `[TEST]` | Testing | Suites completas de pruebas unitarias, de integración, E2E o mocks. | `[TEST] order-processing: implement strict tdd test suite for pricing` |
| `[PERF]` | Rendimiento | Optimización de tiempos de respuesta, queries, memoria o bundling. | `[PERF] cache: implement in-memory redis layer for catalog queries` |
| `[CONFIG]` | Configuración | Tooling, Dockerfiles, variables de entorno, pipelines CI/CD, linters. | `[CONFIG] ci: setup github actions matrix testing and security linter` |

### Anti-Patrones vs Buenas Prácticas

```diff
- git commit -m "update database and models"
- git commit -m "fix error in query"
- git commit -m "add test"
- git commit -m "update readme"
+ [FEAT] catalog: implement product entity, postgres repository and unit tests
+ [DOCS] readme: add database schema diagram and ISO 8000 data quality specs
```

---

## 4. Etiquetado de Versiones en Git (Git Tags)

Cuando el lote de commits represente una versión estable, un sprint cerrado o un hito verificable, el agente debe generar un tag anotado de Git usando versionado semántico (`vMAJOR.MINOR.PATCH`):

```bash
# 1. Crear tag anotado con mensaje descriptivo
git tag -a v1.2.0 -m "Release v1.2.0: [FEAT] Multi-agent pool orchestration and ISO 42001 governance"

# 2. Publicar tags en el repositorio remoto
git push origin --tags
```

### Reglas para Git Tags:
- **vX.Y.Z**:
  - `MAJOR (v2.0.0)`: Cambios incompatibles con versiones anteriores (breaking changes).
  - `MINOR (v1.1.0)`: Nuevas funcionalidades compatibles hacia atrás.
  - `PATCH (v1.0.1)`: Correcciones de errores compatibles hacia atrás.
