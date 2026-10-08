# 📱 Fut5Go - Frontend

Aplicación cliente web desarrollada con **React 19 + TypeScript + Vite + Tailwind CSS**.

---

## 📂 Organización de Carpetas (`src/`)

```text
src/
├── assets/          # Imágenes, logos, íconos SVG
├── components/      # Componentes UI organizados por dominio
│   ├── common/      # Botones, Modales, Inputs, Navbar, Footer
│   ├── booking/     # Grilla horaria, calendario interactivo, resumen de pago
│   ├── complex/     # Panel del complejo, formularios de canchas y horarios
│   └── tournaments/ # Tablas de posiciones, fixtures, llaves de playoffs
├── hooks/           # Custom hooks reutilizables (useAuth, useBookings, etc.)
├── layouts/         # Layouts de página (DashboardLayout, PublicLayout)
├── pages/           # Rutas y páginas principales
│   ├── Home.tsx
│   ├── ComplexesList.tsx
│   ├── ComplexDetail.tsx
│   ├── BookingCheckout.tsx
│   ├── Tournaments.tsx
│   └── Dashboard/
├── services/        # Clientes HTTP (Axios) para comunicarse con la API
├── store/           # Manejadores de estado global con Zustand
├── types/           # Interfaces y types TypeScript
└── utils/           # Formateo de fechas, moneda, validaciones
```

---

## 🚀 Comandos

```bash
# Instalar dependencias
npm install

# Modo desarrollo
npm run dev

# Compilar para producción
npm run build
```
