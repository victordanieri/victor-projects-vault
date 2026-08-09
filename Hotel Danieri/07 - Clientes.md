# 👥 Clientes

**Función:** Base de datos de huéspedes, historial de estadías, mensajes WhatsApp

## Conexiones
- ← [[02 - Habitaciones]] — check-in crea/actualiza cliente
- ← [[05 - Caja y Cobros]] — historial de pagos
- → [[08 - Canales OTA]] — origen de la reserva

## Migración automática (nuevo)
- Al cargar el sistema, los check-ins históricos se migran/sincronizan
  automáticamente a la base de clientes (sin duplicados)
- Al confirmar un check-in, el cliente se crea/actualiza automáticamente con
  datos completos (incluye origen/canal tomado del check-in)

## Sub-secciones
- Lista y ficha (`renderCliLista()`, `renderCliForm()`, `renderCliPerfil()`)
- Acompañantes (`renderCliAcompanantes()`)
- Campañas WhatsApp (`renderCliCampanas()`)
- Cumpleaños (`renderCliCumples()`)
- Config del módulo (`renderCliConfig()`)

## Estado
- 🟢 Módulo funciona, migración e integración con check-in automatizadas

## Código
- Función: `renderClientes()` (contenedor)
- Archivo: `index.html`
