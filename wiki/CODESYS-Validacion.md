# 4. CODESYS — Validación en simulación

**Entorno:** CODESYS V3.5 SP22 Patch 3 · runtime de simulación (*Online → Simulación*) · aplicación en estado **EN EJECUCIÓN**.
**Compilación:** 0 errores, 0 advertencias.

## 4.1 Procedimiento
1. *Compilar* la aplicación y verificar que no haya errores.
2. *Login*, iniciar la ejecución y abrir `Visualization`.
3. Accionar los botones B1, B2 y B3 para generar cada combinación y registrar la lámpara activa, el texto de estado y el nivel del tanque.
4. Verificar las 8 combinaciones directamente sobre las variables de `PLC_PRG` (forzado de entradas en línea y lectura de salidas).

## 4.2 Resultado: las 8 combinaciones
| B3 | B2 | B1 | Esperado | H1 | H2 | H3 | H4 | H5 | Resultado |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| 0 | 0 | 0 | H4 | 0 | 0 | 0 | 1 | 0 | ✔ |
| 0 | 0 | 1 | H2 | 0 | 1 | 0 | 0 | 0 | ✔ |
| 0 | 1 | 0 | H5 | 0 | 0 | 0 | 0 | 1 | ✔ |
| 0 | 1 | 1 | H1 | 1 | 0 | 0 | 0 | 0 | ✔ |
| 1 | 0 | 0 | H5 | 0 | 0 | 0 | 0 | 1 | ✔ |
| 1 | 0 | 1 | H5 | 0 | 0 | 0 | 0 | 1 | ✔ |
| 1 | 1 | 0 | H5 | 0 | 0 | 0 | 0 | 1 | ✔ |
| 1 | 1 | 1 | H3 | 0 | 0 | 1 | 0 | 0 | ✔ |

En todas las combinaciones se enciende **una sola** salida. Se verificó además que `H5_Parpadeo` conmuta con periodo de 1 s mientras `H5_Error` permanece fija.

## 4.3 Comprobación en el HMI
| Combinación (B3 B2 B1) | Estado | Lámpara | Texto de estado | Nivel del tanque |
|:-:|---|:-:|---|---|
| 000 | Vacío | H4 (rojo) | TANQUE VACÍO | Mínimo |
| 001 | Nivel bajo | H2 (amarillo) | NIVEL BAJO | Bajo |
| 011 | Nivel correcto | H1 (verde) | NIVEL CORRECTO | Medio |
| 111 | Rebose | H3 (amarillo, intermitente) | REBOSE: ¡DETENER LLENADO! | Lleno |
| 010, 110, 100 | Error de sensor | H5 (rojo, intermitente) | ERROR DE SENSOR: REVISAR B1/B2/B3 | Oculto |

![Vacío](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/hmi_01_vacio.png)
*Figura 7. Estado 000 — tanque vacío (H4).*

![Nivel bajo](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/hmi_02_bajo.png)
*Figura 8. Estado 001 — nivel bajo (H2).*

![Nivel correcto](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/hmi_03_correcto.png)
*Figura 9. Estado 011 — nivel correcto (H1).*

![Rebose](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/hmi_04_rebose.png)
*Figura 10. Estado 111 — rebose (H3).*

![Error 010](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/hmi_05_error_010.png)
*Figura 11. Estado 010 — error de sensor (H5): B2 activo sin B1.*

![Error 110](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/hmi_06_error_110.png)
*Figura 12. Estado 110 — error de sensor (H5): B3 y B2 activos sin B1.*

![Error 100](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/hmi_07_error_100.png)
*Figura 13. Estado 100 — error de sensor (H5): B3 activo sin B2 ni B1.*

![Entorno CODESYS en línea](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/ide_ladder_en_linea.png)
*Figura 14. Entorno CODESYS en línea (simulación): monitorización de variables y de la red de H4 energizada.*

## 4.4 Conclusión
El HMI se conecta correctamente con la lógica Ladder: las transiciones entre los cuatro estados válidos y el estado de error ocurren según lo diseñado, y las lámparas, el nivel del tanque y el texto de estado responden a las variables `H1…H5`.
