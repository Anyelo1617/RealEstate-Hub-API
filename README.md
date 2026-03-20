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
