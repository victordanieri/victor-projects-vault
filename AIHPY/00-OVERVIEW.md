# AIHPY OS — Sistema Operativo Digital
## Vault de Seguimiento de Proyecto

---

## ESTADO DE SESION

**Ultima sesion:** 2026-08-20
**Estado:** FASE 1 LIVE — Sistema deployado y funcionando en produccion.

---

## QUE SE HIZO EN LA SESION 2026-08-20

- Rediseño UI completo: Inter font, SVG icons, sidebar blanco minimalista, acento sky blue #0EA5E9
- Ícono AIHPY: grupo de 3 hoteles de distintas alturas con ventanitas (reemplazó la casa genérica)
- Login mejorado: card centrada con logo, Enter activa el ingreso
- Validación de token al arrancar: llama /api/me contra el servidor (no confía ciegamente en localStorage)
- Cambio de contraseña propio: click en avatar/rol del sidebar → modal
- Reset de contraseña desde Usuarios (superadmin): sin necesitar la contraseña actual
- Corrección bug crítico: login ahora verifica SHA-256 legacy Y PBKDF2, migra automáticamente al hacer login
- Corrección bug salt: PATCH /api/usuarios/:id ahora busca el email real en Supabase para calcular el salt
- Permisos granulares por rol implementados en backend y frontend:
  - Leer: todos los roles autenticados
  - Crear: superadmin, presidencia, admin, tesoreria
  - Editar: superadmin, presidencia, admin
  - Eliminar: superadmin
- Botones de acción ocultos en UI según rol del usuario logueado
- Mínimo de contraseña: 6 caracteres (era 8)
- Contraseñas de Victor y Juan reseteadas y verificadas funcionando con PBKDF2
- Nuevos endpoints: GET /api/me, POST /api/me/password, PATCH mejorado /api/usuarios/:id

---

## PENDIENTES PARA PROXIMA SESION

- Coordinar con Juan los datos de los 68 socios para carga
- Crear usuarios adicionales (Administradora, Tesoreria, viewers socios)
- Probar módulos: Organizaciones, Socios, Tareas, Cobranzas, Solicitudes
- Activar backups en Supabase
- Evaluar upgrade Supabase a plan Pro
- Módulo de carga masiva de datos (socios)
- Módulo de aprobación de datos
- Configurar dominio propio cuando Juan lo decida

---

## CREDENCIALES DE PRODUCCION

- URL sistema: https://aihpy-system.victordanieri.workers.dev
- Supabase: imtymjxcwbityqdigpvm.supabase.co
- GitHub repo: victordanieri/aihpy-system
- Cloudflare worker: aihpy-system
- Victor: victordanieri@gmail.com / (contraseña reseteada esta sesión) rol superadmin
- Juan: gerencia@aihpy.org.py / (contraseña reseteada esta sesión) rol presidencia

---

## FIX CRITICO APLICADO EN SUPABASE

GRANT USAGE ON SCHEMA public TO anon, authenticated, service_role;
GRANT ALL ON ALL TABLES IN SCHEMA public TO service_role;
GRANT ALL ON ALL SEQUENCES IN SCHEMA public TO service_role;

---

## COMANDOS DE SESION

/inicio aihpy -> leer este archivo, mostrar estado y pendientes
/fin aihpy -> actualizar este archivo con lo hecho y pendientes

---

## FASES

FASE 1 COMPLETADA: Login, infraestructura, seguridad, deploy, dashboard, UI rediseño, permisos por rol
FASE 2 EN CURSO: Cargar datos reales (68 socios), probar módulos, usuarios adicionales
FASE 3: Dashboard Presidencia, KPIs, alertas, Centro de Decisiones
FASE 4: Gmail, Calendar, Drive
FASE 5: Automatizacion + IA

---

## CONTEXTO

- Cliente: Juan Danieri, Presidente AIHPY
- Email Juan: gerencia@aihpy.org.py
- Socios actuales: 68 aprox
- Base nacional: 400+ hoteles
- Implementador: Victor Danieri
