# Semana 1: endpoint /api/version

## Objetivo

Agregar el endpoint GET /api/version, que devuelve la versión definida en app/proyecto.py, con su prueba en tests/test_inicio.py.

## Herramienta y modelo

Claude Code, modelo Sonnet 5.5 (en lugar de Copilot, por decisión del desarrollador).

## Prompt

Lee AGENTS.md. Agrega el endpoint GET /api/version, que devuelva la versión definida en app/proyecto.py, con su prueba en tests/test_inicio.py. Antes de escribir código, explícame qué archivos vas a modificar y espera mi aprobación.

## Resultado

El agente propuso modificar solo app/rutas/api.py y tests/test_inicio.py. Escribió primero la prueba, que falló con 404, y después la ruta, que devuelve proyecto.VERSION y no un número escrito a mano. Las 7 pruebas pasan y Ruff no reporta errores. No fue necesario corregir nada del cambio.
