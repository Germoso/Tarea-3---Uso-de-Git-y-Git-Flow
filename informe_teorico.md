# Informe Teórico: Git y Git Flow

**Asignatura:** Programación III  
**Tarea:** #3 – Uso de Git y Git Flow  
**Estudiante:** Germán Moscoso  
**Institución:** ITLA  
**Fecha:** Abril 2026

---

## 1. ¿Qué es Git?

Git es un **sistema de control de versiones distribuido** (DVCS, por sus siglas en inglés) creado por Linus Torvalds en 2005. A diferencia de los sistemas centralizados, Git permite que cada desarrollador tenga una copia completa del historial del repositorio en su máquina local.

### Características principales

- **Distribuido:** No depende de un servidor central; cada clon es un repositorio completo.
- **Integridad:** Cada commit se identifica con un hash SHA-1, garantizando que el historial no pueda alterarse sin que Git lo detecte.
- **Ramas ligeras:** Crear y fusionar ramas en Git es extremadamente rápido y eficiente.
- **Área de staging (índice):** Permite preparar cambios de forma selectiva antes de confirmarlos.

### Comandos esenciales

| Comando | Descripción |
|---|---|
| `git init` | Inicializa un nuevo repositorio local |
| `git clone <url>` | Clona un repositorio remoto |
| `git add <archivo>` | Agrega cambios al área de staging |
| `git commit -m "msg"` | Confirma los cambios con un mensaje |
| `git status` | Muestra el estado del árbol de trabajo |
| `git log` | Muestra el historial de commits |
| `git branch` | Lista, crea o elimina ramas |
| `git checkout` / `git switch` | Cambia entre ramas o commits |
| `git merge` | Fusiona una rama en la rama actual |
| `git push` | Envía commits al repositorio remoto |
| `git pull` | Descarga e integra cambios remotos |

---

## 2. ¿Qué es Git Flow?

**Git Flow** es un modelo de ramificación (branching strategy) propuesto por Vincent Driessen en 2010 en su artículo *"A successful Git branching model"*. Define un conjunto estricto de roles para diferentes ramas y cómo deben interactuar entre sí.

### Ramas principales

| Rama | Propósito |
|---|---|
| `main` | Contiene el código de producción estable. Cada commit representa una versión lanzada. |
| `dev` | Rama de integración continua. Aquí se acumulan los features completados antes del lanzamiento. |

### Ramas de soporte

| Rama | Prefijo | Se crea desde | Se fusiona en | Propósito |
|---|---|---|---|---|
| Feature | `feature/` | `dev` | `dev` | Desarrollar nuevas funcionalidades |
| Release | `release/` | `dev` | `main` y `dev` | Preparar una nueva versión para producción |
| Hotfix | `hotfix/` | `main` | `main` y `dev` | Corregir errores críticos en producción |

### Flujo de trabajo típico

```
main ──────────────────────────────────────────────────► (producción)
  \                                                   /
   dev ──────────────────────────────────────────────►
       \         \         \         \              /
      feat/A   feat/B    feat/C    feat/D         /
       /         /         /         /           /
   dev ◄────────────────────────────────────────
                                                \
                                              release/v1.0
                                                /
   main ◄──────────────────────────────────────
```

### Ventajas de Git Flow

1. **Separación clara de entornos:** `main` siempre está limpio y production-ready.
2. **Trabajo paralelo:** Múltiples equipos pueden trabajar en features independientes sin interferirse.
3. **Ciclos de release controlados:** La rama `release` permite hacer QA y ajustes finales antes de llegar a producción.
4. **Mantenimiento de producción:** Los `hotfix` permiten corregir bugs críticos sin interrumpir el desarrollo activo.

### Desventajas

- Puede resultar **complejo** para proyectos pequeños o equipos que practican entrega continua (CI/CD).
- La acumulación de features en `dev` puede provocar **integration hell** si las ramas viven demasiado tiempo.

---

## 3. Flujo aplicado en este proyecto

El proyecto **Task Master** (aplicación CRUD de gestión de tareas) siguió el siguiente flujo Git Flow:

### 3.1 Estructura de ramas creadas

```
main
├── dev
│   └── qa
├── feature/ui-layout
├── feature/create-task
├── feature/read-tasks
├── feature/toggle-task
└── feature/delete-task
```

### 3.2 Cronología de desarrollo

| Fecha | Actividad |
|---|---|
| 27 mar 2026 | Commit inicial: PDF de tarea e informe teórico en borrador |
| 28 mar 2026 | Branch `feature/ui-layout`: estructura HTML y estilos CSS |
| 29 mar 2026 | Merge de `feature/ui-layout → dev` |
| 30 mar 2026 | Branch `feature/create-task`: lógica de creación de tareas en JS |
| 31 mar 2026 | Merge de `feature/create-task → dev`; branches `feature/read-tasks` y `feature/toggle-task` |
| 01 abr 2026 | Branch `feature/delete-task`; merges de read-tasks y toggle-task |
| 02 abr 2026 | Merge de `feature/delete-task → dev`; release a `qa` |
| 03 abr 2026 | Release final: merge `qa → main`; tag `v1.0.0` |

### 3.3 Comandos utilizados

```bash
# Inicialización
git init
git branch -m main

# Crear y trabajar en un feature
git checkout -b feature/ui-layout
git add index.html style.css
git commit -m "feat: implement basic UI layout and styling"

# Integrar feature en dev
git checkout dev
git merge --no-ff feature/ui-layout -m "Merge feature/ui-layout into dev"

# Preparar release (QA)
git checkout qa
git merge --no-ff dev -m "release: merge dev into qa for v1.0 testing"

# Lanzar a producción
git checkout main
git merge --no-ff qa -m "release: merge qa into main — v1.0.0"
git tag -a v1.0.0 -m "Version 1.0.0 — Task Master CRUD app"
```

---

## 4. Buenas prácticas de commits

Se siguió la convención **Conventional Commits** para los mensajes:

| Tipo | Uso |
|---|---|
| `feat:` | Nueva funcionalidad |
| `fix:` | Corrección de errores |
| `docs:` | Cambios en documentación |
| `style:` | Cambios de formato sin afectar lógica |
| `refactor:` | Refactorización de código |
| `chore:` | Tareas de mantenimiento |

---

## 5. Conclusión

Git y Git Flow son herramientas esenciales en el desarrollo de software profesional. Git provee el mecanismo de control de versiones, mientras que Git Flow establece un **contrato de trabajo en equipo** que define claramente cómo y cuándo se crean, integran y eliminan ramas.

Para proyectos académicos como éste, Git Flow permite demostrar un proceso de desarrollo organizado, trazable y alineado con estándares de la industria. Cada commit representa un paso deliberado en el ciclo de vida del software, desde la concepción de una funcionalidad hasta su despliegue en producción.

---

*Documento generado como parte de la Tarea #3 de Programación III — ITLA, Abril 2026.*
