<div align="center">

# ⚽ Fut5Go

**La plataforma inteligente para gestión de canchas de fútbol, reservas en tiempo real y torneos barriales.**

[![Status](https://img.shields.io/badge/Status-Scaffolding%20%26%20Diseño-blue.svg)](#)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Workspaces](https://img.shields.io/badge/Architecture-Monorepo%20(Front%20%2B%20Back)-purple.svg)](#)
[![Team](https://img.shields.io/badge/Team-3%20Developers-orange.svg)](#)

</div>

---

## 📌 ¿Qué es Fut5Go?

**Fut5Go** es un ecosistema digital diseñado para modernizar y digitalizar la experiencia del fútbol amateur. Conecta en una sola plataforma a:
1. **Complejos y Dueños de Canchas:** Herramienta SaaS B2B para administrar grillas horarias, automatizar señas/pagos, evitar turnos solapados y reducir ausencias (*no-shows*).
2. **Jugadores y Equipos:** Buscador geolocalizado de canchas disponibles, reserva inmediata compartiendo el pago entre amigos, y armado de partidos.
3. **Organizadores de Torneos:** Gestión integral de copas y ligas locales (inscripciones, tablas de posiciones, cruces/fixtures, tabla de goleadores y sanciones).

---

## 👥 Estructura del Equipo y División de Roles

El proyecto está diseñado para desarrollarse de manera colaborativa y organizada entre **3 desarrolladores**, con responsabilidades claras y sin pisarse el trabajo:

| Rol | Miembro | Área de Enfoque | Responsabilidades Clave |
| :--- | :--- | :--- | :--- |
| **Rol 1: Frontend & UX Lead** | *Amigo 1* | Cliente Web & Mobile-First UI | • Diseño y maquetación de pantallas (Portal Jugador y Dashboard Canchas).<br>• Grilla horaria interactiva (calendario de turnos tipo Google Calendar).<br>• Estado global (Zustand/Redux), llamadas a API con React Query.<br>• Responsive design y experiencia PWA para celulares. |
| **Rol 2: Backend & Logic Lead** | *Amigo 2* | REST API, Reglas de Negocio & Pagos | • Endpoints y arquitectura REST / GraphQL.<br>• Motor de reservas y prevención de colisiones de turnos con transacciones.<br>• Integración de pasarelas de pago (Mercado Pago / Checkout Pro / Webhooks).<br>• Lógica de torneos (generación de fixtures, tablas automáticas). |
| **Rol 3: Data, DevOps & Infra Lead** | *Amigo 3* | Base de Datos, Cloud, Auth & Calidad | • Modelado de base de datos relacional (PostgreSQL/MySQL) y migraciones.<br>• Autenticación segura (JWT, OAuth, roles RBAC).<br>• Contenedores Docker, CI/CD en GitHub Actions y despliegue.<br>• Notificaciones (WhatsApp API / Email) y WebSockets para cambios en vivo. |

> 📖 *Para ver el detalle completo de responsabilidades, flujos diarios y tareas paso a paso, consulta [docs/ROLES_AND_WORKFLOW.md](docs/ROLES_AND_WORKFLOW.md).*

---

## 🏛️ Arquitectura del Repositorio

El proyecto utiliza un enfoque **Monorepo** modular y limpio:

```text
fut5go/
├── .github/                     # Plantillas de PRs, issues y CI/CD de GitHub
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── workflows/
│       └── ci.yml
│
├── apps/
│   ├── frontend/                # Aplicación cliente (React / Vite / Tailwind)
│   │   ├── public/              # Assets estáticos
│   │   ├── src/
│   │   │   ├── assets/          # Imágenes, íconos y logos
│   │   │   ├── components/      # Componentes modulares y reutilizables
│   │   │   │   ├── common/      # Botones, modales, inputs
│   │   │   │   ├── booking/     # Grilla horaria, selector de fecha, checkout
│   │   │   │   ├── complex/     # Panel de administración de la cancha
│   │   │   │   └── tournaments/ # Tablas de posiciones, fixtures, llaves
│   │   │   ├── hooks/           # Custom hooks
│   │   │   ├── layouts/         # Layouts (Navbar, Sidebar, Footer)
│   │   │   ├── pages/           # Vistas principales de la app
│   │   │   ├── services/        # Clientes Axios / Fetch para la API
│   │   │   ├── store/           # Manejadores de estado (Zustand)
│   │   │   ├── types/           # Interfaces y tipos TypeScript
│   │   │   └── utils/           # Formateadores de fecha, moneda, etc.
│   │   ├── .env.example
│   │   └── package.json
│   │
│   └── backend/                 # API Server (Node.js / Express / TypeScript)
│       ├── src/
│       │   ├── config/          # Variables de entorno y conexiones
│       │   ├── controllers/     # Controladores de solicitudes HTTP
│       │   ├── middlewares/     # Auth, validaciones (Zod/Joi), errores
│       │   ├── models/          # Entidades / Modelos ORM (Prisma/TypeORM)
│       │   ├── routes/          # Definición de rutas y endpoints
│       │   ├── services/        # Lógica pura de negocio
│       │   │   ├── booking/     # Validación de disponibilidad y bloqueos
│       │   │   ├── payments/    # Webhooks y cobro de señas
│       │   │   ├── tournaments/ # Algoritmos de fixtures y puntos
│       │   │   └── notifications/# Disparo de WhatsApp / Emails
│       │   └── utils/           # Helpers y utilitarios
│       ├── .env.example
│       └── package.json
│
├── database/                    # Esquemas, migraciones y seeders de prueba
│   ├── migrations/
│   └── seeds/
│
├── docker/                      # Orquestación de contenedores locales
│   ├── docker-compose.yml
│   ├── Dockerfile.frontend
│   └── Dockerfile.backend
│
├── docs/                        # Documentación técnica y del negocio
│   ├── ARCHITECTURE.md          # Diagrama de arquitectura y flujos
│   ├── DATABASE_DESIGN.md       # Modelo de datos y relaciones ERD
│   ├── ROLES_AND_WORKFLOW.md    # Protocolo de trabajo en equipo y ramas
│   └── ROADMAP.md               # Plan de entregas por fases
│
├── .editorconfig                # Configuración unificada de indentación
├── .gitignore                   # Archivos ignorados por Git
├── .env.example                 # Plantilla general de variables de entorno
├── package.json                 # Orquestador raíz de scripts (Workspaces)
└── README.md                    # Documento principal
```

---

## 🛠️ Stack Tecnológico Sugerido

- **Frontend:** React 19 + TypeScript + Vite, Tailwind CSS, Lucide Icons, Zustand (estado ligero), TanStack Query (React Query).
- **Backend:** Node.js + Express (o NestJS) + TypeScript.
- **Base de Datos:** PostgreSQL (recomendado para integridad transaccional de reservas) + Prisma ORM o TypeORM.
- **Caché & Concurrencia:** Redis (para bloquear turnos durante los 5-10 minutos de checkout y evitar doble reserva).
- **Infraestructura & Contenedores:** Docker & Docker Compose.
- **Integraciones:** Mercado Pago SDK (Checkout Pro y Webhooks), Twilio / WhatsApp Cloud API para confirmación de turnos.

---

## 🔄 Flujo de Trabajo en Git (Git Workflow)

Para trabajar de manera ordenada entre los 3 sin pisarse código:

1. **Rama Principal (`main`):** Siempre contiene código estable y listo para desplegar. Nadie hace commit directo a `main`.
2. **Rama de Integración (`develop`):** Donde confluyen las funcionalidades terminadas antes de pasar a producción.
3. **Ramas de Funcionalidad (`feature/...`):**
   - Frontend: `feature/front-booking-calendar`, `feature/front-complex-dashboard`
   - Backend: `feature/back-reservations-api`, `feature/back-mercadopago-webhook`
   - Data/DevOps: `feature/db-tournaments-schema`, `feature/docker-setup`
4. **Pull Requests (PRs):**
   - Toda funcionalidad entra mediante Pull Request hacia `develop`.
   - Se requiere al menos **1 revisión y aprobación** de otro compañero antes de hacer merge.
5. **Formato de Commits (Conventional Commits):**
   - `feat: agregar grilla de turnos semanal`
   - `fix: corregir validacion de solapamiento de horarios`
   - `docs: actualizar diagrama de base de datos`
   - `chore: agregar configuracion de docker compose`

---

## 🚀 Primeros Pasos (Setup Inicial)

### Requisitos Previos
- [Node.js](https://nodejs.org/) (versión 20 LTS o superior)
- [Git](https://git-scm.com/)
- [Docker Desktop](https://www.docker.com/) (opcional pero recomendado)

### Clonar el Repositorio
```bash
git clone https://github.com/TU_USUARIO/fut5go.git
cd fut5go
```

### Variables de Entorno
Crea tus archivos `.env` a partir de los ejemplos:
```bash
cp .env.example .env
cp apps/frontend/.env.example apps/frontend/.env
cp apps/backend/.env.example apps/backend/.env
```

---

## 🗺️ Roadmap de Desarrollo

- [x] **Fase 0: Scaffolding & Arquitectura** (Estructura de carpetas, documentación, definición de roles).
- [ ] **Fase 1: Módulo Core de Canchas y Turnos** (Registro de complejos, configuración de canchas/precios, reserva y cobro de seña).
- [ ] **Fase 2: Portal de Jugadores & Geolocalización** (Búsqueda de canchas por zona, mapa interactivo, perfil de usuario).
- [ ] **Fase 3: Módulo de Torneos** (Creación de copas/ligas, inscripción de equipos, fixture automático y tablas).
- [ ] **Fase 4: Notificaciones en Tiempo Real & PWA** (Avisos de recordatorio por WhatsApp y WebSockets para cambios inmediatos).

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Consulta el archivo [LICENSE](LICENSE) para más detalles.
