# Mini-aplicación de práctica: Trivia de cultura general

Repositorios: mini-app-a (versión A) y mini-app-b (versión B y mejora), en la cuenta brandonxf de GitHub.

## Versión A

**Objetivo:** construir la mini-aplicación con un prompt de una línea.
**Herramienta y modelo:** Claude Code con el modelo Sonnet 5.5 (el instructor aceptó Claude Code en lugar de Copilot).
**Prompt:**

```
Haz una app web de trivia con FastAPI.
```

**Resultado:** un solo mensaje. La aplicación arranca y la página funciona, pero el agente eligió por su cuenta rutas y campos en inglés (/api/questions, /api/answer) y puso todo en main.py. Pruebas de aceptación aprobadas: 0 de 6 (ModuleNotFoundError: No module named 'app'). Cumple 1 de 4 requisitos del anexo. Ruff reportó 1 error, en el archivo de pruebas (orden de imports), y ninguno en el código del agente.

## Versión B

**Objetivo:** construir la misma aplicación con un prompt estructurado, AGENTS.md y pruebas de aceptación.
**Herramienta y modelo:** Claude Code con el modelo Sonnet 5.5.
**Prompt:**

```
## Rol
Actúa como desarrollador backend con experiencia en FastAPI, Jinja2 y pruebas con pytest.

## Objetivo
Construir la mini-aplicación web Trivia de cultura general con estos requisitos:
1. GET / muestra una página que presenta las preguntas una por una, con cuatro botones de opción, indica si la respuesta fue correcta y muestra el puntaje acumulado.
2. GET /api/preguntas devuelve una lista de 5 preguntas de cultura general. Cada pregunta tiene los campos id (entero), texto (texto) y opciones (lista de 4 textos). La respuesta correcta no se incluye.
3. POST /api/responder recibe {"id": 1, "opcion": 2}, donde opcion es un índice de 0 a 3, y devuelve {"correcta": true o false, "respuesta_correcta": índice de 0 a 3}.
4. Si el id no existe, responde con el código 404. Si la opción no está entre 0 y 3, responde con el código 422.

## Contexto
Lee AGENTS.md. El contrato de la API está en tests/test_aceptacion.py: las rutas, los nombres de los campos y los códigos de estado deben coincidir con esas pruebas.

## Restricciones
- Usa solo las dependencias de requirements.txt.
- Define las rutas en app/main.py y la lógica en app/servicios.py, sin importar FastAPI en servicios.
- Guarda los datos en memoria, sin base de datos ni archivos.
- No modifiques tests/test_aceptacion.py.
- La página usa HTML, CSS y JavaScript sin librerías externas.

## Formato de salida
Antes de escribir código, muestra el plan con las rutas completas de los archivos que vas a crear y espera mi aprobación. Al terminar, resume en una línea qué hace cada archivo.

## Ejemplo
GET /api/preguntas devuelve: [{"id": 1, "texto": "¿Cuál es el planeta más grande del sistema solar?", "opciones": ["Marte", "Júpiter", "Saturno", "Venus"]}, ...]

POST /api/responder con {"id": 1, "opcion": 1} devuelve: {"correcta": true, "respuesta_correcta": 1}

## Criterio de éxito
python -m pytest -v pasa todas las pruebas y ruff check . no reporta errores. Si después de 3 intentos las pruebas no pasan, detente y explica el problema.
```

**Resultado:** dos mensajes: el prompt y «Aprobado, procede.». El plan nombró solo los archivos esperados y el agente los escribió sin tocar las pruebas ni las dependencias. No ejecutó las pruebas porque no se le autorizó el comando, y se verificaron a mano: 6 de 6 pruebas de aceptación aprobadas, Ruff sin errores y 4 de 4 requisitos cumplidos. No hizo falta ningún reintento.

## Mejora

**Objetivo:** agregar el parámetro opcional cantidad (de 1 a 5) a GET /api/preguntas, con selector en la página.
**Herramienta y modelo:** Claude Code con el modelo Sonnet 5.5.
**Prompt:**

