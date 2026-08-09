# 🧺 Blanquería

**Tab real en el menú:** sub-tab propio "🧺 Blanqueria" (`ot==='blanqueria'`,
solo visible para admin/superadmin) dentro del tab **Operaciones**. Es
DISTINTO del sub-tab "📦 Inventario" (ver nota abajo).

**Función:** Control de stock de bienes de uso del hotel — sábanas, toallas,
vajilla, muebles, equipos/electrodomésticos, amenities — con categorías,
ubicaciones, compras y ajuste manual de stock.

## Corrección: hay 3 sistemas de stock separados en Operaciones, no uno
1. **Inventario** (sub-tab "📦 Inventario", admin) — `renderConsInventario()`
   — stock de productos de comida/bebida que se cargan a habitaciones vía
   [[03 - Consumicion]]. Documentado en ese archivo.
2. **Blanquería** (este archivo) — `renderBlanq()` — sábanas, muebles,
   equipos, amenities. Independiente de Consumición.
3. **Cocina** (sub-tab "🍳 Cocina", admin) — `renderCocina()` — stock propio
   de insumos de cocina (lácteos, panadería, condimentos, etc., `DB.cocina`),
   también independiente. Sin nota propia en el vault por ahora.

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
