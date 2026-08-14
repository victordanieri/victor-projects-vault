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
**Último commit:** 0dd1d086 — fix totalRoomUSD usa finalUSD para reservas locales lh-sync

### Qué se hizo en la última sesión
1. Deploy correcto — worker.js de GitHub reemplazó versión manual vieja de Dani
2. Cobro Pierre Santos corregido — pago único Gs. 25.000 Tarjeta a nombre de Isolina Riquelme
3. Cargos pendientes en Caja & Cobros — eliminado botón "Cobrar" por línea; ahora es solo vista
4. Bug cobro grupal con descuento — saldoGsEquiv y vuelto suman Gs. de todas las habs del grupo y restan descuento; fallback con getGrupoCheckin() si _ciGrupalActivo se resetea
5. Cancelación grupal lh-sync — cancela todas las reservas activas del mismo huésped/fecha/canal al recibir email de cancelación Booking
6. Reservas locales lh-sync — detecta canal === 'Local' y asigna precios fijos desde Config/Habitaciones
7. Reservas Claudio Sotomayor — 3 canceladas vía Supabase (Hab. 14, 22, 23 Booking); Hab. 22 Local quedó activa
8. Fix totalRoomUSD en lh-sync — reservas locales ahora graban finalUSD correcto (antes siempre quedaba 0)
9. Comandos /inicio hotel y /fin hotel configurados en memoria y OVERVIEW

### Pendientes para la próxima sesión
- [ ] Obligar selección método de pago antes de ingresar monto — modales check-in, check-out y deuda (identificado, no implementado)
- [ ] Precio USD reserva Claudio Hab. 22 — corregir manual como SA desde Editar datos
- [ ] Precio USD reserva Luis Santacruz Hab. 13 — corregir manual como SA desde Editar datos

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
