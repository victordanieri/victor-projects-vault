# 💰 Caja y Cobros

**Función:** Apertura/cierre de caja, cobros de estadías, pagos manuales

## Conexiones
- ← [[02 - Habitaciones]] — checkout genera cobro
- ← [[03 - Consumicion]] — cargos pendientes se cobran acá
- → [[06 - Finanzas]] — movimientos van al reporte financiero
- → [[01 - Dashboard]] — muestra cobrado hoy y pendiente

## Reglas de negocio
- Solo puede haber UNA caja abierta por recepcionista a la vez
- No puede abrir caja otro recepcionista si hay una abierta
- Responsable = usuario logueado automáticamente (no editable por recepcionista)

## Estado
- 🟡 Revisar integración cobros con checkout
- 🟡 Revisar responsable automático en cobros manuales

## Código
- Función: `renderCaja()`, `doManualPay()`, `doCheckout()`
- Archivo: `index.html`