```
## Rol
Actúa como desarrollador backend con experiencia en FastAPI, Jinja2 y pruebas con pytest.

## Objetivo
Agregar a la trivia el parámetro opcional cantidad en GET /api/preguntas. Acepta un entero entre 1 y 5 y devuelve solo esa cantidad de preguntas; sin el parámetro devuelve 5, y con un valor fuera del rango responde con el código 422. La página principal también debe permitir elegir cuántas preguntas se juegan (de 1 a 5) antes de empezar, y usar ese valor al pedir las preguntas.

## Contexto
Lee AGENTS.md, app/main.py, app/servicios.py, app/templates/index.html y tests/test_aceptacion.py. Las pruebas de aceptación actuales deben seguir pasando sin cambios.

## Restricciones
- No modifiques tests/test_aceptacion.py.
- Escribe la prueba nueva en tests/test_mejora.py. Debe comprobar el contenido de la respuesta (cantidad de preguntas y que no se revele la respuesta correcta), no solo el código de estado.
- Usa solo las dependencias de requirements.txt.
- Las rutas van en app/main.py y la lógica en app/servicios.py, sin importar FastAPI en servicios.
- La página usa HTML, CSS y JavaScript sin librerías externas.
- No hagas commit ni push.

## Formato de salida
Antes de escribir código, muestra el plan con las rutas completas de los archivos que vas a modificar o crear y espera mi aprobación. Al terminar, resume en una línea qué cambió en cada archivo.

## Ejemplo
GET /api/preguntas?cantidad=2 devuelve una lista con 2 preguntas, cada una con id, texto y opciones.

GET /api/preguntas devuelve 5 preguntas.

GET /api/preguntas?cantidad=9 responde con el código 422.

## Criterio de éxito
python -m pytest -v pasa todas las pruebas, las de aceptación y las de la mejora, y ruff check . no reporta errores. Si después de 3 intentos las pruebas no pasan, detente y explica el problema.
```

**Resultado:** el plan incluyó los cuatro archivos esperados, y el agente modificó app/main.py, app/servicios.py y app/templates/index.html, y creó tests/test_mejora.py con 12 pruebas que comprueban el contenido de la respuesta. Pasan las 18 pruebas (6 de aceptación y 12 de la mejora) y Ruff no reporta errores. No hubo que corregir nada; la mejora se comprobó también en el navegador.

## Comparación

| Criterio | Versión A | Versión B |
| --- | --- | --- |
| Modelo usado | Claude Code, Sonnet 5.5 | Claude Code, Sonnet 5.5 |
| Mensajes enviados al agente | 1 | 2 |
| La aplicación arranca (sí o no) | Sí | Sí |
| Requisitos del anexo que cumple (X de N) | 1 de 4 | 4 de 4 |
| Pruebas de aceptación aprobadas (X de N) | 0 de 6 | 6 de 6 |
| Errores de Ruff | 1 (solo en el archivo de pruebas) | 0 |
| Separa rutas y lógica (sí o no) | No | Sí |
| Dependencias que no estaban indicadas | 2 (creó su propio requirements.txt) | 0 |
| Decisiones que el agente tomó por su cuenta | Rutas y campos en inglés, 12 preguntas al azar, no validar la opción, todo en main.py | Puntaje en el navegador y una excepción propia para el id inexistente |

**Conclusión:** Lo que más diferencia a la versión B de la A son las pruebas de aceptación. En la versión A el agente no sabía qué rutas ni qué nombres esperaban las pruebas, entonces creó rutas en inglés y falló porque no sabía bien lo que iba a hacer. En la versión A el agente decidió por su cuenta cosas que no servían: dejó el main.py suelto, pero las pruebas buscaban la carpeta app, y puso las rutas en inglés, entonces cuando las pruebas pedían /api/preguntas la app respondía 404. La página funcionaba, pero no cumplía lo que se pedía. La próxima vez crearía un archivo .md explicándole todo antes de ponerlo a trabajar, para que esté más claro de lo que tiene que hacer.
