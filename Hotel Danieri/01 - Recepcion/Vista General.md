# 📊 Vista General

Sub-tab de [[01 - Recepcion]] (botón "Vista general", `rt==='resumen'`). El
código interno la llama "Dashboard" (`renderDash()`), pero ese nombre no
aparece en la UI.

**Función:** Resumen del día — ocupación, alertas, check-ins activos, reservas próximas

## Conexiones
- ← [[06 - Canales OTA]] — trae reservas del día
- → [[Habitaciones]] — muestra mapa de ocupación
- → [[03 - Caja y Cobros]] — muestra cobrado hoy y pendiente
- ↔ [[02 - Operaciones]] — muestra cargos pendientes

## Indicadores que muestra
- Ocupadas / Total habitaciones
- Check-outs hoy
- Check-ins hoy
- Libres
- Pendiente de aseo (mucamas)
- Cobrado hoy (Gs.)
- Pendiente de cobro (Gs.)
- Calendario de ocupación (vista mensual)
- Check-ins activos con detalle
- Reservas próximas (7/15/30 días)
- Tabla de limpieza pendiente con asignación de mucamas

## Estado
- 🟢 Funciona
- 🟡 Revisar integración con Caja (cobrado hoy)
- 🟡 Revisar tabla mucamas (módulo nuevo)

## Bugs conocidos
- Ninguno registrado aún

## Código
- Función: `renderDash()`, llamada desde `renderRecepcion()` cuando la sub-tab activa es "resumen"
- Archivo: `index.html`
