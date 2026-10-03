# 2. CODESYS — Diseño del Ladder

**Herramienta:** CODESYS V3.5 SP22 Patch 3 [[8]](Referencias.md), con programación en Ladder según IEC 61131-3 [[5]](Referencias.md) · Dispositivo `CODESYS Control Win V3` (runtime de simulación) · `PLC_PRG` en lenguaje **Ladder (LD)**.
**Archivo:** [`codesys/EjercicioTanqueQuimico_HMI.project`](../codesys/EjercicioTanqueQuimico_HMI.project)

## 2.1 Variables de `PLC_PRG`
| Variable | Tipo | Función |
|---|---|---|
| `B1_TankEmpty` | BOOL | Sensor B1 (inferior): 1 = hay líquido en el fondo |
| `B2_MinLevel` | BOOL | Sensor B2 (medio): 1 = se alcanzó el nivel mínimo |
| `B3_Overflow` | BOOL | Sensor B3 (superior): 1 = nivel de rebose |
| `H1_NivelOK` | BOOL | Lámpara verde: nivel correcto |
| `H2_NivelBajo` | BOOL | Lámpara amarilla: nivel bajo |
| `H3_NivelAlto` | BOOL | Lámpara amarilla: nivel alto / rebose |
| `H4_TanqueVacio` | BOOL | Lámpara roja: tanque vacío |
| `H5_Error` | BOOL | Lámpara roja: error de sensor (señal incoherente) |
| `tonParpadeoOn`, `tonParpadeoOff` | TON | Temporizadores del reloj de parpadeo (500 ms cada uno) |
| `Parpadeo_500ms`, `Fase_OFF` | BOOL | Salidas del reloj de parpadeo |
| `H3_Parpadeo`, `H5_Parpadeo` | BOOL | Versiones intermitentes de H3 y H5, **solo para el HMI** |

## 2.2 Redes del Ladder
Cada red lleva un título y un comentario con la fila de la tabla de verdad y la ecuación que implementa.

| Red | Condición | Salida |
|:-:|---|---|
| 1 | `/B3 · /B2 · /B1` | `H4_TanqueVacio` |
| 2 | `/B3 · /B2 · B1` | `H2_NivelBajo` |
| 3 | `/B3 · B2 · B1` | `H1_NivelOK` |
| 4 | `B3 · B2 · B1` | `H3_NivelAlto` |
| 5 | `B3 · (/B2 + /B1)  +  /B3 · B2 · /B1` (ramas en paralelo = OR) | `H5_Error` |
| 6 | `/tonParpadeoOff.Q` → TON(500 ms) | `Parpadeo_500ms` (= `tonParpadeoOn.Q`) |
| 7 | `tonParpadeoOn.Q` → TON(500 ms) | `Fase_OFF` (reinicia ambos TON) |
| 8 | `H5_Error · Parpadeo_500ms` | `H5_Parpadeo` |
| 9 | `H3_NivelAlto · Parpadeo_500ms` | `H3_Parpadeo` |

![Ladder en línea, redes 1 a 3](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/ladder_01.png)
*Figura 1. Redes 1–3 en modo en línea (estado 000: se energiza la red de H4).*

![Ladder en línea, redes 3 a 5](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/ladder_02.png)
*Figura 2. Redes 3–5.*

![Red de error H5 con rama en paralelo](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/ladder_03.png)
*Figura 3. Red 5: la rama en paralelo implementa el OR de las condiciones de error.*

![Reloj de parpadeo con TON](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/ladder_04.png)
*Figura 4. Redes 6–7: reloj de parpadeo con dos temporizadores TON de 500 ms (se observa el tiempo transcurrido `ET`).*

![Redes de parpadeo para el HMI](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/ladder_05.png)
*Figura 5. Redes 8–9: lámparas intermitentes de error y rebose (solo HMI).*

## 2.3 Decisiones de diseño

1. **Una sola bobina para H5.** El enunciado define una única lámpara de error. Una versión inicial usaba dos bobinas (`H5_Error` y `H5_Errormin`) porque CODESYS no admite escribir la misma bobina en dos redes; se resolvió reuniendo las condiciones en **una sola red con ramas en paralelo**, de modo que H5 cubre las cuatro combinaciones inválidas.
2. **H3 exige los tres sensores.** En una versión previa H3 solo evaluaba `B3`, lo que contradecía la condición de error (rebose con sensores inferiores incoherentes). Se corrigió a `B3 · B2 · B1`.
3. **Variables unificadas Ladder–HMI.** Un defecto temprano dejó el HMI escribiendo variables (`B1_TankEmpty`…) distintas de las que leía el Ladder (`B1`…), por lo que H4 permanecía encendida. Se unificaron los nombres; la lección es verificar que programa y visualización usen *exactamente* las mismas variables.
4. **Elemento controlado por tiempo.** El proceso es combinacional, pero se añadió un reloj de 1 s (dos TON encadenados: 500 ms en cada fase) para que el HMI haga **parpadear** las lámparas de alarma (H3 y H5). Las salidas lógicas `H3_NivelAlto` y `H5_Error` no cambian: el parpadeo vive en variables auxiliares (`H3_Parpadeo`, `H5_Parpadeo`), de modo que el Ladder que se migra a OpenPLC permanece idéntico.
5. **Documentación.** Cada red lleva título y comentario con la fila de verdad y la ecuación, y cada variable tiene su comentario de declaración.
