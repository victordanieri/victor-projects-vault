# ⚙️ Config

**Función:** Configuración general del sistema

## Secciones
- **Hotel** — nombre, dirección, datos generales
- **Monedas** — PYG, USD, BRL y tipos de cambio
- **Habitaciones** — agregar/editar habitaciones y tipos
- **Servicios** — lista de servicios cobrados
- **Origenes & Staff** — canales de reserva (staff viene de Usuarios)
- **Usuarios** — gestión de usuarios y contraseñas (solo Superadmin)
- **Datos** — exportar/importar JSON, sincronizar Supabase

## Reglas de seguridad
- Contraseñas hasheadas con SHA-256 (Web Crypto API)
- Solo Superadmin puede cambiar contraseñas (botón 🔑 Clave)
- Staff/responsables se toman automáticamente de Usuarios activos

## Estado
- 🟢 Funciona correctamente

## Código
- Función: `renderConfig()`
- Archivo: `index.html`
