# 🏨 Hotel Danieri — Overview

**Sistema:** Hotel Manager v5  
**URL live:** https://victordanieri.github.io/hotel-danieri  
**Repo GitHub:** https://github.com/victordanieri/hotel-danieri  
**Base de datos:** Supabase (danieri_hotel_v5)  
**Stack:** Single-file HTML + Vanilla JS + Supabase + GitHub Pages  

---

## Módulos del sistema

### Operativo
- [[01 - Dashboard]] — Resumen del día, ocupación, alertas
- [[02 - Habitaciones]] — Mapa de habitaciones, estados, limpieza
- [[03 - Consumicion]] — Cargos a habitaciones activas
- [[04 - Inventario]] — Stock de productos y movimientos
- [[05 - Caja y Cobros]] — Apertura/cierre de caja, cobros, pagos

### Gestión
- [[06 - Finanzas]] — Reportes financieros, ingresos por período
- [[07 - Clientes]] — Base de datos de huéspedes, historial
- [[08 - Canales OTA]] — Booking, Expedia, Local, sincronización
- [[09 - Auditoria]] — Log de todas las operaciones del sistema

### Configuración
- [[10 - Config]] — Hotel, monedas, habitaciones, usuarios, datos

---

## Flujo principal de una reserva

```
[[08 - Canales OTA]] → Reserva ingresa
        ↓
[[01 - Dashboard]] → Aparece en "Reservas próximas"
        ↓
[[02 - Habitaciones]] → Check-in confirma ocupación
        ↓
[[03 - Consumicion]] → Cargos durante estadía
        ↓
[[05 - Caja y Cobros]] → Cobro al hacer checkout
        ↓
[[07 - Clientes]] → Historial del huésped actualizado
        ↓
[[02 - Habitaciones]] → Habitación pasa a "Pendiente aseo"
        ↓
[[01 - Dashboard]] → Mucama marca como limpia → Libre
```

---

## Estado general
- 🟢 Sistema online y funcionando
- 🟢 Supabase como fuente de verdad
- 🟢 GitHub API conectada (Claude escribe directamente)
- 🟡 Integraciones entre módulos — pendiente revisión
- 🔴 Extensión Chrome Booking — pendiente
- 🔴 Dominio propio — pendiente
