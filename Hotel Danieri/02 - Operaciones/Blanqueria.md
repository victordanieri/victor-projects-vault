# 🧺 Blanquería

Sub-tab de [[02 - Operaciones]] (botón "🧺 Blanqueria", `ot==='blanqueria'`). Solo admin/superadmin.

**Función:** Control de stock de bienes de uso del hotel — sábanas, toallas,
vajilla, muebles, equipos/electrodomésticos, amenities — con categorías,
ubicaciones, compras y ajuste manual de stock. Sistema de stock independiente
de [[Inventario (FB)]] y de [[Cocina]].

## Categorías (`DB.invCategorias`)
Blanqueria, Camas y colchones, Vajilleria, Equipos y electrodomesticos,
Amenities, Muebles, Otros

## Funcionalidad
- Ajuste de stock manual (solo superadmin)
- Registro de compras
- Sort persistente de la tabla

## Estado
- 🟢 Funciona

## Código
- Función: `renderBlanq()` (contenedor, llamado desde `renderOperaciones()`
  vía `renderBlanqContent()`) con `renderBlanqStock()`, `renderBlanqCompras()`,
  `renderBlanqImprimir()`, `renderBlanqUbicaciones()`
- Archivo: `index.html`
