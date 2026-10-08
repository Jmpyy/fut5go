# 👥 Guía de Roles y Flujo de Trabajo en Equipo

Este documento establece la distribución de responsabilidades y el protocolo de colaboración para el equipo de **Fut5Go** (3 desarrolladores).

---

## 🎯 Distribución de Roles

### 👤 Miembro 1: Frontend & UX Lead (Experiencia de Usuario)
**Foco:** Todo lo que ve y toca el usuario final (jugadores, capitanes de equipo y administradores de canchas).

* **Responsabilidades:**
  - Maquetación y desarrollo de componentes reutilizables con Tailwind CSS y React.
  - Implementación del **Dashboard para Complejos**:
    - Grilla horaria interactiva (tipo calendario semanal/diario con slots libres, ocupados o bloqueados).
    - Gestión de canchas (precio por hora, tipo de césped, techada/descubierta).
  - Implementación del **Portal para Jugadores**:
    - Buscador y filtros de canchas (por zona/barrio, precio, tipo 5/7/11, horario).
    - Flujo de reserva (selección de slot, confirmación de datos, botón de pago).
  - Implementación del **Módulo de Torneos (Visual)**:
    - Vista de tablas de posiciones, cruces/playoffs y estadísticas de goleadores.
  - Integración con el backend mediante Axios / TanStack Query (React Query) y manejo de estados globales con Zustand.
  - Optimización Responsive & Mobile-First (pensado para usarse desde el celular al salir del trabajo).

---

### 👤 Miembro 2: Backend & Business Logic Lead (Lógica de Negocio y API)
**Foco:** Las reglas del sistema, endpoints RESTful, seguridad de transacciones y pasarelas de pago.

* **Responsabilidades:**
  - Diseño y desarrollo de la REST API (Node.js + Express / NestJS con TypeScript).
  - **Motor de Reservas (Core):**
    - Algoritmo de validación de disponibilidad horaria.
    - Prevención de doble reserva simultánea (control de concurrencia y bloqueos temporales de turnos).
    - Cálculo de señas y cancelaciones según políticas de cada complejo.
  - **Integración de Pasarelas de Pago:**
    - Mercado Pago SDK (Checkout Pro y suscripciones para complejos).
    - Endpoint seguro para recepción y validación de Webhooks de pago.
  - **Motor de Torneos (Lógica):**
    - Generación automática de fixtures (todos contra todos / llaves de eliminación directa).
    - Cálculo de puntos, diferencia de gol y posiciones tras el cierre de cada partido.
  - Validaciones de entrada estrictas (con Zod o Joi) y manejo de respuestas de error estandarizadas.

---

### 👤 Miembro 3: Data, DevOps & Infra Lead (Arquitectura de Datos y Operaciones)
**Foco:** La base de datos, la nube, la autenticación y la infraestructura del sistema.

* **Responsabilidades:**
  - **Base de Datos y Modelado:**
    - Diseño del esquema relacional (PostgreSQL / MySQL) en Prisma o TypeORM.
    - Creación de migraciones y scripts de carga de datos iniciales (*seeders* con canchas y torneos de prueba).
    - Índices y optimización de consultas (por ejemplo, búsquedas por rango de fecha/hora y geolocalización).
  - **Autenticación y Autorización (RBAC):**
    - Sistema seguro de login y registro (JWT, hashing con bcrypt).
    - Control de accesos según roles: `ADMIN_GENERAL`, `COMPLEX_OWNER`, `PLAYER`, `REFEREE/ORGANIZER`.
  - **DevOps y Entornos:**
    - Configuración y mantenimiento de Docker y Docker Compose para desarrollo local unificado.
    - Automatización de CI/CD mediante GitHub Actions (linter, validación de TypeScript, tests básicos).
  - **Integraciones y Tiempo Real:**
    - Configuración de WebSockets (Socket.io) para actualizar la grilla de turnos en tiempo real cuando alguien reserva.
    - Servicio de notificaciones (plantillas de confirmación vía WhatsApp API o Email).

---

## 🌿 Estrategia de Ramas en Git (Git Flow simplificado)

Para evitar conflictos y trabajar en paralelo sin riesgos:

```
main (Producción - Sólo versiones estables etiquetadas)
  │
develop (Integración - El código que se está probando actualmente)
  │
  ├── feature/front-booking-grid      (Trabajo de Frontend)
  ├── feature/back-reservations-api   (Trabajo de Backend)
  └── feature/db-initial-schema       (Trabajo de DevOps / Data)
```

### 1. Convención de Nombres de Ramas
- Nuevas funcionalidades: `feature/<area>-<nombre-corto>`
  - Ejemplos: `feature/front-auth-screens`, `feature/back-mercadopago`, `feature/db-tournaments-migration`
- Corrección de errores: `fix/<area>-<nombre-del-bug>`
  - Ejemplos: `fix/front-slot-time-format`, `fix/back-overlap-query`
- Mejoras técnicas o configuración: `chore/<nombre>`
  - Ejemplos: `chore/docker-compose-redis`, `chore/eslint-setup`

### 2. Ciclo de Vida de una Tarea
1. Antes de empezar, pararse en `develop` y actualizar:
   ```bash
   git checkout develop
   git pull origin develop
   ```
2. Crear la rama de trabajo:
   ```bash
   git checkout -b feature/front-login-page
   ```
3. Realizar commits pequeños y con mensajes descriptivos.
4. Subir la rama a GitHub:
   ```bash
   git push origin feature/front-login-page
   ```
5. Abrir un **Pull Request (PR)** hacia la rama `develop`.
6. Al menos **un compañero del equipo debe revisar y aprobar el PR** antes de fusionarlo.

---

## ✍️ Formato de Mensajes de Commit (Conventional Commits)

Utilizaremos el estándar de *Conventional Commits*:
- `feat:` Nueva funcionalidad para el usuario.
- `fix:` Corrección de un error o bug.
- `docs:` Cambios exclusivamente en la documentación.
- `style:` Formateo, espacios en blanco, punto y coma (sin cambio de lógica).
- `refactor:` Modificación de código que no agrega funcionalidad ni arregla un bug.
- `chore:` Tareas auxiliares (configuración de build, librerías, docker, scripts).

**Ejemplo:**
```bash
git commit -m "feat(booking): agregar bloqueo temporal de slot por 10 minutos"
git commit -m "fix(auth): corregir expiracion de token jwt en refresh"
```

---

## 📅 Ritmo de Comunicación Recomendado

- **Sincronización Semanal (30 minutos):**
  - ¿Qué completamos esta semana?
  - ¿Qué módulo empezamos la próxima?
  - ¿Hay dependencias bloqueantes (por ejemplo: el front necesita los endpoints de canchas)?
- **Definición de Contratos Primero (API First):**
  - Antes de que el Frontend empiece a programar una vista compleja o el Backend un servicio, ambos acuerdan la estructura del JSON (ver `docs/API_SPEC.md`). De esa forma, el frontend puede trabajar con mocks mientras el backend programa la lógica real.
