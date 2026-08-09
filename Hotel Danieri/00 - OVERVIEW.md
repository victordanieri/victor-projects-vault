# 🏨 Hotel Danieri — Overview

**Sistema:** Hotel Manager v5  
**URL live:** https://hotel-danieri.victordanieri.workers.dev  
**Repo GitHub:** https://github.com/victordanieri/hotel-danieri (privado, git solo local — no hay push automático)  
**Base de datos:** Supabase Postgres — tabla `hotel_data`, fila única `danieri_hotel_v5` (blob JSONB)  
**Stack:** `index.html` (single-file, front-end) → `build_worker.py` genera `worker.js` → Cloudflare Worker (backend + servido) → GitHub Actions (`deploy.yml`) hace `wrangler deploy` en cada push a `main`  

> ✅ **Última sincronización Obsidian ↔ código** — 09/08/2026 (commit `4b8a302`)

## Seguridad (capa server-side, no existía en la versión anterior)
- Login server-side en el Worker (`/api/login`), hash `pbkdf2v2` (PBKDF2 8000 iteraciones, ajustado al presupuesto de CPU gratis de Cloudflare)
- Rate limiting real de intentos de login vía KV (`LOGIN_ATTEMPTS`)
- Autorización de escritura por rol en el servidor (no solo en el front)
- Bloqueo de concurrencia optimista: si dos sesiones editan el mismo blob, la que quedó desactualizada recibe un aviso (HTTP 409) y debe refrescar antes de guardar — simétrico para todos los roles, incluido superadmin
- Detalle completo de esta capa: ver memoria `project-hotel-danieri-security` (no está en el vault, es infraestructura de backend, no un módulo funcional)

---

## Módulos del sistema

### Operativo
- [[01 - Dashboard]] — Resumen del día, ocupación, alertas
- [[02 - Habitaciones]] — Mapa de habitaciones, estados, limpieza
- [[03 - Consumicion]] — Cargos a habitaciones activas
- [[04 - Inventario]] — Stock de productos y movimientos
- [[05 - Caja y Cobros]] — Apertura/cierre de caja, cobros, pagos

### Gestión
- [[06 - Finanzas]] — Reportes financieros, ingresos por período
- [[07 - Clientes]] — Base de datos de huéspedes, historial
- [[08 - Canales OTA]] — Booking, Expedia, Local, sincronización
- [[09 - Auditoria]] — Log de todas las operaciones del sistema

### Configuración
- [[10 - Config]] — Hotel, monedas, habitaciones, usuarios, datos

---

## Flujo principal de una reserva

```
[[08 - Canales OTA]] → Reserva ingresa
        ↓
[[01 - Dashboard]] → Aparece en "Reservas próximas"
        ↓
[[02 - Habitaciones]] → Check-in confirma ocupación
        ↓
[[03 - Consumicion]] → Cargos durante estadía
        ↓
[[05 - Caja y Cobros]] → Cobro al hacer checkout
        ↓
[[07 - Clientes]] → Historial del huésped actualizado
        ↓
[[02 - Habitaciones]] → Habitación pasa a "Pendiente aseo"
        ↓
[[01 - Dashboard]] → Mucama marca como limpia → Libre
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
- 🔴 Sincronización automática Obsidian ↔ código — pendiente de implementar (ver abajo)

## Sync automático de este vault
Este vault se actualiza automáticamente 1 vez por día (rutina programada) leyendo
los commits nuevos en `hotel-danieri` desde la última sincronización y
actualizando solo los módulos afectados. Última fecha de referencia usada por
la rutina: commit `4b8a302` (2026-08-07). Si necesitás forzar un sync fuera de
horario, pedilo directamente.
