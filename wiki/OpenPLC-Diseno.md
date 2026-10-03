# 5. OpenPLC — Diseño

**Herramienta:** OpenPLC Editor 4 · **Placa:** ESP32 WROOM (DevKit de 30 pines, puente USB-UART CP2102).
**Archivos:** [`openplc/OpenPLC_TanqueQuimico/`](../openplc/OpenPLC_TanqueQuimico/)

## 5.1 Enfoque
OpenPLC es una alternativa abierta a los PLC comerciales [[4]](Referencias.md), [[7]](Referencias.md). La literatura reciente señala que carece de mecanismos de seguridad por defecto [[2]](Referencias.md); en este prototipo de laboratorio, aislado de redes de producción, esto no se consideró crítico.

La lógica combinacional validada en CODESYS **no se rediseña**: se migra a OpenPLC con las mismas variables y ecuaciones. El proceso queda **controlado por sensores**: el programa se ejecuta en una tarea cíclica de 20 ms; en cada ciclo se leen B1, B2 y B3 y se recalculan las salidas, por lo que cualquier cambio en un sensor se refleja de inmediato en los LED.

## 5.2 Variables y direcciones
| Variable | Tipo | Dirección IEC | GPIO | Descripción |
|---|---|---|:-:|---|
| `B1_TankEmpty` | BOOL | `%IX0.0` | 4 | Sensor B1 (simulado con DIP switch) |
| `B2_MinLevel` | BOOL | `%IX0.1` | 16 | Sensor B2 |
| `B3_Overflow` | BOOL | `%IX0.2` | 17 | Sensor B3 |
| `H1_NivelOK` | BOOL | `%QX0.0` | 21 | LED nivel correcto |
| `H2_NivelBajo` | BOOL | `%QX0.1` | 19 | LED nivel bajo |
| `H3_NivelAlto` | BOOL | `%QX0.2` | 22 | LED nivel alto / rebose |
| `H4_TanqueVacio` | BOOL | `%QX0.3` | 18 | LED tanque vacío |
| `H5_Error` | BOOL | `%QX0.4` | 23 | LED error de sensor |

Los pines de entrada se declaran como *Digital Input* y los de salida como *Digital Output*. Cada variable tiene su dirección en la columna *Location*; sin este paso el programa se ejecuta pero no lee ni escribe el hardware.

**Pines evitados:** GPIO2, GPIO5 y GPIO15 (*strapping pins*, condicionan el arranque) y RX0/TX0 (reservados a la comunicación serie/programación).

![Placa seleccionada](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/openplc/placa_esp32_wroom.png)
*Figura 15. Configuración de la placa ESP32 WROOM en OpenPLC Editor.*

![Pin mapping](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/openplc/pin_mapping.png)
*Figura 16. Tabla de mapeo de pines: tipo, dirección IEC y alias de cada señal.*

## 5.3 Ladder
Cinco redes, cada una con comentario con la fila de la tabla de verdad que implementa:

| Red | Ecuación | Salida |
|:-:|---|---|
| 1 | `/B3 · /B2 · /B1` | `H4_TanqueVacio` |
| 2 | `/B3 · /B2 · B1` | `H2_NivelBajo` |
| 3 | `/B3 · B2 · B1` | `H1_NivelOK` |
| 4 | `B3 · B2 · B1` | `H3_NivelAlto` |
| 5 | `/H1 · /H2 · /H3 · /H4` | `H5_Error` |

La red 5 expresa literalmente el enunciado: *cualquier combinación que no corresponda a un estado válido es un error*. Equivale a `B3·(/B2+/B1) + /B3·B2·/B1` (Ladder de CODESYS) y usa solo contactos en serie. **Debe ir después de las redes 1–4**, porque lee sus salidas dentro del mismo ciclo de ejecución.

![Redes 1 y 2](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/openplc/ladder_red_1_y_2.png)
*Figura 17. Redes 1 y 2 del Ladder en OpenPLC Editor.*

![Red 3](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/openplc/ladder_red_3.png)
*Figura 18. Red 3 (nivel correcto).*

![Red 5](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/openplc/ladder_red_5.png)
*Figura 19. Red 5 (error de sensor).*

## 5.4 Verificación previa a la carga
Se evaluó el grafo del Ladder (`main.ld`) recorriendo las cinco redes para las 8 combinaciones de entrada; las salidas coincidieron con la tabla de verdad y con la ecuación de H5 de CODESYS.
