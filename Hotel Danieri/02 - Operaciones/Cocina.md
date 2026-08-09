# 🍳 Cocina

Sub-tab de [[02 - Operaciones]] (botón "🍳 Cocina", `ot==='cocina'`). Solo admin/superadmin.

**Función:** Stock propio de insumos de cocina (`DB.cocina`), independiente
del inventario de [[Consumicion]] y de [[Blanqueria]].

## Categorías (`DB.cocina.categorias`)
Lacteos, Panaderia, Bebidas, Condimentos, Frutas, Enlatados, Otros

## Sub-vistas internas
- Stock actual (con alerta de stock bajo)
- Movimientos

## Estado
- 🟢 Funciona

## Código
- Función: `renderCocina()`
- Archivo: `index.html`
