# GestionGuardias-App

App de asignación de guardias médicas. Vanilla JS sin build step: `index.html` + `style.css` + `app.js` (~7.000 líneas), backend en Supabase.

Documentos de referencia: `GestionGuardias_PRD.md` (producto), `GestionGuardias_AUDIT.md` (deuda técnica y hoja de ruta).

## Herramientas

**No uses Python para modificar archivos del proyecto.** Las ediciones sobre `.js`, `.html` y `.css` se hacen con las herramientas de edición directa. Python solo para cálculos puntuales que no toquen archivos del proyecto.

## Control de versiones

- `GestionGuardias-BETA` es la **rama de integración** (staging, ggsbeta.vercel.app): **todo el trabajo va a BETA**. Es el único destino de merge por defecto.
- `main` es **PRODUCCIÓN** y está protegida. **Cualquier merge hacia `main` requiere confirmación triple y explícita del usuario**, caso por caso. Nunca se fusiona nada a `main` por iniciativa propia ni como paso implícito.
- Toda funcionalidad, mejora o corrección se desarrolla en una rama temporal (`feature/nombre` o `fix/nombre`); no se comitea directamente sobre `BETA` ni sobre `main`.
- El flujo es: rama temporal → validación del `testing-lead` → merge hacia `BETA`.
- Confirma la rama exacta antes de cualquier push o merge.

## Estilo de trabajo

- Trabaja por sección o motor, nunca sobre todo `app.js` a la vez.
- **No propongas dividir `app.js` en módulos ES.** Es una decisión ya tomada.
- Los cambios son quirúrgicos y respetan la arquitectura existente.

## Calidad del código

Antes de modificar, haz una auditoría estática de la zona que vas a tocar. No dejes atrás:

- Funciones muertas ni callbacks huérfanos.
- Inconsistencias de mayúsculas/minúsculas (case sensitivity) — es una fuente recurrente de bugs en este código.

## UI

Los residentes usan esto en el móvil, a las 3 de la mañana. La legibilidad y el tamaño de los objetivos táctiles pesan más que la densidad de información. Cualquier cambio visual se juzga primero en pantalla pequeña.

## Supabase

Proyecto GestionGuardias: `https://elmpelhplacgkgfuiwno.supabase.co`

**Avisa antes de cualquier operación**, indicando proyecto exacto, tabla y SQL. Espera confirmación.

## Validación

Antes de fusionar hacia BETA, invoca el subagente `testing-lead` para auditar el cambio. Pásale en el brief los fragmentos de código relevantes: está diseñado para trabajar sobre lo que le incluyas, no para explorar `app.js` a ciegas.
