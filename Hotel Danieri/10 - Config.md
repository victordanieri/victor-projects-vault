# ⚙️ Config

**Función:** Configuración general del sistema

## Secciones
- **Hotel** — nombre, dirección, datos generales
- **Monedas** — solo USD y Gs. (PYG). BRL/ARS eliminados del sistema (05/08)
- **Habitaciones** — agregar/editar habitaciones y tipos
- **Servicios** — lista de servicios cobrados
- **Origenes & Staff** — canales de reserva (staff viene de Usuarios)
- **Usuarios** — gestión de usuarios y contraseñas (solo Superadmin)
- **Datos** — exportar/importar JSON, sincronizar Supabase

## Reglas de seguridad (reescritas — 5 niveles de hardening, 07/08/2026)
- Contraseñas: hash `pbkdf2v2` (PBKDF2, 8000 iteraciones) calculado y
  verificado **server-side** en el Worker, ya no SHA-256 client-side
- Login pasa por `/api/login` en el Worker, con rate limiting real por
  usuario vía KV (`LOGIN_ATTEMPTS`)
- Autorización de escritura por rol validada en el servidor (no solo oculta
  botones en el front)
- Bloqueo de navegación forzada por consola a tabs de admin
- Concurrencia optimista: si el blob cambió en el servidor desde la última
  lectura del cliente, se bloquea el guardado con un aviso (409) hasta
  refrescar — aplica igual a todos los roles, incluido superadmin
- Solo Superadmin puede cambiar contraseñas (botón 🔑 Clave)
- Staff/responsables se toman automáticamente de Usuarios activos

## Estado
- 🟢 Funciona correctamente, seguridad reforzada y verificada en producción

## Código
- Función: `renderCfg()` (front). Lógica de seguridad server-side vive en
  `build_worker.py` → se compila a `worker.js` (no en `index.html`)
- Archivo: `index.html`, `build_worker.py`
