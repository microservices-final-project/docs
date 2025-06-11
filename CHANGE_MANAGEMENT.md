### Proceso formal de Change Management


* **Aprobación técnica:**
  Revisión por arquitecto o líder técnico antes de pasar a develop.
* **Ejecución controlada:**
  Implementación mediante pull requests con revisión cruzada.
* **Validación en entornos de staging/preproducción:**
  Requiere pruebas automáticas y manuales antes de aprobación para producción.
* **Despliegue:**
  Automatizado vía pipelines (CI/CD).
* **Monitoreo post-release:**
  Alerta de errores, métricas, y validación funcional inmediata.

---

## 📝 2. Generación automática de Release Notes

### ⚙️ Implementación sugerida

* **Commit estándar (Convencional Commits):**

  * Formato: `feat:`, `fix:`, `chore:`, `refactor:`, etc.
  * Ejemplo: `feat(cart): add support for discount codes`

* **Herramientas:**

  * `semantic-release`
  * `git-chglog` o `standard-version`

* **Release Notes por microservicio:**

  * Cada microservicio genera su changelog en cada release tag (ej. `v1.2.3`)
  * Publicación en:

    * Repositorio GitHub/GitLab
    * Documentación central (Confluence, Notion, etc.)
    * Slack/email automático (si aplica)

---

## ⏪ 3. Planes de Rollback

### 📜 Documentación por microservicio:

* Estrategia de rollback (por versión/tag)
* Scripts de reversión de base de datos (si aplica)
* Validación post-rollback
* Checklist para rollback:

  * ¿Afecta integraciones?
  * ¿Hay cambios en el schema de base de datos?
  * ¿Se requiere restaurar backups?

### 🧰 Técnicas utilizadas:

* **Blue/Green Deployments** o **Canary Releases** con:

  * Kubernetes (via ArgoCD, Spinnaker, etc.)
  * Spring Boot Actuator + readiness/liveness probes
* **Rollback automático en caso de fallo**:

  * Configurado en los pipelines de CI/CD con validación post-deploy

---

## 🏷️ 4. Etiquetado de Releases

### 📌 Convención de versiones (SemVer)

* `MAJOR.MINOR.PATCH` (Ej. `3.2.1`)
* Cada microservicio tiene su propio esquema de versionado

### 🔖 Etiquetado en Git:

* Automático al hacer `merge` a la rama `main`
* Incluye release notes como parte del tag

### 🗂️ Visibilidad:

* Tags accesibles vía:

  * Git (por desarrolladores)
  * Registry (por devops)

---

## 📈 Ejemplo de Automatización con GitHub Actions

```yaml
name: Release Notes and Tag

on:
  push:
    branches:
      - main

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Generate Release Notes
        uses: actions/create-release@v1
        with:
          tag_name: v1.2.3
          release_name: Release v1.2.3
          body: ${{ steps.changelog.outputs.notes }}
```
