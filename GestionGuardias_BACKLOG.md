# Backlog de ideas

Ideas que aparecen fuera de secuencia. Se anotan **en el momento en que surgen**, ya triadas: es cuando el contexto está cargado y el triaje sale casi gratis. Reconstruirlo en frío semanas después cuesta una lectura entera del PRD.

Esto **no** es la lista de tareas pendientes — esa vive en el §9 del handover más reciente. Aquí solo llega lo que rompe la secuencia planificada.

**Nada entra sin posición y veredicto.** Una idea capturada ya viene triada — no hay bandeja de "pendiente de decidir". Dejarla flotando obliga a re-deliberar semanas después con el contexto frío, que es el coste que este ciclo existe para evitar.

Acabar lo que hay entre manos manda sobre lo nuevo, aunque lo nuevo sea mejor idea. Pero la posición no es "al final" por inercia: va **detrás del punto que ya vaya a abrir el mismo motor**, porque abrir un motor dos veces se paga dos veces. Solo si ninguno lo abre, final de cola.

## Triaje — las cuatro preguntas

1. **¿Llega tarde?** ¿Era de un punto ya cerrado? Entonces implica reabrir algo ya validado.
2. **¿Qué impacto tiene?** Motor a reabrir · lógica ya escrita que se retoca · efecto sobre el render (en móvil).
3. **¿Dónde es más barato meterla?** Junto al punto que ya toca ese motor.
4. **¿Debería morir?** Redundante con algo existente o encolado · *function bloat*: superficie nueva para un caso marginal. Se propone con motivo; decide el usuario.

## Formato

```
### [ID] Título
- **Qué:** una frase
- **¿Tarde?:** no · sí — era de <punto cerrado>, reabrirlo cuesta <qué>
- **Impacto:** motor(es) a reabrir · lógica que se retoca · efecto visual
- **Dónde:** detrás de <punto>, que ya abre <motor> · final de cola
- **Veredicto:** entra · descartada — redundante con <X> / function bloat
- **Anotada:** YYYY-MM-DD
```

Al empezar un punto de la cola se mueve a las pendientes del handover y se marca aquí como promovido, con fecha. Las descartadas **se quedan, con el motivo** — evita volver a proponer lo mismo dentro de tres meses.

---

## Cola

*El orden de esta lista **es** la decisión. Insertar según la posición asignada al capturar.*

*(vacía)*

## Promovidas

*(vacía)*

## Descartadas

*(vacía)*
