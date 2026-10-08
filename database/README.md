# 💾 Fut5Go - Base de Datos

Este directorio contiene las migraciones, esquemas y scripts de datos de prueba (*seeds*) para la base de datos de **Fut5Go**.

---

## 📂 Estructura

```text
database/
├── migrations/      # Archivos SQL o migraciones generadas por ORM (Prisma/TypeORM)
├── seeds/           # Datos iniciales para pruebas (complejos, canchas, usuarios demo)
└── README.md        # Esta guía
```

---

## 📌 Guía Rápida para Sembrar Datos de Prueba (Seeders)

Los seeders deben incluir datos representativos para que el equipo pueda probar la aplicación localmente sin tener que cargar todo a mano:

1. **Usuario Administrador:** `admin@fut5go.com`
2. **Usuario Dueño de Cancha:** `dueno@canchaspalermo.com`
3. **Usuario Jugador:** `jugador@test.com`
4. **Complejo Ejemplo:** "Complejo Fútbol Central" con 3 canchas (Fútbol 5 sintético techada, Fútbol 5 descubierta, Fútbol 7 sintético).
5. **Horarios de Atención:** Lunes a Domingo de 14:00 a 00:00 hs.
6. **Torneo Ejemplo:** "Torneo Clausura 2026 - Copa Relámpago" con 8 equipos y fase de grupos.

Consulta [docs/DATABASE_DESIGN.md](../docs/DATABASE_DESIGN.md) para ver la especificación completa de tablas, campos y relaciones.
