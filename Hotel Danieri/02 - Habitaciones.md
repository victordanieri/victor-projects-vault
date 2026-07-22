# 🛏️ Habitaciones

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

## Estado
- 🟢 Mapa de habitaciones funciona
- 🟢 Check-in funciona
- 🟢 Módulo mucamas implementado (nuevo)
- 🟡 Revisar flujo checkout → estado habitación
- 🟡 Revisar responsable automático en check-in

## Bugs conocidos
- Ninguno registrado aún

## Código
- Función: `renderHabitaciones()`, `mCheckin()`, `doCheckin()`, `mAsignarLimpieza()`
- Archivo: `index.html` línea ~950
