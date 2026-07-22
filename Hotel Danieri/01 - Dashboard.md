# 📊 Dashboard

**Función:** Resumen del día — ocupación, alertas, check-ins activos, reservas próximas

## Conexiones
- ← [[08 - Canales OTA]] — trae reservas del día
- → [[02 - Habitaciones]] — muestra mapa de ocupación
- → [[05 - Caja y Cobros]] — muestra cobrado hoy y pendiente
- ↔ [[03 - Consumicion]] — muestra cargos pendientes

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
- Función: `renderDashboard()`
- Archivo: `index.html` línea ~700
