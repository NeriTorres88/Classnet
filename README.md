# Módulo: Alumnos & Tutores — Plan de Trabajo

Este README resume el alcance del módulo de gestión de **alumnos**, **tutores** y su **vinculación** (alumno ↔ tutor), incluyendo API, UI, permisos (RBAC), pruebas y documentación.

---

## 📌 Alcance
- CRUD de **alumnos** y **tutores**.
- Vinculación/desvinculación **alumno ↔ tutor** con tipo de parentesco.
- **RBAC**: admin, tutor, docente.
- UI para administración (listas filtrables, formularios y modal de vinculación).
- Pruebas funcionales y documentación breve del módulo.

---

## 🗂️ Plan de trabajo

| # | Tarea                         | Descripción                                                                                                                                     | Responsable | Estimación (h) |
|---|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|-------------|----------------|
| 1 | Modelado BD y migraciones     | Crear tablas `students`, `tutors`, `student_tutor`; relaciones, tipo de parentesco, índices y llaves únicas (matrícula, correo).              | NERI        | 4              |
| 2 | Endpoints alumnos (CRUD)      | Crear/leer/actualizar/eliminar alumnos; incluir búsqueda, paginación y validaciones.                                                           | CARLOS      | 5              |
| 3 | Endpoints tutores y vínculo   | CRUD de tutores + lógica para **vincular**/**desvincular** alumno ↔ tutor.                                                                     | ALEXIS      | 5              |
| 4 | Autorización / RBAC           | Roles y permisos: **admin** (total), **tutor** (solo vinculados), **docente** (solo grupos).                                                   | NERI        | 3              |
| 5 | UI alumnos                    | Lista filtrable + formulario de alta/edición de alumnos.                                                                                       | GRACIELA    | 4              |
| 6 | UI tutores y vinculación      | Pantalla de tutores + **modal** para vincular/desvincular alumno ↔ tutor.                                                                      | GRACIELA    | 3              |
| 7 | Pruebas QA                    | Casos de prueba funcionales, exploratorias y regresión básica.                                                                                 | CARLOS      | 2              |
| 8 | Documentación breve           | Estructuras, endpoints y flujo general del módulo.                                                                                              | ALEJANDRA   | 1              |

**Total estimado:** **27 h**

---

## 🗃️ Modelo de datos (resumen)

- **students**
  - `id` (PK), `matricula` (UNIQUE), `nombre`, `apellido`, `correo` (UNIQUE), `telefono`, `estatus`, `created_at`, `updated_at`
- **tutors**
  - `id` (PK), `nombre`, `apellido`, `correo` (UNIQUE), `telefono`, `ocupacion`, `created_at`, `updated_at`
- **student_tutor**
  - `id` (PK), `student_id` (FK → students.id), `tutor_id` (FK → tutors.id), `parentesco` (ENUM: madre, padre, tutor_legal, otro), `principal` (bool), `created_at`
  - Índices compuestos: `(student_id, tutor_id)` UNIQUE

---

## 🔐 RBAC (Roles y Permisos)

- **admin**: acceso total al módulo.
- **tutor**: acceso solo a alumnos **vinculados**.
- **docente**: acceso de solo lectura a **grupos** y a listados autorizados.

---

## 🔗 Endpoints (bosquejo)

> Rutas y contratos podrán ajustarse según framework.

### Alumnos
- `GET /api/students?search=&page=&limit=` — listar + filtro + paginación
- `GET /api/students/{id}` — detalle
- `POST /api/students` — crear (validar matrícula/correo únicos)
- `PUT /api/students/{id}` — actualizar
- `DELETE /api/students/{id}` — eliminar

### Tutores
- `GET /api/tutors?search=&page=&limit=`
- `GET /api/tutors/{id}`
- `POST /api/tutors`
- `PUT /api/tutors/{id}`
- `DELETE /api/tutors/{id}`

### Vinculación alumno ↔ tutor
- `POST /api/students/{id}/tutors` — **vincular** `{ tutor_id, parentesco, principal }`
- `DELETE /api/students/{id}/tutors/{tutor_id}` — **desvincular**
- `GET /api/students/{id}/tutors` — listar tutores del alumno
- `GET /api/tutors/{id}/students` — listar alumnos del tutor

---

## ✅ Criterios de aceptación (DoD)

- Migraciones ejecutables y reversibles.
- Validaciones de negocio (matrícula y correo únicos, parentesco válido).
- Paginación y búsqueda funcionando en endpoints de alumnos y tutores.
- RBAC aplicado en todas las rutas protegidas.
- UI navegable: listas filtrables, formularios y modal de vinculación.
- Pruebas base pasando (funcionales y de regresión mínima).
- README y documentación técnica del módulo actualizados.

---

## 🧪 Pruebas (mínimas sugeridas)

- **Alumnos**: crear con matrícula/correo duplicados (debe fallar); búsqueda por nombre; paginación.
- **Tutores**: CRUD completo; búsqueda por correo/teléfono.
- **Vínculos**: crear vínculo válido; impedir duplicados `(student_id, tutor_id)`; marcar **principal** único por alumno.
- **RBAC**: tutor no puede ver alumnos no vinculados; docente con solo lectura.

---

## 📄 Documentación pendiente
- Diagrama ER definitivo (imagen).
- Ejemplos de payloads de request/response.
- Mapa de navegación de UI (wireframe o capturas).
