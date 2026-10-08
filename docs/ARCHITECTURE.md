# 🏛️ Arquitectura del Sistema - Fut5Go

Este documento detalla la arquitectura técnica, las capas de la aplicación y los flujos críticos de información entre los clientes, la API y los servicios de terceros.

---

## 1. Visión General de la Arquitectura

```mermaid
flowchart TD
    subgraph Clientes ["📱 Clientes (Frontend)"]
        A1["Portal Jugador / Web Mobile (React + Vite)"]
        A2["Panel Cancha / Complejo (React + Vite)"]
        A3["Admin Plataforma"]
    end

    subgraph API_Gateway ["🛡️ Capa de Entrada & Seguridad"]
        B["API Gateway / Express Server con CORS & Rate Limit"]
    end

    subgraph Backend_Services ["⚙️ Servicios de Negocio (Backend)"]
        C1["Módulo de Autenticación & Usuarios"]
        C2["Módulo de Canchas & Horarios"]
        C3["Motor de Reservas & Concurrencia"]
        C4["Módulo de Torneos & Fixtures"]
        C5["Integrador de Pagos (Mercado Pago)"]
        C6["Servicio de Notificaciones (WhatsApp/Email)"]
    end

    subgraph Persistencia ["💾 Datos & Caché"]
        D1[("Base de Datos Relacional\nPostgreSQL")]
        D2[("Redis\nLocks de Turnos & Caché")]
    end

    subgraph Externos ["🌐 Servicios Externos"]
        E1["Mercado Pago API (Webhooks)"]
        E2["WhatsApp Cloud API / Twilio"]
        E3["Mapas / Geolocalización"]
    end

    A1 -->|HTTPS / REST & WS| B
    A2 -->|HTTPS / REST & WS| B
    A3 -->|HTTPS / REST| B

    B --> C1
    B --> C2
    B --> C3
    B --> C4
    B --> C5
    B --> C6

    C1 & C2 & C4 --> D1
    C3 -->|Transacciones ACID| D1
    C3 -->|Lock temporal 10 min| D2
    C5 --> E1
    C6 --> E2
    A1 & A2 -.-> E3
```

---

## 2. Flujo Crítico: Reserva de Turno con Prevención de Solapamiento

El problema clásico en aplicaciones de canchas es que dos usuarios intenten reservar el mismo turno al mismo tiempo. El siguiente diagrama ilustra cómo se previene la doble reserva mediante bloqueo temporal:

```mermaid
sequenceDiagram
    autonumber
    actor Jugador as Jugador (Usuario)
    participant Front as Frontend (Web/PWA)
    participant API as Backend API
    participant Cache as Redis (Lock)
    participant DB as PostgreSQL
    participant MP as Mercado Pago

    Jugador->>Front: Selecciona Cancha y Horario (ej: 20:00 - 21:00)
    Front->>API: POST /api/bookings/hold (PitchId, Fecha, Hora)
    
    API->>Cache: Solicitar Lock temporal (10 minutos)
    alt Turno ya bloqueado o reservado
        Cache-->>API: Slot ocupado
        API-->>Front: Error: "El turno ya está siendo reservado por otro usuario"
    else Slot disponible
        Cache-->>API: Lock concedido con TTL de 10 min
        API->>DB: Crear registro de Reserva (Estado: PENDING_PAYMENT)
        API->>MP: Generar Preferencia de Pago (Seña)
        MP-->>API: URL de pago / Checkout Init
        API-->>Front: Retorna Link de pago y temporizador de 10 min
        Front-->>Jugador: Redirige a Mercado Pago
    end

    Jugador->>MP: Realiza el pago de la seña
    MP->>API: Webhook (payment.updated: APPROVED)
    
    API->>DB: Actualizar Reserva a CONFIRMED
    API->>Cache: Liberar Lock temporal (ya es definitivo en DB)
    API->>Front: Notificación WebSocket (Slot ocupado)
    API->>Jugador: Envía WhatsApp de confirmación con código de reserva
```

---

## 3. Capas del Software

### Frontend (`apps/frontend`)
- **UI Components:** Basados en Tailwind CSS y componentes modulares accesibles.
- **State Management:** 
  - `Zustand` para el estado global del cliente (usuario autenticado, carrito/turno en proceso).
  - `TanStack Query` para caching del lado servidor, reintentos automáticos y sincronización de datos de canchas.
- **Client Services:** Abstracción en `src/services/` para cada dominio (`authService`, `bookingService`, `complexService`, `tournamentService`).

### Backend (`apps/backend`)
- **Controllers:** Manejo de la petición HTTP, validación de parámetros mediante esquemas (Zod) y envío de códigos de estado HTTP semánticos (200, 201, 400, 401, 404, 409).
- **Services (Lógica Pura):** No conocen los objetos `req` ni `res`. Reciben datos planos y orquestan la lógica de negocio, validaciones cruzadas y llamadas a la base de datos.
- **Middlewares:**
  - `authMiddleware`: Valida el token JWT y adjunta `req.user`.
  - `roleMiddleware`: Verifica que el usuario tenga el rol necesario (ej. `COMPLEX_ADMIN`).
  - `errorMiddleware`: Captura excepciones no controladas y formatea una respuesta limpia sin filtrar información sensible del servidor.

### Persistencia y Concurrencia
- **PostgreSQL:** Elección ideal por soporte de transacciones ACID estrictas (`SELECT ... FOR UPDATE` en caso de no contar con Redis) e integridad referencial sólida.
- **Redis (Opcional en fase inicial, recomendado en producción):** Manejo de locks distribuidos mediante `SET resource_name my_random_value NX PX 600000`.
