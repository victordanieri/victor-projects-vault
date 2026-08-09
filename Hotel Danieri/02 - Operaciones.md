# 🍽️ Operaciones

**Tab principal del menú** (`navItems()` id `operaciones`). Roles: superadmin, admin, recepcionista (algunos sub-tabs solo admin).

## Sub-tabs (`renderOperaciones()`, index.html:3893) — todos hermanos, ninguno anidado en otro
- [[Consumicion]] — cargos a habitaciones (todos los roles)
- [[Inventario (FB)]] — stock de comida/bebida (solo admin)
- [[Blanqueria]] — stock de bienes de uso: sábanas, muebles, equipos (solo admin)
- [[Cocina]] — stock propio de insumos de cocina (solo admin)
- Desayuno, Precios, Imprimir — ver [[Otros Sub-tabs]]

## Conexiones
- ← [[01 - Recepcion]] — check-in activo habilita cargos
- → [[03 - Caja y Cobros]] — cargos suman al saldo
- → [[07 - Auditoria]] — log de movimientos
- ← [[08 - Config]] — monedas (USD/Gs.), tasas, productos

## Estado
- 🟢 Funciona

## Código
- Función: `renderOperaciones()` (contenedor)
- Archivo: `index.html`
