# 🗺️ Roadmap de Desarrollo - Fut5Go

Este documento desglosa el plan de trabajo por etapas para guiar al equipo de 3 desarrolladores hacia el lanzamiento del Producto Mínimo Viable (MVP) y versiones posteriores.

---

## 📌 Fase 0: Scaffolding, Estructura y Acuerdos de Equipo (Completada ✅)
- [x] Creación del árbol de carpetas monorepo (`apps/frontend`, `apps/backend`, `database`, `docker`, `docs`).
- [x] Definición de roles y responsabilidades para los 3 miembros del equipo.
- [x] Especificación de arquitectura y modelo de datos relacional.
- [x] Configuración de plantillas de GitHub (Issues, PRs, CI/CD).
- [x] Definición de estándares de commits y ramas (`develop` / `feature/*`).

---

## ⚽ Fase 1: MVP Core - Gestión de Canchas y Reservas Básicas
**Objetivo:** Permitir que un dueño de complejo cargue sus canchas y horarios, y que un usuario pueda reservar un turno y pagar una seña.

### Tareas Frontend (Rol 1)
- [ ] Maquetar pantalla de Landing Page con presentación del servicio.
- [ ] Diseñar el panel de administración del complejo:
  - Formulario de alta/edición de canchas (nombre, tipo, precio).
  - Configuración de horarios de apertura/cierre.
- [ ] Construir la grilla horaria interactiva de turnos (vista día y vista semana).
- [ ] Implementar el modal de reserva de turno y resumen de precio/seña.

### Tareas Backend (Rol 2)
- [ ] Implementar endpoints CRUD de Complejos y Canchas (`/api/complexes`, `/api/pitches`).
- [ ] Desarrollar endpoint de consulta de slots disponibles por fecha (`/api/pitches/:id/availability`).
- [ ] Implementar endpoint de bloqueo temporal y confirmación de reserva (`/api/bookings`).
- [ ] Integrar el SDK de Mercado Pago para generar preferencia de cobro de seña.
- [ ] Crear webhook de Mercado Pago (`/api/payments/webhook`) para confirmar turnos automáticamente.

### Tareas Data & DevOps (Rol 3)
- [ ] Configurar proyecto base de base de datos (migraciones iniciales con Prisma / TypeORM).
- [ ] Implementar autenticación con JWT y middlewares de roles (`ADMIN`, `COMPLEX_OWNER`, `PLAYER`).
- [ ] Configurar Docker Compose local (PostgreSQL + Redis opcional + API).
- [ ] Realizar seeders con 2 complejos reales o ficticios con canchas para pruebas de desarrollo.

---

## 🏆 Fase 2: Portal de Jugadores y Torneos Locales
**Objetivo:** Permitir a los jugadores buscar canchas cercanas y a los complejos organizar torneos con tablas automáticas.

### Tareas Frontend (Rol 1)
- [ ] Buscador de canchas con filtros (por zona, precio, tipo 5/7/11).
- [ ] Pantalla de perfil de jugador y sección "Mis Reservas" con estado del turno.
- [ ] Interfaz visual del módulo de torneos:
  - Tabla de posiciones (PJ, G, E, P, GF, GC, DG, Pts).
  - Grilla de cruces o fixture de la fecha.

### Tareas Backend (Rol 2)
- [ ] Endpoints de búsqueda y filtrado de canchas.
- [ ] Módulo de torneos:
  - CRUD de torneos y equipos inscriptos.
  - Algoritmo generador de fixture (Round-Robin / Todos contra todos).
  - Endpoint para cargar resultado de partido y recálculo automático de la tabla.

### Tareas Data & DevOps (Rol 3)
- [ ] Migraciones de base de datos para tablas `tournaments`, `teams` y `matches`.
- [ ] Configurar pipeline de CI en GitHub Actions para verificar build y linter en cada PR.
- [ ] Preparar despliegue de staging (por ejemplo en Render / Railway / Supabase / Vercel).

---

## 🚀 Fase 3: Tiempo Real, Notificaciones y Monetización
**Objetivo:** Elevar la calidad del producto a nivel SaaS comercial.

- [ ] WebSockets para actualizar en vivo la grilla de turnos (si alguien reserva, a los demás se les pinta en gris en tiempo real).
- [ ] Notificaciones automáticas por WhatsApp (recordatorio 3 horas antes del partido).
- [ ] Función de "Dividir el pago entre amigos" (generación de link con monto dividido por 10 jugadores).
- [ ] Planes de suscripción mensual para los complejos deportivos (cobro recurrente por uso de la plataforma).
