# 📱 Semana 20 — SnapTask

> **App móvil de gestión de tareas con Flutter y sincronización en la nube**

| Campo              | Detalle                |
| ------------------ | ---------------------- |
| 📅 Fechas          | 18-19 de julio 2026    |
| 🏷️ Categoría       | Full-Stack Integration |
| ⏱️ Tiempo estimado | 10-12 horas            |
| 📊 Dificultad      | ⭐⭐⭐ Intermedio      |

---

## 🎯 Descripción

SnapTask es una aplicación móvil de gestión de tareas construida con **Flutter**, diseñada para funcionar en Android e iOS desde una sola base de código. Incluye un backend con FastAPI para sincronización en la nube, notificaciones push y categorización inteligente de tareas.

Este proyecto demuestra la capacidad de extender habilidades full-stack al desarrollo móvil, manteniendo código limpio y reutilizable con Dart y el ecosistema de Flutter.

---

## ✨ Features

### Gestión de tareas

- [ ] Crear, editar y eliminar tareas con título, descripción y prioridad
- [ ] Categorías personalizables con colores e íconos
- [ ] Fechas límite con recordatorios locales
- [ ] Estados: pendiente, en progreso, completada
- [ ] Gestos de deslizamiento para acciones rápidas (completar, eliminar)

### Interfaz móvil nativa

- [ ] Navegación fluida con GoRouter
- [ ] Pantalla principal con lista de tareas agrupadas por fecha
- [ ] Pantalla de detalle de tarea con edición inline
- [ ] Dark mode / Light mode con toggle automático
- [ ] Animaciones suaves y transiciones Hero
- [ ] Pull-to-refresh para sincronización

### Sincronización en la nube

- [ ] Backend FastAPI con endpoints REST
- [ ] Autenticación con JWT (login/registro)
- [ ] Sincronización bidireccional de tareas
- [ ] Almacenamiento local con Hive/SharedPreferences (funciona offline)
- [ ] Resolución de conflictos (última escritura gana)

### Notificaciones

- [ ] Notificaciones push con Firebase Cloud Messaging (FCM)
- [ ] Recordatorios programados para tareas con fecha límite usando flutter_local_notifications
- [ ] Badge de la app con conteo de tareas pendientes

---

## 🛠️ Stack técnico

| Tecnología               | Propósito                                  |
| ------------------------ | ------------------------------------------ |
| **Flutter**              | Framework de desarrollo móvil              |
| **Dart**                 | Lenguaje de programación unificado         |
| **GoRouter**             | Navegación entre pantallas                 |
| **Provider / Riverpod**  | Gestión del estado                         |
| **Hive / SharedPreferences**| Almacenamiento local persistente        |
| **FCM / Local Notifications**| Notificaciones push y locales          |
| **FastAPI**              | Backend API REST                           |
| **PostgreSQL**           | Base de datos del backend                  |
| **JWT (python-jose)**    | Autenticación basada en tokens             |

---

## 📁 Estructura del proyecto

```text
snaptask/
├── mobile/                        # App Flutter
│   ├── lib/                       # Código fuente Dart
│   │   ├── screens/               # Pantallas principales
│   │   │   ├── home/
│   │   │   │   └── home_screen.dart   # Pantalla principal (lista tareas)
│   │   │   ├── categories/
│   │   │   │   └── categories_screen.dart # Gestión de categorías
│   │   │   ├── settings/
│   │   │   │   └── settings_screen.dart   # Configuración y perfil
│   │   │   ├── task/
│   │   │   │   └── task_detail_screen.dart# Detalle de tarea
│   │   │   └── auth/
│   │   │       ├── login_screen.dart      # Inicio de sesión
│   │   │       └── register_screen.dart   # Registro
│   │   ├── widgets/
│   │   │   ├── task_card.dart         # Tarjeta de tarea con gestos
│   │   │   ├── task_form.dart         # Formulario de tarea
│   │   │   ├── category_badge.dart    # Badge de categoría
│   │   │   └── empty_state.dart       # Estado vacío ilustrado
│   │   ├── providers/
│   │   │   ├── task_provider.dart     # Gestión de tareas
│   │   │   ├── auth_provider.dart     # Manejo de sesión
│   │   │   └── sync_provider.dart     # Lógica de sincronización
│   │   ├── services/
│   │   │   ├── api_service.dart       # Cliente HTTP (dio/http)
│   │   │   ├── storage_service.dart   # Almacenamiento local (Hive)
│   │   │   └── notification_service.dart# Config FCM/Locales
│   │   ├── theme/
│   │   │   └── app_theme.dart         # Paleta de colores (dark/light)
│   │   ├── models/
│   │   │   └── task_model.dart        # Modelos (clases de datos)
│   │   └── main.dart                  # Punto de entrada de la app
│   ├── pubspec.yaml                   # Dependencias de Flutter
│   └── README.md
├── backend/                       # API FastAPI
│   ├── app/
│   │   ├── main.py                # Entry point FastAPI
│   │   ├── models.py              # Modelos SQLAlchemy
│   │   ├── schemas.py             # Pydantic schemas
│   │   ├── routes/
│   │   │   ├── auth.py            # Endpoints de autenticación
│   │   │   └── tasks.py           # Endpoints CRUD de tareas
│   │   └── database.py            # Configuración de BD
│   ├── requirements.txt
│   └── Dockerfile
├── docker-compose.yml             # Backend + PostgreSQL
├── Makefile
└── README.md
```

