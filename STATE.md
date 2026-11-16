# STATE.md

Estado del proyecto. Léalo al iniciar cada sesión de trabajo y actualícelo al cerrarla.

## Estado actual

Repositorio publicado en GitHub con el CI en verde. Página de inicio, /salud, /docs y /api/version funcionando en https://plataforma-ludica-9f75.onrender.com. El repositorio tiene AGENTS.md, STATE.md, permisos de OpenCode en opencode.json, secretos en .env (fuera de git) y un hook de pre-commit que ejecuta Ruff y las pruebas. Hay 7 pruebas pasando.

## Decisiones

| Fecha | Decisión | Motivo |
| --- | --- | --- |
| 2026-11-16 | Usar la plantilla del curso con FastAPI | Es el stack definido para el proyecto |
| 2026-11-16 | Trabajar cada tarea en una rama con pull request | Ningún cambio llega a main sin el CI en verde |
| 2026-11-16 | Escribir la prueba antes del código | Lo indica AGENTS.md y confirma que la prueba detecta el fallo |
| 2026-11-16 | La ruta /api/version lee proyecto.VERSION | La versión se cambia en un solo lugar |

## Siguiente paso

Definir en specs/spec.md las historias de usuario del curso de Matemáticas con la mecánica Niveles, incluida la regla de desbloqueo con 80 % de aciertos, y escribir su prueba en tests/aceptacion/.
