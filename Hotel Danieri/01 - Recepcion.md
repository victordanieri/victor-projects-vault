# 🏨 Recepción

**Tab principal del menú** (`navItems()` id `recepcion`). Roles: superadmin, admin, recepcionista.

## Sub-tabs (`renderRecepcion()`, index.html:1234)
- [[Vista General]] — resumen del día, ocupación, alertas
- [[Habitaciones]] — mapa visual, check-in/checkout, limpieza
- Historial estadías — listado de check-outs pasados, solo admin/superadmin (`renderHistorialEstadias()`, sin nota propia por ser solo un listado de solo lectura)

## Conexiones
- ← [[06 - Canales OTA]] — trae reservas del día
- → [[03 - Caja y Cobros]] — checkout genera cobro
- → [[02 - Operaciones]] — check-in activo habilita cargos
- → [[05 - Clientes]] — check-in registra/actualiza huésped

## Estado
- 🟢 Funciona

## Código
- Función: `renderRecepcion()` (contenedor)
- Archivo: `index.html`
