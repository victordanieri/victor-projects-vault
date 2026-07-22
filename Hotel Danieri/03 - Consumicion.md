# 🍽️ Consumición

**Función:** Registrar cargos (comidas, bebidas, servicios) a habitaciones con check-in activo

## Conexiones
- ← [[02 - Habitaciones]] — solo muestra habitaciones con check-in activo
- → [[05 - Caja y Cobros]] — los cargos suman al saldo del huésped
- ← [[04 - Inventario]] — descuenta stock al registrar consumo
- → [[07 - Clientes]] — queda en historial del huésped

## Flujo
```
Seleccionar habitación activa
    ↓
Agregar ítem (producto del inventario o servicio manual)
    ↓
Cargo queda pendiente en la habitación
    ↓
Se cobra al hacer checkout en [[05 - Caja y Cobros]]
```

## Estado
- 🟡 Revisar que cargos se asocien correctamente al check-in activo
- 🟡 Revisar descuento de inventario
- 🟡 Revisar responsable automático (debe tomar usuario logueado)

## Bugs conocidos
- Ninguno registrado aún — pendiente revisión

## Código
- Función: `renderConsumicion()`, `doAddCharge()`
- Archivo: `index.html` línea ~1400
