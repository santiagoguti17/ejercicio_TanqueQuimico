# 7. OpenPLC — Validación con hardware

Se validó el programa de OpenPLC sobre el prototipo descrito en [Montaje físico](Montaje-Fisico.md): ESP32 WROOM, DIP switch como sensores y cinco LED como lámparas. Convención: switch ON = 1 = nivel de 3,3 V en el GPIO.

## 7.1 Evidencia fotográfica de los estados

![Nivel bajo](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/montaje/estado_nivel_bajo.jpg)
*Figura 20. Nivel bajo (B3 B2 B1 = 001): se enciende únicamente el LED de H2.*

![Nivel correcto](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/montaje/estado_nivel_correcto.jpg)
*Figura 21. Nivel correcto (B3 B2 B1 = 011): se enciende el LED verde de H1.*

![Rebose](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/montaje/estado_rebose.jpg)
*Figura 22. Rebose (B3 B2 B1 = 111): se enciende únicamente el LED de H3.*

![Error 1](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/montaje/estado_error_1.jpg)
*Figura 23. Error de sensor, caso 1: se enciende el LED rojo de H5 (combinación inválida).*

![Error 2](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/montaje/estado_error_2.jpg)
*Figura 24. Error de sensor, caso 2: se enciende el LED rojo de H5 (otra combinación inválida).*

## 7.2 Protocolo de pruebas

### Prueba 1 — Tabla de verdad (8 combinaciones)
| # | B3 | B2 | B1 | LED esperado | LED observado | ¿Correcto? |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| 1 | 0 | 0 | 0 | H4 (vacío) | | |
| 2 | 0 | 0 | 1 | H2 (bajo) | | |
| 3 | 0 | 1 | 0 | H5 (error) | | |
| 4 | 0 | 1 | 1 | H1 (correcto) | | |
| 5 | 1 | 0 | 0 | H5 (error) | | |
| 6 | 1 | 0 | 1 | H5 (error) | | |
| 7 | 1 | 1 | 0 | H5 (error) | | |
| 8 | 1 | 1 | 1 | H3 (rebose) | | |

### Prueba 2 — Llenado y vaciado
Secuencia: 000 → 001 → 011 → 111 → 011 → 001 → 000. Esperado: H4, H2, H1, H3, H1, H2, H4.

| Paso | Combinación | Esperado | Observado |
|:-:|:-:|:-:|:-:|
| 1 | 000 | H4 | |
| 2 | 001 | H2 | |
| 3 | 011 | H1 | |
| 4 | 111 | H3 | |
| 5 | 011 | H1 | |
| 6 | 001 | H2 | |
| 7 | 000 | H4 | |

### Prueba 3 — Transiciones con error
Activar el sensor superior sin los inferiores (000 → 010 → 000; 000 → 100 → 000). Esperado: H5 mientras dure la combinación inválida y retorno a H4 en 000. **Observado:** ______

### Prueba 4 — Estabilidad de las entradas
Con todos los switches en OFF durante 1 minuto: H4 fijo, sin parpadeos ni otro LED encendido. **Observado:** ______

### Prueba 5 — Arranque en frío
Reconectar el USB con la combinación 011 (debe encender H1) y con 000 (debe encender H4). **Observado:** ______

### Prueba 6 — Verificación eléctrica
| Medición | Esperado | Medido |
|---|:-:|:-:|
| GPIO4/16/17 con switch OFF | ≈ 0 V | |
| GPIO4/16/17 con switch ON | ≈ 3,3 V | |
| Tensión GPIO → resistencia con LED encendido | ≈ 3,3 V | |
| Continuidad de GND común | Sí | |

## 7.3 Resultado esperado y criterio de aceptación
Se considera aprobada la validación si, en las pruebas 1 y 2, **exactamente un LED** se enciende por combinación y coincide con la tabla de verdad, y si en las pruebas 4 y 5 no aparecen cambios espontáneos de estado.

## 7.4 Correspondencia con la simulación
Para cada combinación, el LED que se enciende en el prototipo corresponde a la lámpara que se activa en el HMI de CODESYS ([validación en simulación](CODESYS-Validacion.md)), pues ambos ejecutan la misma lógica.
