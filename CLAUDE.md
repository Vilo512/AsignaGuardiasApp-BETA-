# GestionGuardias-App

App de asignación de guardias médicas. Vanilla JS sin build step: `index.html` + `style.css` + `app.js` (~7.250 líneas), backend en Supabase.

## Documentos y qué contiene cada uno

No son intercambiables. Confundirlos es la causa habitual de leer 13.000 tokens para no encontrar lo que se buscaba.

| Archivo | Qué es | Cuándo se lee |
|---|---|---|
| `GestionGuardias_PRD.md` | **Especificación** de producto, por dominio (§8 motor de turnos, §9 rotación, §11 mercadillo…). No contiene un plan. | Por secciones, cuando toca implementar esa zona. **Nunca entero.** |
| `GestionGuardias_AUDIT.md` | Deuda técnica y hoja de ruta. | Por secciones, al priorizar. **Nunca entero.** |
| `GestionGuardias_HANDOVER_<fecha>.md` | Estado real al cerrar la última sesión. El más reciente manda. | §11 (arranque) + §9 (pendientes) al abrir sesión. |
| `GestionGuardias_BACKLOG.md` | Ideas capturadas fuera de secuencia, ya triadas. | Al abrir sesión y al cerrar. |
| `GestionGuardias_REDISENO.md` | Plan del rediseño visual + PWA. | Cuando el punto en curso sea de rediseño. |

## Ciclo de sesión

**Una sesión = un punto del backlog.** Cerrar y abrir una nueva es más barato que encadenar: cada turno re-envía todo el historial, así que una sesión que arrastra tres puntos paga el peso de los tres en cada turno restante, y la calidad cae al final.

1. **Apertura barata.** Leer §11 y §9 del handover más reciente. Nada más por defecto. Si el usuario ya dice qué toca, ni eso.
2. **Contexto bajo demanda.** La sección del PRD y la zona de `app.js` que haga falta, cuando haga falta. Nunca lecturas completas "por contexto".
3. **Ejecución.** Rama temporal → `testing-lead` → merge a BETA.
4. **Cierre, siempre.** Actualizar pendientes del handover y dejar escrito en una frase qué toca después. Un handover se escribe al terminar un punto, no cuando el contexto está saturado.

Coste objetivo de arranque: **2-3k tokens**.

## Ideas nuevas a media sesión

Cuando el usuario propone algo fuera del punto en curso — sobre todo si toca lógica ya escrita o llega a destiempo:

**No implementar. No abrir el PRD entero. Y no dejarla flotando.** Se tría en el momento —que es cuando el contexto ya está cargado y sale casi gratis— y entra en la cola **con posición y veredicto**. Nada queda "pendiente de decidir": eso obliga a re-deliberar en frío, que es el coste que este ciclo evita.

### Las cuatro preguntas del triaje

1. **¿Llega tarde?** ¿Pertenecía a un punto ya cerrado? Si sí, implica reabrir algo ya validado — decirlo, con el coste.
2. **¿Qué impacto tiene?** Tres ejes, concretos: qué **motor** hay que reabrir · qué **lógica ya escrita** se retoca, o si solo añade · qué le hace al **render** (juzgado en móvil, ver §UI).
3. **¿Dónde es más barato meterla?** Detrás del punto de la cola que **ya vaya a abrir ese mismo motor**: abrir un motor dos veces se paga dos veces. Si ninguno lo abre, final de cola.
4. **¿Debería morir?** Dos tests. **Redundante**: ya lo cubre algo existente o un punto encolado. **Function bloat**: superficie nueva para un caso marginal — en una app que se usa a las 3 de la mañana, cada opción extra es carga cognitiva. El descarte se **propone con motivo**; lo decide el usuario.

Anotar en `GestionGuardias_BACKLOG.md`:

```
### [ID] Título
- **Qué:** una frase
- **¿Tarde?:** no · sí — era de <punto cerrado>, reabrirlo cuesta <qué>
- **Impacto:** motor(es) a reabrir · lógica que se retoca · efecto visual
- **Dónde:** detrás de <punto>, que ya abre <motor> · final de cola
- **Veredicto:** entra · descartada — redundante con <X> / function bloat
- **Anotada:** YYYY-MM-DD
```

