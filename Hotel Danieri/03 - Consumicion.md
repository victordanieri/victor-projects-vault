# 🍽️ Consumición

**Función:** Registrar cargos (comidas, bebidas, servicios) a habitaciones con check-in activo

## Conexiones
- ← [[02 - Habitaciones]] — solo muestra habitaciones con check-in activo
- → [[05 - Caja y Cobros]] — los cargos suman al saldo del huésped
- → [[07 - Clientes]] — queda en historial del huésped

**Nota:** el stock de productos de comida/bebida vive DENTRO de este módulo
(sub-tab propio), no en [[04 - Inventario]] — ese módulo es en realidad
Blanquería/bienes de uso (sábanas, muebles, equipos), un inventario distinto.

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
- Función: `renderCons()` (contenedor) con sub-tabs `renderConsOperaciones()`,
  `renderConsVentas()`, `renderConsInventario()` (stock de productos propio),
  `renderConsImprimir()`; cargo con `doCharge()`
- Archivo: `index.html`
