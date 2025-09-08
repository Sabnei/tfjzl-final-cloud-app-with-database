## Final Project – Online Course App: Nueva funcionalidad de evaluación

Este proyecto corresponde al "Final Project: Add a New Assessment Feature to an Online Course App" del curso "Django Application Development with SQL and Databases" (Coursera). Sobre la app existente `onlinecourse` se implementa una nueva funcionalidad de evaluación (quizzes) que permite a los estudiantes responder preguntas, enviar sus respuestas y ver su calificación.

### Requisitos
- **Python**: 3.8 (ver `runtime.txt`)
- **Django**: 4.2.3
- Otras dependencias en `requirements.txt` (incluye `gunicorn`, `Pillow`, etc.)

### Instalación y ejecución local
1. Crear y activar entorno virtual (recomendado):
   - Windows PowerShell:
     ```bash
     python -m venv .venv
     .\.venv\Scripts\Activate.ps1
     ```
   - macOS/Linux:
     ```bash
     python3 -m venv .venv
     source .venv/bin/activate
     ```
2. Instalar dependencias:
   ```bash
   pip install -r requirements.txt
   ```
3. Preparar base de datos (SQLite por defecto):
   ```bash
   python manage.py migrate
   ```
4. Crear superusuario (opcional, para acceder al admin):
   ```bash
   python manage.py createsuperuser
   ```
5. Ejecutar el servidor de desarrollo:
   ```bash
   python manage.py runserver
   ```
6. Abrir en el navegador: `http://127.0.0.1:8000/` (la app `onlinecourse` se monta en la raíz).

### Funcionalidad de evaluación (assessment)
La app incorpora preguntas y opciones de respuesta asociadas a cada curso. El flujo principal es:
- Listar cursos y ver el detalle de un curso.
- Inscribirse en el curso (`Enroll`).
- Responder el examen del curso y enviar respuestas (`Submit`).
- Ver el resultado y la calificación obtenida.

Endpoints relevantes (`onlinecourse/urls.py`):
- `""` → lista de cursos.
- `"<int:pk>/"` → detalle del curso.
- `"<int:course_id>/enroll/"` → inscripción al curso.
- `"<int:course_id>/submit/"` → envío de respuestas del examen.
- `"course/<int:course_id>/submission/<int:submission_id>/result/"` → resultado del examen.

En `onlinecourse/views.py` se implementan:
- `submit`: crea un `Submission` para el `Enrollment` del usuario, recoge las opciones seleccionadas y redirige a resultados.
- `show_exam_result`: calcula la puntuación comparando las opciones seleccionadas con las correctas y renderiza la vista de resultados.

### Modelado y ERD
El modelo sigue la guía del diagrama entidad–relación provisto por el curso (Curso → Pregunta → Opción; Usuario ↔ Inscripción ↔ Envío). Referencia visual del ERD:
`https://github.com/ibm-developer-skills-network/final-cloud-app-with-database/blob/master/static/media/course_images/onlinecourse_app_er.png`

### Panel de administración
Acceso en `http://127.0.0.1:8000/admin/` con el superusuario creado. Desde allí puedes gestionar cursos, preguntas, opciones, etc.

### Archivos de despliegue
- `Procfile`: define el comando de arranque con `gunicorn` (para despliegues en PaaS).
- `manifest.yml`: ejemplo de despliegue en IBM Cloud Foundry.
- `runtime.txt`: versión de Python sugerida.

### Despliegue (ejemplo IBM Cloud Foundry)
1. Autenticarse y seleccionar organización/espacio en IBM Cloud CLI.
2. Ejecutar:
   ```bash
   cf push
   ```
   El `manifest.yml` define nombre de la app, memoria, buildpack, etc.

Notas de estáticos:
- En entornos productivos ejecuta `python manage.py collectstatic` y asegúrate de servir archivos estáticos adecuadamente.

### Pruebas rápidas manuales
1. Crear superusuario y acceder a `/admin/`.
2. Configurar un curso con preguntas y opciones (marcando las correctas).
3. Desde la UI pública, inscribirse, responder y enviar el examen.
4. Verificar la calificación en la página de resultado.

### Estructura relevante
- `myproject/` → configuración del proyecto Django.
- `onlinecourse/` → app principal: modelos, vistas, rutas y plantillas.
- `static/` → archivos estáticos (incluye recursos del curso).

### Licencia
Este repositorio se distribuye bajo la licencia incluida en `LICENSE`.
