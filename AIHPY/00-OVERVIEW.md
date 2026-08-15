# AIHPY OS — Sistema Operativo Digital
## Vault de Seguimiento de Proyecto

---

## ⚡ ESTADO DE SESIÓN

**Última sesión:** 2026-08-14
**Estado:** PLANIFICACIÓN — Schema y arquitectura definidos. Pendiente setup de infraestructura.

---

## QUÉ SE HIZO EN LA SESIÓN 2026-08-14

- Revisión completa de especificación maestra de Juan (49 secciones)
- Definición de stack técnico: mismo que hotel (CF Worker + Supabase + GitHub)
- Decisión: tablas relacionales separadas en Supabase (NO JSON único como el hotel)
- Schema completo diseñado (20 tablas): organizaciones, socios, personas, cuotas, pagos, tareas, solicitudes, proyectos, reuniones, correspondencia, documentos, auditoría, etc.
- Fases redefinidas (5 fases vs 7 de Juan) para entregar valor desde día 1
- Lecciones del hotel aplicadas desde el inicio (soft-delete, roles tempranos, no editar worker.js, build pipeline automático)

---

## PENDIENTES PARA PRÓXIMA SESIÓN

### SETUP INICIAL (requiere acción de Dani)
- [ ] Crear proyecto nuevo en Supabase → pasar URL + anon key + service role key
- [ ] Crear repo nuevo en GitHub → 
- [ ] Decidir dominio → ¿ o dominio propio AIHPY?
- [ ] Definir superadmin inicial → ¿Dani como implementador o Juan directo?

### CONSTRUCCIÓN FASE 1 (cuando esté el setup)
- [ ] Ejecutar schema SQL completo en Supabase
- [ ] Crear repo con estructura inicial + build pipeline
- [ ] Login con roles (presidencia / admin / tesoreria / directiva / viewer)
- [ ] Módulo Organizaciones — alta, edición, estados, filtros
- [ ] Módulo Socios — relación sobre organización, pipeline de captación
- [ ] Cobranzas básicas — cuotas, pagos, morosos
- [ ] Dashboard "Mi Día" básico para la Administradora

---

## ARQUITECTURA TÉCNICA

### Stack
- **Frontend:**  single-file (mismo patrón que hotel)
- **Backend:** Cloudflare Worker () — solo lógica API, nunca UI
- **Base de datos:** Supabase — proyecto independiente, tablas relacionales
- **Deploy:** GitHub Actions + Wrangler (automático)
- **Repo:**  (nuevo, independiente)

### Comandos de sesión
-  → leer este archivo, mostrar estado, pendientes, preguntar por dónde arrancamos
-  → actualizar este archivo con lo hecho y pendientes nuevos

### Lecciones del hotel aplicadas
| Error hotel | Fix AIHPY |
|---|---|
| JSON único gigante | Tablas relacionales separadas |
| Worker editado para UI | UI solo en index.html |
| Deploy manual propenso a sobreescribir | GitHub Actions automático |
| Sin tipos estrictos | Columnas tipadas en Supabase |
| Roles implementados tarde | Roles en primera tabla |
| Sin soft-delete | Toda tabla tiene , nunca DELETE físico |
| Config hardcodeada | Tabla  global desde el inicio |
| Schema improvisado | Schema completo definido antes de escribir código |

---

## SCHEMA DE TABLAS (resumen)

### Núcleo
-  — roles: presidencia / admin / tesoreria / directiva / viewer
-  — configuración global del sistema

### Capa 1 — Base Maestra
-  — entidad maestra (hoteles, instituciones, proveedores, etc.)
-  — relación sobre organización, con pipeline de captación
-  — contactos
-  — relación muchos-a-muchos

### Capa 2 — Operación
-  — generadas por período
-  — registros de cobro
-  — gastos institucionales con aprobación
-  — centro de tareas con SLA y prioridades
-  — solicitudes de socios con SLA
-  — con % avance y equipo
-  — con decisiones y compromisos
- 
-  — generan tareas automáticamente
-  — entrada/salida con responsable
-  — links a Drive con vencimientos y alertas
-  — toda acción queda registrada

---

## FASES DE IMPLEMENTACIÓN

### FASE 1 — NÚCLEO OPERATIVO (próxima sesión)
Login + roles, Organizaciones, Socios, Cobranzas básicas, Tareas, Dashboard Mi Día

### FASE 2 — GESTIÓN COMPLETA
Solicitudes con SLA, Reuniones → Tareas, Proyectos, Correspondencia, Pipeline captación, Onboarding/Offboarding

### FASE 3 — INTELIGENCIA
Dashboard Presidencia, KPIs históricos, Alertas semáforo, Centro de Decisiones, Score salud socio

### FASE 4 — INTEGRACIONES
Gmail sync, Google Calendar, Google Drive links

### FASE 5 — AUTOMATIZACIÓN + IA
Recordatorios cobranza, Reportes automáticos, Asistente IA institucional

---

## CONTEXTO DEL PROYECTO

- **Cliente:** Juan Danieri, Presidente de AIHPY
- **AIHPY:** Asociación Industrial Hotelera del Paraguay
- **Socios actuales:** ~68
- **Base nacional:** 400+ hoteles
- **Implementador:** Victor Danieri (Dani)
- **Especificación original:** Documento de 49 secciones entregado por Juan (agosto 2026)
