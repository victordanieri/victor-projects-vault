# 📦 Inventario (comida/bebida)

Sub-tab de [[02 - Operaciones]] (botón "📦 Inventario", `ot==='inventario'`). Solo admin/superadmin.

**Función:** Stock, costos, márgenes y compras de los productos de comida/bebida
que se cargan a las habitaciones desde [[Consumicion]]. Sistema de stock
independiente de [[Blanqueria]] y de [[Cocina]].

## Contenido
- Tabla de stock con columnas ordenables: Producto, Categoría, Stock, Precio
  venta, Último costo (editable por superadmin), Costo promedio, Markup %
- Registrar compra / Salida-Baja de stock
- Editar productos
- Historial de compras con cálculo de ROI por compra

## Conexiones
- → [[Consumicion]] — descuenta stock al cargar un consumo

## Estado
- 🟢 Funciona

## Código
- Función: `renderConsInventario()` (única función activa — también existe
  huérfana dentro de `renderCons()`, código muerto, ver nota en [[Consumicion]])
- Archivo: `index.html`
