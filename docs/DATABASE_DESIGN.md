# 💾 Diseño del Modelo de Datos (Database Design) - Fut5Go

Este documento describe el esquema de base de datos relacional para el sistema **Fut5Go**, diseñado para soportar tanto la gestión de turnos/canchas como el módulo de torneos barriales.

---

## 1. Diagrama Entidad-Relación (ERD)

```mermaid
erDiagram
    USERS ||--o{ COMPLEXES : "posee (dueño)"
    USERS ||--o{ BOOKINGS : "reserva"
    USERS ||--o{ TEAMS : "capitanea"
    
    COMPLEXES ||--|{ PITCHES : "contiene"
    COMPLEXES ||--|{ OPERATING_HOURS : "define horario"
    COMPLEXES ||--o{ TOURNAMENTS : "organiza"
    
    PITCHES ||--o{ BOOKINGS : "es reservada en"
    PITCHES ||--o{ MATCHES : "alberga partidos de"
    
    BOOKINGS ||--o{ PAYMENTS : "tiene registro de pago"
    
    TOURNAMENTS ||--|{ TEAMS : "inscribe"
    TOURNAMENTS ||--|{ MATCHES : "compuesto por"
    
    TEAMS ||--o{ MATCHES : "juega local/visitante"

    USERS {
        uuid id PK
        string full_name
        string email UK
        string phone
        string password_hash
        string role "ADMIN | COMPLEX_OWNER | PLAYER"
        datetime created_at
    }

    COMPLEXES {
        uuid id PK
        uuid owner_id FK
        string name
        string address
        string city
        decimal latitude
        decimal longitude
        string phone
        text cancel_policy
        datetime created_at
    }

    PITCHES {
        uuid id PK
        uuid complex_id FK
        string name "Ej: Cancha 1 - Sintético"
        string pitch_type "FUTBOL_5 | FUTBOL_7 | FUTBOL_11"
        string surface "SINTETICO | NATURAL | PARQUET"
        boolean is_covered
        decimal price_per_hour
        boolean is_active
    }

    OPERATING_HOURS {
        uuid id PK
        uuid complex_id FK
        int day_of_week "0=Domingo a 6=Sábado"
        time open_time
        time close_time
    }

    BOOKINGS {
        uuid id PK
        uuid pitch_id FK
        uuid user_id FK
        date booking_date
        time start_time
        time end_time
        decimal total_price
        decimal deposit_amount
        string status "HOLD | PENDING_PAYMENT | CONFIRMED | CANCELLED | COMPLETED"
        datetime hold_expires_at
        datetime created_at
    }

    PAYMENTS {
        uuid id PK
        uuid booking_id FK
        string gateway "MERCADO_PAGO"
        string external_id
        decimal amount
        string status "PENDING | APPROVED | REJECTED | REFUNDED"
        json raw_gateway_response
        datetime created_at
    }

    TOURNAMENTS {
        uuid id PK
        uuid complex_id FK
        string name
        string format "LEAGUE | PLAYOFF | GROUP_STAGE"
        decimal registration_fee
        date start_date
        date end_date
        int max_teams
        string status "REGISTRATION_OPEN | IN_PROGRESS | FINISHED"
    }

    TEAMS {
        uuid id PK
        uuid tournament_id FK
        uuid captain_user_id FK
        string name
        string logo_url
        int points
        int played_matches
        int goals_for
        int goals_against
    }

    MATCHES {
        uuid id PK
        uuid tournament_id FK
        uuid home_team_id FK
        uuid away_team_id FK
        uuid pitch_id FK
        datetime scheduled_at
        int home_score
        int away_score
        string status "SCHEDULED | PLAYING | FINISHED | SUSPENDED"
    }
```

---

## 2. Índices y Restricciones Clave para Alto Rendimiento

Para asegurar consultas rápidas y prevenir turnos duplicados:

1. **Restricción Unitaria (Constraint) para Evitar Solapamientos:**
   ```sql
   -- Ejemplo conceptual: No puede haber 2 reservas confirmadas para la misma cancha, fecha y hora de inicio
   CREATE UNIQUE INDEX idx_unique_confirmed_booking 
   ON bookings (pitch_id, booking_date, start_time) 
   WHERE status IN ('CONFIRMED', 'HOLD');
   ```

2. **Índices de Búsqueda Geográfica y por Fecha:**
   - `INDEX idx_complexes_city_location (city, latitude, longitude);` (Búsqueda rápida de canchas en el mapa).
   - `INDEX idx_bookings_date_status (booking_date, status);` (Filtrado del calendario para saber qué slots están ocupados).

3. **Campos Soft-Delete (Opcional):**
   - Se recomienda agregar `deleted_at TIMESTAMP NULL` en complejos y canchas para no romper el historial de reservas de años anteriores si una cancha se da de baja.
