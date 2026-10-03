# 8. Análisis: consumo de energía y desperdicio de líquido

> **Pregunta guía.** ¿Cómo puede utilizarse la automatización industrial para monitorear los niveles de un tanque de líquido químico, reduciendo el consumo de energía y el desperdicio de líquido?

## 8.1 Qué aporta la solución implementada
| Riesgo operativo | Mecanismo de la solución | Efecto esperado |
|---|---|---|
| **Desbordamiento** del tanque (pérdida de producto químico) | El sensor B3 activa H3 (y, en el HMI, un texto de «DETENER LLENADO» con parpadeo) | Permite cortar el llenado antes de perder líquido |
| **Marcha en vacío** de equipos de bombeo | El estado «vacío» (H4) y «nivel bajo» (H2) informan que no hay producto suficiente | Evita operar bombas sin líquido (consumo innecesario de energía y desgaste) |
| **Llenados innecesarios o excesivos** | El estado «nivel correcto» (H1) indica el punto de operación | Se detiene el llenado al alcanzar el nivel requerido |
| **Decisiones basadas en lecturas falsas** | Toda combinación imposible activa H5 | Evita actuar sobre un sensor defectuoso (p. ej., creer que hay rebose o que está vacío por una falla) |
| **Dependencia de inspección manual** | Monitoreo continuo con ciclo de 20 ms (OpenPLC) | Respuesta inmediata a cualquier cambio de nivel |

## 8.2 Alcance
La solución **monitorea y señaliza**; no controla bombas ni válvulas porque el enunciado solicita únicamente las lámparas H1–H5. Su contribución al ahorro de energía y a la reducción del desperdicio es, por tanto, **habilitadora**: entrega la información fiable (incluida la detección de fallas de sensor) sobre la cual un operador o un lazo de control posterior puede detener el llenado y evitar el bombeo en vacío. No se midieron consumos ni volúmenes, por lo que no se presentan cifras de ahorro.

## 8.3 Líneas de trabajo futuro
- Conectar las salidas H3 y H4 a una válvula de llenado y a una bomba (corte automático por rebose y por vacío).
- Reportar los estados a un sistema de supervisión mediante Modbus/MQTT (enfoque IIoT) para registrar eventos y calcular indicadores de consumo y pérdidas [[1]](Referencias.md).
- Incorporar sensores de nivel reales (flotadores o capacitivos) y verificación formal del Ladder [[3]](Referencias.md).
