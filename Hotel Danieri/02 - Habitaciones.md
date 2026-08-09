# 🛏️ Habitaciones

**Tab real en el menú:** no es un tab propio — es la sub-vista "Habitaciones"
dentro del tab **Recepción** (junto con [[01 - Dashboard]] e Historial de
estadías).

**Función:** Mapa visual de habitaciones, estados, check-in/checkout, limpieza

## Conexiones
- → [[01 - Dashboard]] — actualiza ocupación y pendiente de aseo
- → [[03 - Consumicion]] — al hacer check-in habilita cargos
- → [[05 - Caja y Cobros]] — checkout genera cobro
- → [[07 - Clientes]] — check-in registra/actualiza huésped
- ← [[08 - Canales OTA]] — reservas confirmadas aparecen acá

## Estados de habitación
- ⬜ **Libre** — disponible para venta
- 🔵 **Reserva** — reservada sin check-in
- 🟠 **Check-in con deuda** — ocupada, saldo pendiente
- 🟢 **Check-in pagado** — ocupada, al día
- ⬛ **Check-out** — proceso de salida
- 🔴 **Cerrada** — fuera de servicio

## Flujo de limpieza (Mucamas)
```
Checkout → dirty=true → "Pendiente aseo"
    ↓
Asignar mucama (usuario sistema o nombre libre)
    ↓
Marcar como limpia → dirty=false → cleaningLog actualizado
    ↓
Habitación vuelve a "Libre"
```

## Check-in (actualizado)
- Precio dual USD/Gs por habitación, con tasa de cambio compra/venta configurable
- Vuelto calculado automáticamente en tiempo real según método de pago
- Descuentos requieren PIN de admin
- Anticipo (si existe reserva con seña) se precarga automáticamente
- Superadmin puede editar el nombre del huésped ya cargado en la reserva
- Sin campo "Unidad" — deuda siempre expresada en USD con equivalente en Gs.

## Estado
- 🟢 Mapa de habitaciones funciona
- 🟢 Check-in funciona (con precio dual y vuelto automático)
- 🟢 Módulo mucamas implementado
- 🟡 Revisar flujo checkout → estado habitación
- 🟡 Revisar responsable automático en check-in

## Bugs conocidos
- Ninguno registrado aún

## Código
- Función: `renderHabs()` (llamada desde `renderRecepcion()`), `mCheckin()`, `doCheckin()`, `mAsignarLimpieza()`
- Archivo: `index.html`
