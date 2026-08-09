# 🍽️ Consumición

Sub-tab de [[02 - Operaciones]] (botón "🍴 Consumicion", `ot==='consumicion'`). Visible para todos los roles.

**Función:** Registrar cargos (comidas, bebidas, servicios) a habitaciones con check-in activo

## Conexiones
- ← [[Habitaciones]] — solo muestra habitaciones con check-in activo
- → [[03 - Caja y Cobros]] — los cargos suman al saldo del huésped
- → [[05 - Clientes]] — queda en historial del huésped
- ← [[Inventario (FB)]] — descuenta stock de productos al cargar consumo

## Flujo
```
Seleccionar habitación activa
    ↓
Agregar ítem (producto de Inventario o servicio manual)
    ↓
Cargo queda pendiente en la habitación
    ↓
Se cobra al hacer checkout en [[03 - Caja y Cobros]]
```

## Estado
- 🟡 Revisar que cargos se asocien correctamente al check-in activo
- 🟡 Revisar descuento de inventario
- 🟡 Revisar responsable automático (debe tomar usuario logueado)

## Bugs conocidos
- Ninguno registrado aún — pendiente revisión

## Código
- Función activa: `renderConsOperaciones()`, cargo con `doCharge()`
- **Ojo — código muerto:** `renderCons()` (línea ~4121) y `renderConsVentas()`
  (línea ~4160) existen en `index.html` pero no se llaman desde ningún lado
  (`renderOperaciones()` no las invoca). Son remanentes de una versión
  anterior donde Consumición e Inventario compartían un solo tab. No confundir
  con las funciones activas de arriba.
- Archivo: `index.html`
