# Tareas del día 📋

App web local de tareas diarias con prioridades, rollover automático, % de cumplimiento real y bloque dedicado al **Daily** (reunión semanal con sub-temas).

Pensada para correr **sin instalación, sin servidor, sin login** — solo abres el HTML en tu navegador y listo. Las tareas se guardan en el `localStorage` del navegador (cada PC mantiene su propia lista).

---

## ¿Qué hace?

| Funcionalidad | Detalle |
|---|---|
| **Tareas por día** | Cada día tiene su propia lista. Navegación con botones Ant./Sig. o **click en la fecha para abrir el calendario nativo** y saltar a cualquier día. |
| **Prioridades** | Alta (rojo), Media (ámbar), Baja (azul). Se ordenan automáticamente. |
| **Rollover inteligente** | Las tareas no completadas se **mueven automáticamente al día siguiente** (a las 6 PM o al abrir la app al día siguiente). Quedan marcadas como `VENCIDA` en su día original (cuentan como NO completadas → el % refleja cumplimiento real). |
| **% de cumplimiento real** | Total, completadas y porcentaje del día. No se infla al 100% por mover tareas — refleja lo que efectivamente hiciste ese día. |
| **Daily — reunión semanal** | Bloque dedicado al final de la lista (auto-creado cada día) donde agregas los temas a tratar en la reunión semanal con tu equipo. Cada tema es una sub-tarea con su propio checkbox. NO cuenta en el % del día. |
| **Auto-orden** | Las completadas/vencidas se mueven solas al final → la zona "activa" arriba siempre limpia. |
| **100% offline** | Funciona sin internet. Datos en `localStorage` del navegador. |

---

## Stack

- **React 18** + **Babel standalone** (via CDN, sin build step)
- **HTML / CSS / JS** en un solo archivo
- **localStorage** para persistencia (clave `daily_tasks_v2`)

---

## Cómo usarla

1. Descarga el archivo `index.html`
2. Doble click → se abre en tu navegador
3. (Opcional) Anclalo a la barra de tareas o ponlo de acceso directo en el Escritorio

> No requiere instalación. No requiere internet (después de la primera carga de los CDNs).

---

## Estructura

```
tareas-diarias-app/
├── index.html        # Toda la app (HTML + CSS + React + lógica)
├── README.md
└── .gitignore
```

Un solo archivo. Todo dentro. ~250 líneas.

---

## Personalización

Las constantes al inicio del `<script>` controlan el look y el comportamiento:

```js
// Colores de cada prioridad
const PRIORITIES = {
  alta:  { label: "Alta",  bg: "#FAECE7", text: "#993C1D", border: "#F0997B" },
  media: { label: "Media", bg: "#FAEEDA", text: "#854F0B", border: "#FAC775" },
  baja:  { label: "Baja",  bg: "#E6F1FB", text: "#185FA5", border: "#85B7EB" },
};

// Colores del bloque Daily
const DAILY_BG     = "#FAF7FC";  // fondo
const DAILY_TEXT   = "#7B5BA8";  // texto
const DAILY_BORDER = "#DDD0EF";  // borde
```

Para cambiar la **hora del rollover automático** (default 6 PM), busca el `useEffect` con `new Date(..., 18, 0, 0)` y ajusta el `18`.

---

## Decisiones de diseño

- **Sin build step:** Babel standalone transpila el JSX directo en el browser. No hay `npm install`, no hay bundler, no hay deploy. Solo abres el archivo.
- **`localStorage` en vez de DB:** Es una app personal de productividad — no necesita sincronización entre dispositivos. Si quisieras eso, podrías reemplazar `loadTasks` / `saveTasks` por llamadas a una API.
- **Rollover con `rolledOver` flag en lugar de mover-y-borrar:** Mover y borrar deja el día original en 100% falso. Marcar como `rolledOver` permite que el % del día anterior refleje la realidad. Las pendientes se DUPLICAN al día siguiente (con `rolledFrom`) para que las puedas trabajar mientras quedan "vencidas" en el día original.
- **Daily como tarea especial (`isDaily: true`):** Es una excepción al modelo de "tarea normal" — tiene sub-tareas, no se puede completar, no cuenta en el %. Se podría haber sacado como una entidad aparte pero mantenerla en el mismo array simplifica el storage y la navegación.

---

## Licencia

Uso personal. Sin restricciones — sí lo quieres adaptar, adelante.
