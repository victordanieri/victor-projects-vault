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
**Último commit:** d625d29 — feat(ux): badge Expedia Collect en modal de reserva con instrucción IVR  
**Sesión cerrada:** 2026-08-18 PY

### Qué se hizo en la última sesión
1. **SA puede reasignar caja desde modal editar pago** — en Historial de Pagos, el lápiz de edición ahora incluye sección "SUPERADMIN — Reasignar caja" con dropdown de todas las cajas (N°, cajero, abierta/cerrada), caja actual pre-seleccionada.
2. **Fix Expedia Collect — monto correcto en lh-sync** — el Apps Script (`lh_gmail_sync.gs`) ahora detecta `"Expedia Collect"` en el email y captura `Remittance amount` (neto) como campo separado `remittanceUSD`. El Worker usa ese valor como `totalRoomUSD` en lugar del bruto `Total Price`. También corregido bug donde `totalRoomUSD` y `amountUSD` del cargo usaban `totalUSD` en vez de `finalUSD`.
3. **Badge visual Expedia Collect en modal de reserva** — cuando `paymentModel === 'expedia_collect'`, el modal muestra un aviso violeta: "EXPEDIA COLLECT — TARJETA VIRTUAL / No cobrar al huésped / Cargar en terminal IVR la tarjeta virtual de Expedia por US$ X.XX al check-in."
4. **Reserva Jorge Narita corregida manualmente** — `totalRoomUSD` editado a `145.86` (remittance real). Las próximas Expedia Collect entran correctas automáticamente.

### Pendientes para la próxima sesión
- Verificar si hay otras reservas turquesas (manuales) de Expedia Collect pre-fix que necesiten corrección manual de `totalRoomUSD` — revisar Historial estadías filtrando por origen Expedia
- Verificar comportamiento al cancelar reservas manuales amarillas (si se eliminan del calendario correctamente)
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


