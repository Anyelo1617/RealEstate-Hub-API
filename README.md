# 🏢 Módulo 3 - Real Estate Hub API

**Full-Stack Single Page Application & REST API**
Evolución del portal inmobiliario hacia una arquitectura cliente-servidor completa. Este proyecto conecta el frontend moderno desarrollado en **React 19** con un backend robusto construido sobre **Node.js, Express y Prisma ORM**. Permite la gestión centralizada de propiedades abandonando el `localStorage` en favor de una base de datos relacional (SQLite).

---

## 💻 Stack Tecnológico

El proyecto se divide en dos capas, implementando las mejores prácticas y tecnologías modernas para cada entorno:

### Backend (API REST)
| Dependencia | Versión | Propósito |
| :--- | :--- | :--- |
| **Express** | 5.2.1 | Framework web minimalista para Node.js |
| **Prisma** | 7.2.0 | ORM Type-Safe de próxima generación |
| **TypeScript** | 5.9.3 | Tipado estático para código robusto |
| **Zod** | 4.1.9 | Validación de esquemas en tiempo de ejecución |
| **better-sqlite3** | 11.8.1 | Adaptador de alto rendimiento para SQLite |

### Frontend (SPA)
| Dependencia | Versión | Propósito |
| :--- | :--- | :--- |
| **React** | 19.2.1 | Biblioteca UI Core |
| **Vite** | 7.3.0 | Build tool & Dev Server ultrarrápido |
| **Tailwind CSS** | 4.1.8 | Framework de estilos (Motor v4 nativo) |
| **Shadcn UI** | Manual | Componentes UI reutilizables |

---

## 🧠 Conceptos Clave Implementados

Este módulo marca la transición a un entorno Full-Stack, cubriendo los siguientes conceptos arquitectónicos y prácticos:

* **Arquitectura Cliente-Servidor (CORS):** Separación clara entre el Frontend (Puerto 3001) y el Backend (Puerto 3002), comunicándose mediante peticiones HTTP asíncronas (`fetch`).
* **Diseño de API RESTful:** Estructuración de endpoints semánticos (GET, POST, PUT, DELETE) y respuestas estandarizadas (`{ success, data, meta, error }`).
* **Patrón Repository & Controller:** Separación de la lógica de enrutamiento, la lógica de negocio y la capa de acceso a datos.
* **Prisma ORM & SQLite:** Modelado de la base de datos mediante `schema.prisma`, migraciones dinámicas y uso de *Seeders* para inyectar datos de prueba.
* **Paginación Backend:** Manejo de grandes volúmenes de datos enviando solo fragmentos específicos (`page`, `limit`) en lugar de toda la colección.

---

## 🏗️ Arquitectura de la Aplicación

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                              ARQUITECTURA FULL-STACK                        │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│    ┌─────────────────────┐         ┌─────────────────────┐                  │
│    │      FRONTEND       │         │       BACKEND       │                  │
│    │    (React + Vite)   │         │  (Express + Prisma) │                  │
│    │    localhost:3001   │         │    localhost:3002   │                  │
│    └──────────┬──────────┘         └──────────┬──────────┘                  │
│               │                               │                             │
│               │  HTTP REST (fetch async)      │                             │
│               │  GET /api/properties?page=1   │                             │
│               └───────────────────────────────┤                             │
│                                               │                             │
│                                               ▼                             │
│                                    ┌─────────────────────┐                  │
│                                    │      DATABASE       │                  │
│                                    │      (SQLite)       │                  │
│                                    │   prisma/dev.db     │                  │
│                                    └─────────────────────┘                  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘

```

# 🚀 Instalación y Configuración

## Prerrequisitos
- Node.js 20.19+ o 22.12+
- npm 10+

> ⚠️ Importante: Necesitarás dos terminales abiertas simultáneamente.

---

## 1. Configurar el Backend (Terminal 1)

```bash
cd backend

# 1. Instalar dependencias (con legacy-peer-deps por Prisma 7)
npm install --legacy-peer-deps

# 2. Sincronizar el esquema con la base de datos
npx prisma db push

# 3. Poblar la base de datos con información de prueba
npm run db:seed

# 4. Iniciar el servidor backend (Correrá en el puerto 3002)
npm run dev
```

---

## 2. Configurar el Frontend (Terminal 2)

```bash
cd frontend

# 1. Instalar dependencias
npm install --legacy-peer-deps

# 2. Iniciar el servidor frontend (Correrá en el puerto 3001)
npm run dev
```

---

## 📂 Estructura del Directorio Principal

```text
module3-realestate-hub-api/
├── backend/                 # API REST (Express + Prisma)
│   ├── prisma/
│   │   ├── schema.prisma    # Modelado de Base de Datos
│   │   ├── seed.ts          # Script de inyección de datos
│   │   └── dev.db           # Archivo físico de SQLite
│   ├── src/
│   │   ├── controllers/     # Lógica de peticiones (ej. Paginación)
│   │   ├── middlewares/     # Interceptores (CORS, Errores)
│   │   ├── repositories/    # Consultas a base de datos
│   │   ├── routes/          # Definición de Endpoints
│   │   └── server.ts        # Punto de entrada Express
│   └── package.json
│
├── frontend/                # Aplicación Cliente (React)
│   ├── src/
│   │   ├── components/      # UI y Vistas
│   │   ├── pages/           # Rutas del SPA
│   │   ├── services/
│   │   │   └── api.ts       # Wrapper para llamadas Fetch a la API
│   │   └── main.tsx
│   └── package.json
└── README.md
```

---

## 📄 Parte 1: Paginación de Resultados y Metadatos (API)

El endpoint de listado de propiedades devolvía todos los registros de la base de datos a la vez, lo cual no es escalable ni eficiente para el frontend.

Se Implementó un sistema de paginación a través de parámetros de consulta (query params) y enriquecer la respuesta JSON con metadatos de navegación.

## Funcionalidades Implementadas

- **Extracción de Parámetros:**  
  Lectura y validación de `?page=` y `?limit=` desde la URL, asignando valores por defecto (página 1, 10 ítems) si no se proporcionan.

- **Paginación en Memoria:**  
  Implementación de `Array.slice()` en el controlador para segmentar los resultados de forma segura sin alterar la lógica de filtrado del repositorio.

- **Cálculo de Metadatos:**  
  Generación dinámica del total de registros (`total`) y cálculo matemático de las páginas disponibles (`pages`) usando `Math.ceil()`.

- **Respuesta Estructurada:**  
  Modificación del payload de salida para incluir el bloque de información `meta` junto al bloque `data`.

- **Soporte de Base de Datos:**  
  Actualización del motor de Prisma para soportar el adaptador `better-sqlite3` durante la inyección de datos de prueba (Seed).
---

```text
Link al video Explicativo:
```
https://youtu.be/Pvbm3bgOr50

# 📊 Parte 2: Property Statistics

### Implementación Realizada
Se desarrolló un endpoint de estadísticas que centraliza información agregada de las propiedades, permitiendo obtener insights de forma rápida y eficiente.

```text
Link al video Explicativo:
```
