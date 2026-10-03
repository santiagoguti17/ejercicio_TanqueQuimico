# 9. Uso de inteligencia artificial

## 9.1 Declaración
En este trabajo se utilizó un asistente de IA generativa como herramienta de apoyo: **Claude Code** (Anthropic), con el modelo **Claude Sonnet 5.5**, ejecutado en la terminal del equipo del autor con acceso a la carpeta del proyecto. La herramienta **generó y modificó artefactos del proyecto** (Ladder y HMI de CODESYS, proyecto de OpenPLC, capturas de pantalla y borradores de la documentación). El autor decidió el alcance, aportó el montaje físico y las pruebas con hardware, y es responsable del contenido entregado.

## 9.2 Distribución del trabajo
| Actividad | Asistente de IA | Autor |
|---|---|---|
| Análisis del enunciado y tabla de verdad | Propuso la revisión contra la rúbrica | Definió la lógica y las decisiones de diseño (p. ej., simplificar o descartar elementos) |
| Ladder y HMI en CODESYS | Revisó el proyecto, propuso mejoras y las implementó mediante scripts de la API de CODESYS (unificación de H5, reloj de parpadeo, etiquetas, texto de estado) | Construyó el proyecto base, validó el funcionamiento en el entorno |
| Validación en simulación | Ejecutó pruebas automatizadas sobre las variables en línea y capturó pantallas | Revisó los resultados en el simulador |
| Proyecto OpenPLC | Generó el programa Ladder y el mapeo de pines a partir de la lógica validada, y verificó la lógica con un evaluador del grafo | Importó el proyecto en OpenPLC Editor, lo cargó en la ESP32 |
| Montaje físico y pruebas con hardware | — | Armó el circuito, resolvió el problema de entradas flotantes (pull-down), ejecutó y fotografió las pruebas |
| Documentación (esta Wiki) | Redactó borradores y organizó la estructura según la rúbrica | Revisión, correcciones y publicación |

## 9.3 Solicitudes realizadas a la herramienta (resumen)
Las solicitudes se presentan **resumidas y redactadas de forma clara**; no son transcripciones literales.

1. *Evaluar el proyecto CODESYS «TanqueQuimico» con la rúbrica del curso, identificar carencias y mejorarlo sin salirse del enunciado, documentando los cambios para la Wiki.*
2. *Implementar el proyecto en OpenPLC a partir de la lógica validada y de la configuración del montaje con ESP32 WROOM (pines y cableado), cumpliendo la rúbrica de OpenPLC.*
3. *Cargar el programa en la ESP32 y proponer pruebas físicas para verificarlo.*
4. *Elaborar la Wiki del trabajo con capturas de CODESYS y OpenPLC, evidencia fotográfica del montaje, referencias en formato IEEE y esta declaración de uso de IA.*

## 9.4 Verificación y límites
- Las salidas de la IA se comprobaron con: compilación en CODESYS (0 errores y 0 advertencias), pruebas de las 8 combinaciones en simulación y evaluación del grafo Ladder de OpenPLC.
- La IA **no** pudo operar la interfaz de OpenPLC Editor para compilar y cargar el programa; esa parte la realizó el autor.
- El HMI y la documentación fueron generados parcialmente por la IA y requieren revisión humana; cualquier error residual es responsabilidad del autor.
- Las referencias [1]–[3] se localizaron mediante búsqueda web y se verificaron en arXiv; las demás corresponden a normas, hojas de datos y documentación de los fabricantes.
