# PROMPT DE CONTEXTO TÉCNICO — Sistema SIZIGIA (Solé)

Usa este texto como prompt/contexto si necesitas que otro desarrollador, o otra sesión de IA, continúe este trabajo desde cero.

---

## Contexto general

Estoy construyendo el sistema digital para **SIZIGIA**, el evento de lanzamiento del primer sencillo de la banda Solé ("Amar = Perder"), el 11 de septiembre de 2026 en El Taller de la Hormiga y la Cigarra (Sogamoso, Boyacá). Evento pequeño (40-50 personas), apoyado por el Ministerio de Cultura.

El sistema tiene dos partes que quiero que entiendas como piezas separadas (se construyeron en momentos distintos y con propósitos distintos):

1. **Invitación pública con RSVP + ticket souvenir** (en producción, es la que se está usando)
2. **Sistema de control de acceso por QR individual** (construido antes, actualmente en pausa/decisión pendiente)

---

## PARTE 1 — Invitación + RSVP + Ticket souvenir (activa)

**Objetivo:** un solo link/QR público. Cualquier invitado lo abre, ve la invitación con foto de la banda, llena un formulario (nombre, teléfono, correo), y al confirmar ve aparecer su nombre insertado en un "ticket" con diseño de marca, que puede descargar como imagen de recuerdo (souvenir).

**Stack:** HTML + CSS + JavaScript vanilla, un solo archivo autocontenido. Sin frameworks, sin build step. Desplegado como sitio estático en Vercel (drag-and-drop manual del archivo, sin CLI ni CI/CD conectado a git).

**Archivo:** `Invitacion_SIZIGIA_RSVP.html`
**URL en producción:** https://invitacionsole.vercel.app/

### Cómo funciona la "base de datos" de confirmaciones

No hay backend propio. Se usa **Google Forms como backend gratuito**:

1. Existe un Google Form llamado "SIZIGIA — Confirmación de asistencia" con 3 preguntas de tipo respuesta corta: Nombre completo, Teléfono/WhatsApp, Correo electrónico.
2. Ese Form está vinculado a un Google Sheet (respuestas en vivo).
3. **El Form debe estar publicado y "aceptando respuestas"** — si no, cualquier envío se pierde en silencio sin dar error (esto ya nos pasó, ver sección de gotchas).
4. Cada pregunta de Google Forms tiene un ID único tipo `entry.XXXXXXXXX`. Estos se obtuvieron generando un "enlace pre-rellenado" desde el propio Form (Menú ⋮ → "Obtener enlace con datos rellenados previamente"), llenando datos de prueba, y extrayendo los `entry.` de la URL resultante.
5. En el HTML, el formulario de la invitación NO apunta al Google Form visualmente — el usuario nunca lo ve. En vez de eso, al hacer submit, un `fetch()` en JavaScript manda un POST directo al endpoint de respuestas de Google Forms:

```javascript
const GOOGLE_FORM_ACTION = "https://docs.google.com/forms/d/e/1FAIpQLSfcn-bteA7n46lkeLuUuPxRlQdC-DhWJnUvGP1tNJsHib4uHA/formResponse";
const ENTRY_NOMBRE = "entry.1138576823";
const ENTRY_TELEFONO = "entry.724068319";
const ENTRY_CORREO = "entry.84926344";

await fetch(GOOGLE_FORM_ACTION, {
  method: 'POST',
  mode: 'no-cors',
  headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
  body: new URLSearchParams({
    [ENTRY_NOMBRE]: nombre,
    [ENTRY_TELEFONO]: telefono,
    [ENTRY_CORREO]: correo
  }).toString()
});
```

**Limitación importante:** `mode: 'no-cors'` significa que el `fetch` SIEMPRE se resuelve como "exitoso" del lado del navegador, sin importar si Google realmente aceptó o rechazó los datos (la respuesta es opaca). No hay forma de detectar fallos reales desde el JavaScript — la única forma de verificar es revisando el Google Sheet directamente.

### El "ticket souvenir"

Después del submit (asumido exitoso), el JS oculta el formulario y muestra el nombre insertado dentro del diseño del ticket, más un botón "Guardar ticket como imagen" que usa la librería `html2canvas` (cargada desde CDN) para capturar el `div` del ticket como PNG y forzar su descarga:

```javascript
const canvas = await html2canvas(document.querySelector('.card'), { backgroundColor: '#0d0e0b', scale: 2 });
const link = document.createElement('a');
link.download = 'ticket_sizigia_' + nombre.replace(/\s+/g,'_') + '.png';
link.href = canvas.toDataURL('image/png');
link.click();
```

### La foto de fondo

La foto de la banda (fondo del ticket) se extrajo de un export previo hecho en Claude Design (un archivo HTML "bundler" que empaquetaba assets en base64 dentro de `<script type="__bundler/manifest">`). Se decodificó, se redujo de ~4600px/2.2MB a 920px/110KB con PIL, y se re-embebió como `data:image/jpeg;base64,...` directamente en el CSS del HTML final, para mantener todo en un solo archivo.

---

## PARTE 2 — Sistema de control de acceso QR individual (en pausa)

**Objetivo original:** cada invitado tiene un código único (`SIZIGIA-001`, `SIZIGIA-002`...), se le genera un QR personal, y en la puerta del evento se escanea con la cámara del celular para marcar su check-in.

**Archivo:** `sizigia_control_acceso.html`
**URL:** https://sizigia-hazel.vercel.app/

**Stack:** HTML/CSS/JS vanilla. Usa:
- `qrcodejs` (davidshimjs) vía CDN para generar los QR
- `jsQR` vía CDN + `getUserMedia` para escanear con la cámara
- `localStorage` del navegador para persistir la lista de invitados y su estado de check-in

**Gotcha importante ya resuelto:** la primera versión usaba `window.storage`, una API que **solo existe dentro del entorno de artefactos de Claude.ai** — no existe en absoluto en un sitio desplegado de forma independiente (Vercel, Netlify, etc.). Esto causaba que el guardado fallara en silencio (el error quedaba atrapado en un try/catch) y por eso el modal del QR nunca terminaba de abrirse bien. Se reemplazó por `localStorage`, que sí es una API estándar del navegador.

**Limitación de diseño aceptada:** como usa `localStorage`, los datos solo viven en el navegador/dispositivo donde se crearon. Para este evento (un solo celular en la puerta) es suficiente; NO sirve si se necesitan varios dispositivos sincronizados en tiempo real sin backend real.

**Estado actual:** no está claro si se seguirá usando la noche del evento en paralelo al sistema de RSVP, o si se descarta a favor de solo tener la lista en el Google Sheet. Pendiente de decisión.

---

## Gotchas / lecciones aprendidas (para no repetir errores)

1. **Google Forms debe estar "Publicado"** antes de aceptar respuestas — un formulario recién creado puede parecer listo pero seguir sin aceptar envíos hasta que se publique explícitamente.
2. **`window.storage` ≠ `localStorage`** — el primero es exclusivo del entorno de artefactos de Claude.ai, el segundo es estándar de cualquier navegador. Para cualquier sitio desplegado fuera de Claude.ai, usar siempre `localStorage` (o un backend real si se necesita compartir datos entre dispositivos).
3. **`fetch` con `mode: 'no-cors'` nunca reporta error real** — cualquier validación de que los datos llegaron debe hacerse revisando el destino final (en este caso, el Google Sheet), no confiando en que el `fetch` no lanzó excepción.
4. Los archivos "bundler" exportados desde Claude Design empaquetan todo (HTML, CSS, JS, imágenes) como JSON/base64 dentro de `<script type="__bundler/...">` tags — son funcionales pero muy frágiles de editar a mano. Es más confiable reconstruir el HTML desde cero replicando el diseño visual que intentar editar el bundle directamente.
5. Despliegue: no hubo acceso de red disponible desde el entorno de Claude para hacer `vercel deploy` de forma automática (dominios de Vercel no estaban en la lista blanca de red). La solución que funcionó fue: generar el archivo, descargarlo, y arrastrarlo manualmente a la pestaña "Deployments" del dashboard de Vercel (sin necesidad de CLI ni cuenta conectada a git).

---

## Qué necesito que hagas ahora

*(completa esta sección según lo que necesites pedir a continuación — el resto del documento es contexto para que no tengas que repetir toda la historia)*