---

## 🗓️ Plan del fin de semana

### Sábado

| Hora           | Actividad                                              |
| -------------- | ------------------------------------------------------ |
| 🌅 9:00-10:00  | Setup: Flutter, GoRouter, estructura del proyecto      |
| 🌅 10:00-12:00 | Pantalla principal: lista de tareas + TaskCard         |
| 🌞 12:00-13:00 | Backend FastAPI: modelos, auth JWT, CRUD tareas        |
| 🌞 14:00-16:00 | Pantalla de detalle + formulario de tarea              |
| 🌆 16:00-18:00 | Categorías, prioridades y gestos (Dismissible)         |

### Domingo

| Hora           | Actividad                                           |
| -------------- | --------------------------------------------------- |
| 🌅 9:00-10:30  | Sincronización: providers de sync, almacenamiento Hive |
| 🌅 10:30-12:00 | Dark mode + animaciones Hero y explícitas           |
| 🌞 13:00-14:30 | Notificaciones push + recordatorios programados     |
| 🌞 14:30-16:00 | Tests (flutter_test para widgets y lógica)          |
| 🌆 16:00-17:00 | README con capturas de pantalla y video demo        |

---

## ✅ Definición de "hecho"

- [ ] CRUD completo de tareas funcionando en la app móvil
- [ ] Navegación fluida entre pantallas (GoRouter)
- [ ] Sincronización con backend FastAPI
- [ ] Modo offline con Hive / SharedPreferences
- [ ] Dark mode y Light mode dinámico
- [ ] Transiciones cuidadas (animaciones en Flutter)
- [ ] Notificaciones locales para recordatorios
- [ ] Tests de widgets clave y lógica de negocio
- [ ] README con capturas de pantalla en ambos temas
- [ ] Docker Compose para el backend

---

## 🧠 Conceptos de Flutter demostrados

| Concepto                     | Aplicación en el proyecto                     |
| ---------------------------- | --------------------------------------------- |
| Widgets Funcionales          | Árbol de widgets modular en toda la app       |
| Gestión de estado            | Patrón Provider o Riverpod para el negocio    |
| Enrutamiento                 | Navegación declarativa con GoRouter           |
| Gestos interactivos          | Widget Dismissible para borrado/completado    |
| Animaciones de UI            | Composiciones de Hero, AnimatedContainer, etc.|
| Almacenamiento local         | SQLite / Hive para base de datos local        |
| Tipado estricto              | Dart puro y null-safety                       |
| Multihilo (Asincronía)       | Futures y Streams en la comunicación con APIs |
| Plugins nativos              | Manejo de hardware a nivel de notificaciones  |

---

## 💼 Lo que demuestra al reclutador

| Habilidad                  | Evidencia                                           |
| -------------------------- | --------------------------------------------------- |
| Desarrollo móvil           | App compilada funcional en Android/iOS (Flutter)    |
| Entendimiento UI/UX        | Experiencia responsiva y fluida nativa              |
| Full-stack mobile          | App móvil + Backend FastAPI + PostgreSQL            |
| Tipado estricto            | Uso idiomático de Dart y Python (Type hints)        |
| Arquitectura escalable     | Separación limpia: UI, Provider, Services, Models   |
| Modo Offline-first         | Funciona sin red y sincroniza cuando es posible     |
