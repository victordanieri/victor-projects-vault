# 🏨 Hotel Danieri — Overview

**Sistema:** Hotel Manager v5  
**URL live:** https://hotel-danieri.victordanieri.workers.dev  
**Repo GitHub:** https://github.com/victordanieri/hotel-danieri (privado, git solo local — no hay push automático)  
**Base de datos:** Supabase Postgres — tabla `hotel_data`, fila única `danieri_hotel_v5` (blob JSONB)  
**Stack:** `index.html` (single-file, front-end) → `build_worker.py` genera `worker.js` → Cloudflare Worker (backend + servido) → GitHub Actions (`deploy.yml`) hace `wrangler deploy` en cada push a `main`  

> ✅ **Última sincronización Obsidian ↔ código** — 09/08/2026 (commit `4b8a302`)

## ⚡ ESTADO DE SESIÓN — leer esto primero

> Este bloque lo actualiza Claude al final de cada sesión. No editar manualmente.

**Última sesión:** 2026-08-14  
**Último commit:** 7393440 — build: worker.js + rediseño reportes

### Qué se hizo en la última sesión
1. Bug USD=0 en reservas locales vía lh-sync — corregido en sesión anterior; pendiente verificar próxima reserva entrante
2. Expedia Hotel Collect / Expedia Collect — identificadas, implementación pendiente
3. Rediseño visual reporte "Resumen por día" — estilo moderno, sin bordes Excel
4. Reporte "Historial de cajas" — agregado desglose línea a línea por pago para punteo con impreso de recepcionista
5. Modal "Historial de Pagos" — ancho ampliado a 1400px
6. Reporte impreso "Historial de Pagos" — rediseño moderno + totales por columna + total general USD y Gs.

### Pendientes para la próxima sesión
- Verificar que próxima reserva local vía lh-sync trae totalRoomUSD correcto
- Implementar soporte Expedia Hotel Collect y Expedia Collect en el sistema

---

## 🚨 REGLA DE DEPLOY — NUNCA pedir deploy manual a Dani

**El deploy es 100% automático.** Flujo correcto siempre:
1. Claude modifica `index.html`
2. Claude pushea `index.html` a GitHub → GitHub Actions corre `deploy.yml` automáticamente → Cloudflare actualizado
3. Claude verifica el resultado del workflow run via API de GitHub antes de dar el cambio por deployado

**Claude NUNCA debe:**
- Pedirle a Dani que corra `wrangler deploy` manualmente
- Pedirle que haga `git pull` o cualquier comando en su PC
- Asumir que el deploy falló sin verificar el workflow run primero

**Por qué:** Claude no tiene acceso a `api.cloudflare.com` desde su entorno, pero GitHub Actions sí. El deploy manual desde la PC de Dani es el camino que históricamente rompió el sistema (worker.js local sobreescribía versiones de GitHub). Siempre usar GitHub Actions.

---

## Seguridad (capa server-side, no existía en la versión anterior)
- Login server-side en el Worker (`/api/login`), hash `pbkdf2v2` (PBKDF2 8000 iteraciones, ajustado al presupuesto de CPU gratis de Cloudflare)
- Rate limiting real de intentos de login vía KV (`LOGIN_ATTEMPTS`)
- Autorización de escritura por rol en el servidor (no solo en el front)
- Bloqueo de concurrencia optimista: si dos sesiones editan el mismo blob, la que quedó desactualizada recibe un aviso (HTTP 409) y debe refrescar antes de guardar — simétrico para todos los roles, incluido superadmin
- Detalle completo de esta capa: ver memoria `project-hotel-danieri-security` (no está en el vault, es infraestructura de backend, no un módulo funcional)

---

## Navegación real del sistema (8 tabs, `navItems()` en index.html:935)

Los 8 módulos principales del vault ahora son 1:1 con los 8 tabs reales del
menú. Los que tienen sub-tabs son carpetas con un archivo índice (`0N - Nombre.md`)
más sus divisiones adentro:

1. [[01 - Recepcion]] (carpeta) → [[Vista General]], [[Habitaciones]], Historial estadías
2. [[02 - Operaciones]] (carpeta) → [[Consumicion]], [[Inventario (FB)]], [[Blanqueria]], [[Cocina]], [[Otros Sub-tabs]]
3. [[03 - Caja y Cobros]] — tab propio, sin sub-tabs
4. [[04 - Finanzas]] — tab propio
5. [[05 - Clientes]] — tab propio
6. [[06 - Canales OTA]] — tab propio
7. [[07 - Auditoria]] — tab propio
8. [[08 - Config]] — tab propio

---

## Flujo principal de una reserva

```
[[06 - Canales OTA]] → Reserva ingresa
        ↓
[[Vista General]] → Aparece en "Reservas próximas"
        ↓
[[Habitaciones]] → Check-in confirma ocupación
        ↓
[[Consumicion]] → Cargos durante estadía
        ↓
[[03 - Caja y Cobros]] → Cobro al hacer checkout
        ↓
[[05 - Clientes]] → Historial del huésped actualizado
        ↓
[[Habitaciones]] → Habitación pasa a "Pendiente aseo"
        ↓
[[Vista General]] → Mucama marca como limpia → Libre
```

---

## Estado general
- 🟢 Sistema online y funcionando (Cloudflare Worker)
- 🟢 Supabase como fuente de verdad
- 🟢 Login server-side + roles + rate limit + concurrencia optimista (409) — 5 niveles de hardening completados
- 🟢 Moneda simplificada a solo USD + Gs. (BRL/ARS eliminados)
- 🟡 Integraciones entre módulos — pendiente revisión
- 🔴 Extensión Chrome Booking — pendiente
- 🔴 Dominio propio — pendiente
- 🟢 Sincronización automática Obsidian ↔ código — activa (tarea diaria 10am, ver abajo)

## Sync automático de este vault
Tarea programada `hotel-danieri-vault-sync`, corre todos los días a las 10am
(hora Paraguay) mientras la app esté abierta. Revisa los commits nuevos desde
el hash de referencia de abajo y actualiza solo los archivos afectados.
Última fecha de referencia usada por la rutina: commit `4b8a302` (2026-08-07).
Si necesitás forzar un sync fuera de horario, pedilo directamente.



