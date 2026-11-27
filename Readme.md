# PoliRestaurante — Sistema de Gestión de Pedidos

Sistema web para la gestión digital de pedidos del restaurante PoliRestaurante, desarrollado como proyecto académico de **Ingeniería de Software II** en el Politécnico Grancolombiano.

El sistema centraliza la toma, preparación y entrega de pedidos y comunica casi en tiempo real a meseros, cocina y administración, reemplazando las comandas en papel y la comunicación verbal entre salón y cocina.

El proyecto cubre el ciclo de vida completo del software: análisis, diseño, construcción, pruebas y despliegue.

---

## Roles del sistema

| Rol | Qué hace |
|---|---|
| **Mesero** | Crea pedidos de salón o para llevar, consulta sus pedidos activos, los marca como entregados y los cancela con un motivo. |
| **Cocina** | Ve los pedidos en un tablero por estado, los toma para prepararlos y los marca como listos. |
| **Administrador** | Supervisa todos los pedidos en tiempo real y gestiona productos, categorías, mesas y personal. |

## Flujo de los pedidos

```
PENDIENTE → EN PREPARACIÓN → LISTO → ENTREGADO
     └──────────┴──→ CANCELADO (con motivo obligatorio)
```

- Un pedido solo se puede **cancelar** mientras está en PENDIENTE o EN PREPARACIÓN.
- Las transiciones permitidas para cada rol están guardadas en la base de datos (tabla `transiciones_estado`), no en el código.
- Cada cambio de estado registra su **marca de tiempo** (`fecha_preparacion`, `fecha_listo`, `fecha_entrega`, `fecha_cancelacion`) para analizar tiempos de atención.
- Si dos usuarios cambian el mismo pedido al mismo tiempo, solo uno lo logra; el otro recibe un aviso (HTTP 409).

## Tecnologías

| Capa | Tecnología |
|---|---|
| Frontend | React 19 + Vite |
| Backend | Node.js 22 + Express 5 |
| Base de datos | PostgreSQL + Sequelize (ORM) |
| Seguridad | JWT (sesión de 30 min) + bcrypt |
| Tiempo real | Consulta periódica cada 2 segundos |

## Estructura del repositorio

```
Polirestaurante-Project/
├── backend/     API REST (Node.js + Express + Sequelize) → ver backend/back.md
├── frontend/    Aplicación web (React + Vite)            → ver frontend/front.md
├── docs/        Análisis, diseño, despliegue y pruebas
└── tests/       Pruebas del proyecto
```

## Ejecución rápida en local

Requisitos: **Node.js 22**, **npm** y **PostgreSQL 14 o superior**.

```bash
# 1. Clonar el repositorio
git clone https://github.com/IngManuelFonseca/Polirestaurante-Project.git
cd Polirestaurante-Project

# 2. Backend (terminal 1)
cd backend
cp .env.example .env      # completar datos de PostgreSQL, JWT y administrador inicial
npm install
npm run dev               # http://localhost:5000

# 3. Frontend (terminal 2)
cd frontend
cp .env.example .env      # VITE_API_URL=http://localhost:5000/api
npm install
npm run dev               # http://localhost:5173
```

Luego abra http://localhost:5173 e inicie sesión con el correo y la contraseña del administrador inicial definidos en `backend/.env`. Los pasos detallados están en [`backend/back.md`](backend/back.md) y [`frontend/front.md`](frontend/front.md).

> En la versión 1.0.0 el sistema se ejecuta en local; no está publicado en internet.

## Fases del proyecto

- **Análisis:** requisitos, historias de usuario, casos de uso y diagrama de estados.
- **Diseño:** modelo entidad-relación, diagrama de clases, arquitectura y wireframes.
- **Construcción:** módulos de mesero, cocina y administración.
- **Pruebas:** pruebas unitarias, pruebas manuales y trazabilidad con las historias de usuario.
- **Despliegue:** manuales técnico y de usuario y demostración funcional.

## Documentación

- Manual Técnico del Sistema (v1.0): arquitectura, modelo de datos, API, instalación, seguridad y pruebas.
- Manual de Usuario del Sistema (v1.0): uso paso a paso por rol, con capturas de pantalla.
- Documento del proyecto: historias de usuario, casos de uso, BPMN, requisitos no funcionales y diagramas.

## Equipo

| Integrante | Rol |
|---|---|
| Daniel Felipe Cortes | Scrum Master |
| Juan Andrés Guerrero Rodríguez | Product Owner |
| Manuel Alejandro Fonseca Riaño | Líder de configuración y pruebas |
| Sebastián Zarate Pinilla | Líder de desarrollo |

**Docente:** Ing. Yamid Ramírez Sánchez

---

Proyecto desarrollado para el Politécnico Grancolombiano — Facultad de Ingeniería, Diseño e Innovación, 2026.
