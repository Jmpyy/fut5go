# ⚙️ Fut5Go - Backend API

Servicio de API REST desarrollado con **Node.js, Express y TypeScript**.

---

## 📂 Organización de Carpetas (`src/`)

```text
src/
├── config/             # Configuración de variables de entorno y base de datos
├── controllers/        # Controladores HTTP (manejo de req, res y status codes)
│   ├── authController.ts
│   ├── complexController.ts
│   ├── pitchController.ts
│   ├── bookingController.ts
│   ├── tournamentController.ts
│   └── paymentController.ts
├── middlewares/        # Middlewares de autenticación, validación de schemas y error handler
│   ├── authMiddleware.ts
│   ├── roleMiddleware.ts
│   ├── validateSchema.ts
│   └── errorHandler.ts
├── models/             # Esquemas de datos / Modelos de entidad (Prisma / TypeORM)
├── routes/             # Enrutadores Express mapeados a controladores
│   ├── index.ts
│   ├── authRoutes.ts
│   ├── complexRoutes.ts
│   ├── pitchRoutes.ts
│   ├── bookingRoutes.ts
│   ├── tournamentRoutes.ts
│   └── paymentRoutes.ts
├── services/           # Lógica pura de negocio independiente de HTTP
│   ├── booking/        # Algoritmo de bloqueo de slots y cálculo de señas
│   ├── payments/       # Integración con Mercado Pago y Webhooks
│   ├── tournaments/    # Generación de fixtures y tablas de posiciones
│   └── notifications/  # Envío de mensajes de confirmación
└── utils/              # Funciones auxiliares, parseo de fechas, formateadores
```

---

## 🚀 Comandos

```bash
# Instalar dependencias
npm install

# Modo desarrollo con recarga automática
npm run dev

# Compilar para producción
npm run build

# Iniciar servidor compilado
npm start
```