### Reglas duras

- **Acabar lo que hay entre manos manda.** El punto en curso no se interrumpe nunca, ni aunque la idea sea mejor.
- Adelantar solo si el usuario lo pide. Aun así, cerrar antes lo que esté a medias.
- Si invalida algo **ya construido**: se anota y ya está. Lo hecho, hecho está; no reordena nada por sí solo.
- Si invalida un punto de la cola **aún no empezado**: anotarlo *en ese punto*, para no construir algo que ya sabemos que se tira. Anotar, no reordenar.

## Decisiones cerradas

Cerradas por defecto: no se re-proponen cada sesión. Pero cada una lleva motivo, fecha y **disparadores de revisión**. Si un disparador se cumple, avisar **una vez**, con el dato concreto, y decide el usuario. Sin disparador cumplido, silencio.

### Vanilla JS sin build step · `app.js` monolítico
*Cerrada. Última revisión: 2026-08-26.*

**Motivo:** sin build step no hay tooling que mantener, se despliega en Vercel tal cual y el ciclo de edición es inmediato. Migrar a módulos/Tailwind/bundler es un desvío grande que para en seco la hoja de ruta.

**Disparadores — avisar si:**
- `app.js` supera las **8.000 líneas**.
- Tercera regresión en el **mismo motor** en sesiones distintas.
- Un bug de case-sensitivity cuesta **más de una sesión** resolverlo.
- Un cambio visual obliga a tocar **más de 3 zonas de `style.css`** a la vez (señal de que falta un sistema de estilos).

### `main` protegida · BETA como integración
*Cerrada. Sin disparadores: es política, no técnica.*

## Control de versiones

- `GestionGuardias-BETA` es la **rama de integración** (staging, ggsbeta.vercel.app): **todo el trabajo va a BETA**. Es el único destino de merge por defecto.
- `main` es **PRODUCCIÓN** y está protegida. **Cualquier merge hacia `main` requiere confirmación triple y explícita del usuario**, caso por caso. Nunca se fusiona nada a `main` por iniciativa propia ni como paso implícito.
- Toda funcionalidad, mejora o corrección se desarrolla en una rama temporal (`feature/nombre` o `fix/nombre`); no se comitea directamente sobre `BETA` ni sobre `main`.
- El flujo es: rama temporal → validación del `testing-lead` → merge hacia `BETA`.
- Confirma la rama exacta antes de cualquier push o merge.

## Calidad del código

- Trabaja por sección o motor, nunca sobre todo `app.js` a la vez. Los cambios son quirúrgicos y respetan la arquitectura existente.
- Antes de modificar, auditoría estática de la zona. No dejes atrás funciones muertas ni callbacks huérfanos.
- Vigila las inconsistencias de mayúsculas/minúsculas: es una fuente recurrente de bugs en este código.
- `node --check` no basta para dar `app.js` por bueno: un throw de nivel superior mata los `let`/`const` posteriores y el hoisting lo disimula. Comprobar consola del navegador y las utilidades `window.*` del final del archivo.

## UI

Los residentes usan esto en el móvil, a las 3 de la mañana. La legibilidad y el tamaño de los objetivos táctiles pesan más que la densidad de información. Cualquier cambio visual se juzga primero en pantalla pequeña.

## Validación

Antes de fusionar hacia BETA, invoca el subagente `testing-lead`. Pásale en el brief los fragmentos de código relevantes: está diseñado para trabajar sobre lo que le incluyas, no para explorar `app.js` a ciegas. Para iteraciones visuales, `design-reviewer` con el CSS/HTML en el brief.

No abras subagentes para planificar: arrancan en frío y vuelven a derivar contexto ya cargado. Su valor es la revisión con ojos frescos sobre un diff acotado.

## Supabase

Proyecto GestionGuardias: `https://elmpelhplacgkgfuiwno.supabase.co`

**Avisa antes de cualquier operación**, indicando proyecto exacto, tabla y SQL. Espera confirmación.

## Herramientas

**No uses Python para modificar archivos del proyecto.** Las ediciones sobre `.js`, `.html` y `.css` se hacen con las herramientas de edición directa. Python solo para cálculos puntuales que no toquen archivos del proyecto.
