# SIZIGIA — Solé

Sistema digital del evento de lanzamiento **SIZIGIA** (Solé, sencillo "Amar = Perder"), 11 de septiembre de 2026, El Taller de la Hormiga y la Cigarra, Sogamoso, Boyacá.

> **Si eres un agente/IA retomando este proyecto:** lee `docs/PROMPT_CONTEXTO_TECNICO_SIZIGIA.md` primero — tiene toda la arquitectura, decisiones técnicas y errores ya resueltos. No repitas esos errores.

---

## Estructura del repo

```
sizigia-repo/
├── invitacion/          → invitación pública con RSVP + ticket souvenir (EN PRODUCCIÓN)
│   └── index.html
├── control-acceso/      → sistema de QR individual para check-in en puerta (EN PAUSA)
│   └── index.html
├── docs/
│   ├── PROMPT_CONTEXTO_TECNICO_SIZIGIA.md   → LEE ESTO PRIMERO si eres un agente
│   ├── SIZIGIA_Documento_Maestro.md         → links, datos del evento, pendientes
│   └── Brief_Invitacion_SIZIGIA_Sole.md     → brief de marca/diseño original
├── data/
│   └── Base_Datos_Invitados_SIZIGIA.xlsx    → plantilla de lista de invitados
└── assets/
    ├── QR_SIZIGIA.png            → QR con etiqueta, listo para compartir
    └── QR_SIZIGIA_limpio.png     → QR sin texto
```

## URLs en producción

| Qué es | URL |
|---|---|
| Invitación (la que va en el QR) | https://invitacionsole.vercel.app/ |
| Sistema de control de acceso (en pausa) | https://sizigia-hazel.vercel.app/ |
| Google Form (backend de confirmaciones) | ver `docs/PROMPT_CONTEXTO_TECNICO_SIZIGIA.md` |
| Google Sheet (respuestas en vivo) | ver `docs/SIZIGIA_Documento_Maestro.md` |

## Cómo desplegar cambios

Este proyecto se despliega en Vercel como sitio estático (sin build step). Si este repo ya está conectado a Vercel (Project → Settings → Git), cualquier `git push` a la rama principal despliega automáticamente. Si no está conectado, se puede subir el archivo `.html` manualmente en Vercel → Deployments → arrastrar archivo.

## Stack

HTML + CSS + JavaScript vanilla, sin frameworks ni build tools. Librerías vía CDN (qrcodejs, jsQR, html2canvas). Persistencia vía Google Forms/Sheets (invitación) y `localStorage` (control de acceso). Ver detalles técnicos completos en `docs/PROMPT_CONTEXTO_TECNICO_SIZIGIA.md`.
