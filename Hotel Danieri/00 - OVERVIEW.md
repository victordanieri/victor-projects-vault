# 🏨 Hotel Danieri — Overview

**Sistema:** Hotel Manager v5  
**URL live:** https://hotel-danieri.victordanieri.workers.dev  
**Repo GitHub:** https://github.com/victordanieri/hotel-danieri (privado, git solo local — no hay push automático)  
**Base de datos:** Supabase Postgres — tabla `hotel_data`, fila única `danieri_hotel_v5` (blob JSONB)  
**Stack:** `index.html` (single-file, front-end) → `build_worker.py` genera `worker.js` → Cloudflare Worker (backend + servido) → GitHub Actions (`deploy.yml`) hace `wrangler deploy` en cada push a `main`  

> ✅ **Última sincronización Obsidian ↔ código** — 09/08/2026 (commit `4b8a302`)

## ⚡ ESTADO DE SESIÓN — leer esto primero

> Este bloque lo actualiza Claude al final de cada sesión. No editar manualmente.

**Última sesión:** 2026-08-18  
**Último commit:** 2b222ee — fix(checkout): TOTAL extras y saldo pendiente en USD cuando corresponde  
**Sesión cerrada:** 2026-08-18 PY

### Qué se hizo en la última sesión
1. **Análisis cancelación reserva manual (Alberto Hab. 11)** — verificado que lh-sync no puede cancelar reservas cargadas manualmente (sin `lhId`). Decisión: se cancela manualmente. No se implementó fix automático.
2. **Fix checkout — servicios adicionales pagados en USD** — el modal de checkout ahora muestra correctamente el monto en USD en: (a) ítem individual del servicio, (b) header "PAGADO —", (c) fila "✚ Servicios adicionales" del resumen inferior, (d) fila "TOTAL extras" con USD pagado + Gs. pendiente, (e) "Pago parcial recibido" en USD.
3. **Fix `doCobrarHab` — guardar `cargoId` en payments** — al cobrar deuda de habitación con un solo cargo pendiente, el payment ahora incluye `cargoId` para vincular correctamente. En pago parcial multi-cargo, se crea un payment por cargo con su `cargoId`. Retrocompatibilidad: fallback por `concept.indexOf('deuda')` para payments anteriores al fix.

### Pendientes para la próxima sesión
- Verificar si hay otras reservas turquesas (Expedia Collect) pre-fix que necesiten corrección manual de `totalRoomUSD` — revisar Historial estadías filtrando por origen Expedia
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


