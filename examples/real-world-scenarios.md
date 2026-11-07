# Casos de Uso Reales: El Agente en Acción

Este documento describe tres escenarios paso a paso que ilustran la aplicación exacta del protocolo.

---

## Escenario 1: Implementación de Feature con Límite de 3 Commits Cohesivos

### Contexto:
Un agente recibe la orden de implementar autenticación JWT en una API FastAPI existente.

### ❌ Lo que haría una IA sin esta skill (Mal):
Ejecutaría 9 commits:
1. `add pyjwt to requirements`
2. `create auth utils`
3. `add user model`
4. `fix import error in auth`
5. `add login endpoint`
6. `add tests`
7. `fix test error`
8. `update readme`
9. `fix formatting`

### ✅ Lo que hace el agente con `agent-github-improvement`:
Planifica el trabajo en **3 unidades cohesivas etiquetadas**:

```bash
# Commit 1: Modelo de datos, utilidades y suite TDD
git add requirements.txt src/models/user.py src/core/security.py tests/test_security.py
git commit -m "[FEAT] auth: implement user credentials model, jwt helpers and security test suite"

# Commit 2: Controlador HTTP, middleware y validación
git add src/api/routes/auth.py src/api/middleware.py tests/test_auth_routes.py
git commit -m "[FEAT] api: wire login endpoints, bearer middleware and integration tests"

# Commit 3: Documentación técnica y especificaciones ISO 27001
git add README.md docs/auth-flow.md
git commit -m "[DOCS] security: document auth sequence, env configuration and ISO 27001 compliance"

# Publicar limpiamente
git push origin main
```
**Resultado:** Historial profesional, 3 commits limpios, trazabilidad total, cero spam.

---

## Escenario 2: Prueba Experimental de Alto Riesgo (Branch Temporal)

### Contexto:
El agente necesita evaluar si migrar de `requests` a `httpx` con HTTP/2 mejora el rendimiento del scraper sin romper la suite de scraping existente.

### Pasos Ejecutados por el Agente:

1. **Crear rama temporal de prueba:**
   ```bash
   git checkout -b test/spike-httpx-http2
   ```

2. **Ejecutar prueba y benchmark comparativo:**
   - Implementa prototipo.
   - Corre suite de benchmarks.
   - Observa que HTTP/2 reduce la latencia en un 42% y pasa todos los tests.

3. **Integrar mediante Squash Merge en 1 único commit cohesivo:**
   ```bash
   git checkout main
   git merge --squash test/spike-httpx-http2
   git commit -m "[PERF] scraper: migrate engine to httpx with http2 support (+42% throughput)"
   ```

4. **Eliminar la rama de prueba (no dejar basura):**
   ```bash
   git branch -D test/spike-httpx-http2
   git push origin main
   ```

**Resultado:** Se probó de forma aislada sin arriesgar `main`, se integró en un solo commit impecable y el árbol de branches quedó 100% limpio.

---

## Escenario 3: Generación de Documentación y Mapeo ISO 25010 en Repo Legacy

### Contexto:
Un repositorio existente carece de README o documentación arquitectónica. El usuario solicita documentarlo.

### Pasos Ejecutados por el Agente:

1. **Inspección del Proyecto:**
   - Lee la estructura de archivos y mapea las dependencias.
2. **Generación del README Estructurado:**
   - Incluye el árbol de carpetas con comentarios descriptivos.
   - Genera el diagrama de arquitectura en Mermaid.
   - Añade la sección de **Normas ISO (ISO/IEC 25010 y 12207)** indicando cómo el software cumple con mantenibilidad, seguridad y fiabilidad.
3. **Commit y Tag:**
   ```bash
   git add README.md
   git commit -m "[DOCS] architecture: create comprehensive readme with tree layout and ISO 25010 specs"
   git push origin main
   ```
