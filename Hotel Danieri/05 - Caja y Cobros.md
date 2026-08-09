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
- Responsable = usuario logueado automáticamente (no editable por recepcionista, salvo admin en cobro manual)
- Solo USD y Gs. (BRL/ARS eliminados del sistema)

## Cobros (actualizado — rework completo 05-07/08)
- Vuelto automático en tiempo real según método de pago (con tolerancia de punto flotante en USD)
- Tablas de pagos con columnas separadas Gs. / USD (ya no una columna "equivalente")
- Redondeo de Gs. siempre hacia arriba al millar (`roundGs` usa `ceil`)
- Resumen de checkout desglosado en Alojamiento / Consumición / Servicios, cada uno con columna Origen, USD y Gs.
- Alojamiento en el resumen solo muestra estado Pagado/Pendiente, sin montos (evita confusión con los otros rubros)

## Estado
- 🟢 Cobros y vuelto reescritos y estables (rework 05-07/08)
- 🟡 Revisar responsable automático en cobros manuales

## Código
- Función: `renderCaja()`, `mManualPay()` / `doManualPay()`, `doCheckout()`, `renderCajaChica()`
- Archivo: `index.html`
