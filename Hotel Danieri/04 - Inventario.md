# 📦 Inventario (Blanquería / Bienes de uso)

**Tab real en el menú:** no es un tab propio — es una sub-vista dentro del tab
**Operaciones** (junto con [[03 - Consumicion]], Desayuno, Cocina, Precios e
Imprimir).

**Función:** Control de stock de bienes de uso del hotel — sábanas, toallas,
vajilla, muebles, equipos/electrodomésticos, amenities — con categorías,
ubicaciones, compras y ajuste manual de stock.

**Corrección importante:** este módulo NO es el stock de comida/bebida.
El inventario de productos consumibles (F&B) vive dentro de
[[03 - Consumicion]] como sub-tab propio (`renderConsInventario()`).
Este módulo es "Blanquería" internamente en el código.

## Conexiones
- → [[01 - Dashboard]] — alerta stock bajo

## Categorías (`DB.invCategorias`)
Blanquería, Camas y colchones, Vajillería, Equipos y electrodomésticos,
Amenities, Muebles, Otros

## Funcionalidad
- Ajuste de stock manual (solo superadmin)
- Registro de compras
- Sort persistente de la tabla

## Estado
- 🟢 Funciona

## Código
- Función: `renderBlanq()` (contenedor) con `renderBlanqStock()`,
  `renderBlanqCompras()`, `renderBlanqImprimir()`, `renderBlanqUbicaciones()`
- Archivo: `index.html`
