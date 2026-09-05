# SIZIGIA — Documento maestro del evento
### Solé · lanzamiento de "Amar = Perder"

Última actualización: usa este documento como punto de partida único — aquí están todos los links, archivos y pendientes en un solo lugar.

---

## 1. Datos duros del evento

| Campo | Detalle |
|---|---|
| Nombre del evento | SIZIGIA |
| Tagline | amar · perder · volver a empezar |
| Artista | Solé — sencillo "Amar = Perder" |
| Open act | Juan Camilo Ortiz |
| Fecha | Viernes 11 de septiembre, 2026 |
| Hora de acceso | 7:00 p.m. |
| Lugar | El Taller de la Hormiga y la Cigarra — Cra 4 # 4-17, Sogamoso, Boyacá |
| Capacidad | 40–50 personas |
| Apoyan | El Taller de la Hormiga y la Cigarra + Ministerio de Cultura |
| Vestimenta | Negro + acento cobre u óxido |
| Presupuesto | $1.000.000 COP |

---

## 2. Links activos

| Qué es | Link | Notas |
|---|---|---|
| **Invitación + confirmación (la que va en el QR)** | https://invitacionsole.vercel.app/ | Página pública. Formulario conecta directo a la hoja de cálculo. |
| **Google Form (motor de confirmaciones)** | https://docs.google.com/forms/d/e/1FAIpQLSfcn-bteA7n46lkeLuUuPxRlQdC-DhWJnUvGP1tNJsHib4uHA/viewform | Ya publicado y aceptando respuestas. Los invitados nunca lo ven directamente. |
| **Google Sheet (respuestas en vivo)** | https://docs.google.com/spreadsheets/d/1JnVEkGCrXmExu7F1zbX3SWgvoWx0B42bDvDbPPK3mHs/edit?gid=1231111417 | Aquí caen nombre, teléfono y correo de cada confirmación, en tiempo real. |
| Sistema de control de acceso (escáner QR individual) | https://sizigia-hazel.vercel.app/ | Construido antes de definir el flujo de RSVP único. Actualmente en paralelo — ver nota en pendientes. |

---

## 3. Archivos generados

| Archivo | Para qué sirve |
|---|---|
| `Invitacion_SIZIGIA_RSVP.html` | Código fuente de la invitación pública (la que está desplegada en Vercel). Súbelo aquí si necesitas actualizarla. |
| `QR_SIZIGIA.png` | QR con etiqueta de marca, listo para compartir. Apunta a la invitación. |
| `QR_SIZIGIA_limpio.png` | Mismo QR sin texto, para insertar en otros diseños. |
| `Base_Datos_Invitados_SIZIGIA.xlsx` | Plantilla de lista de invitados con códigos únicos (pensada para el sistema de control de acceso individual). |
| `Brief_Invitacion_SIZIGIA_Sole.md` | Brief de diseño original con paleta, tipografía y reglas de marca de Solé. |
| `sizigia_control_acceso.html` | Código fuente del sistema de escaneo QR individual (app aparte). |

---

## 4. Datos técnicos (por si hay que reconectar algo)

**Google Form — códigos de campo (`entry.`):**
- Nombre completo → `entry.1138576823`
- Teléfono / WhatsApp → `entry.724068319`
- Correo electrónico → `entry.84926344`

**Endpoint de envío:**
`https://docs.google.com/forms/d/e/1FAIpQLSfcn-bteA7n46lkeLuUuPxRlQdC-DhWJnUvGP1tNJsHib4uHA/formResponse`

Estos códigos están escritos dentro de `Invitacion_SIZIGIA_RSVP.html`. Si en algún momento se **edita el Google Form** (se agregan o reordenan preguntas), estos códigos pueden cambiar y habría que volver a sacarlos con el truco del "enlace pre-rellenado".

---

## 5. Pendientes / decisiones abiertas

- [ ] **Definir qué pasa con el sistema de control de acceso individual** (`sizigia-hazel.vercel.app`): ¿se usa la noche del evento como lista de chequeo en la puerta, o se descarta ahora que existe el flujo de RSVP único?
- [ ] **Cambios pendientes en la invitación** (a definir contigo — anota aquí qué necesitas ajustar)
- [ ] Confirmar lineamientos de logo del Ministerio de Cultura, si aplica
- [ ] Definir plan de música (playlist por momentos del evento)
- [ ] Definir visuales/señalética física del evento

---

## 6. Flujo resumido para un invitado

1. Recibe el QR o el link por WhatsApp/Instagram
2. Escanea → ve la invitación con foto de la banda y datos del evento
3. Llena nombre, teléfono, correo → clic en "Confirmar"
4. Ve aparecer su ticket personalizado con su nombre
5. Descarga el ticket como imagen de recuerdo
6. Tú ves la confirmación aparecer en el Google Sheet en tiempo real
