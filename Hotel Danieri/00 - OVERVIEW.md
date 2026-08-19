# 🏨 Hotel Danieri — Overview

**Sistema:** Hotel Manager v5  
**URL live:** https://hotel-danieri.victordanieri.workers.dev  
**Repo GitHub:** https://github.com/victordanieri/hotel-danieri (privado, git solo local — no hay push automático)  
**Base de datos:** Supabase Postgres — tabla `hotel_data`, fila única `danieri_hotel_v5` (blob JSONB)  
**Stack:** `index.html` (single-file, front-end) → `build_worker.py` genera `worker.js` → Cloudflare Worker (backend + servido) → GitHub Actions (`deploy.yml`) hace `wrangler deploy` en cada push a `main`  

> ✅ **Última sincronización Obsidian ↔ código** — 09/08/2026 (commit `4b8a302`)

## ⚡ ESTADO DE SESIÓN — leer esto primero

> Este bloque lo actualiza Claude al final de cada sesión. No editar manualmente.

**Última sesión:** 2026-08-19  
**Último commit:** 5463830 — fix(reserva): campos anticipo readonly para no-SA cuando ya hay anticipo registrado  
**Sesión cerrada:** 2026-08-19 PY

### Qué se hizo en la última sesión
1. **Fix consumición duplicada** — `saDeletePayment` (botón ✕ en Caja & Cobros) ahora anula la orden en `DB.consumicion.orders` y repone stock automáticamente cuando el pago es de consumición (`isConsumicion:true`). Payment de Lobby ahora guarda `orderId` para el linkeo.
2. **Botón SA anular orden directo** — nuevo botón ✕ en la tabla "Órdenes de hoy" (Operaciones → Consumición) para anular órdenes huérfanas sin tocar el stock (casos donde el pago ya fue eliminado manualmente).
3. **Botón SA editar responsable de caja** — nuevo botón "✎ Editar caja" en el header verde de Caja & Cobros. Permite cambiar `abiertaPor` de la caja activa y actualiza `staffResp` de todos sus pagos. Fix aplicado para leer `DB.cajaActual` directamente (no la copia en `DB.cajas`).
4. **Fix color calendario reservas con anticipo posterior** — `ciStatus()` ahora detecta `ci.paidInAdvance=true` y retorna `res-paid` directamente. Además, `obs-warn` (turquesa con `!important`) ya no pisa el color de reservas `res-paid`.
5. **Fix campos anticipo readonly** — en editar reserva, si ya hay anticipo registrado, los campos de monto y método quedan `readonly`/`disabled` para recepcionistas y admins. Solo SA puede modificarlos (con etiqueta "(SA)" en amarillo).

### Pendientes para la próxima sesión
- Verificar si hay otras reservas Expedia Collect pre-fix que necesiten corrección manual de `totalRoomUSD` — revisar Historial estadías filtrando por origen Expedia
- Verificar que próxima reserva local vía lh-sync trae `totalRoomUSD` correcto
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

---

## Navegación real del sistema (8 tabs, `navItems()` en index.html:935)

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
- 🟢 Login server-side + roles + rate limit + concurrencia optimista (409)
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


