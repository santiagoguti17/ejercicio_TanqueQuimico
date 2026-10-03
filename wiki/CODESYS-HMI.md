# 3. CODESYS — HMI (visualización `Visualization`)

La visualización representa todas las etapas del proceso: el nivel del líquido en el tanque, el estado de las cinco lámparas y un texto con el estado activo. Los tres sensores se simulan con botones.

## 3.1 Elementos y su comportamiento
| Elemento | Variable enlazada | Comportamiento |
|---|---|---|
| Botones `B1_TankEmpty`, `B2_MinLevel`, `B3_Overflow` | `PLC_PRG.B1_TankEmpty`, `B2_MinLevel`, `B3_Overflow` | *Conmutar variable booleana*: cada clic activa/desactiva el sensor |
| Lámparas H1–H5 (elipses) | `H1_NivelOK`, `H2_NivelBajo`, `H3_Parpadeo`, `H4_TanqueVacio`, `H5_Parpadeo` | **Cambio de color** por estado de alarma (verde, amarillo, rojo); H3 y H5 parpadean |
| Rótulos junto a cada lámpara | — | **Etiquetas** descriptivas (p. ej. «H3 · NIVEL ALTO / REBOSE») |
| Marcas B1, B2, B3 junto al tanque | — | Indican la altura de cada sensor |
| Líquido del tanque (4 rectángulos) | `H4`, `H2`, `H1`, `H3` | **Aparición/desaparición** mediante la propiedad *Traer a primer plano*: el rectángulo del estado activo se superpone a una tapa que oculta los demás |
| **Texto de estado** («TANQUE VACÍO», «NIVEL BAJO», «NIVEL CORRECTO», «REBOSE: ¡DETENER LLENADO!», «ERROR DE SENSOR: REVISAR B1/B2/B3») | `H4`, `H2`, `H1`, `H3`, `H5_Error` | **Campo de texto** dinámico: cinco cajas de color superpuestas, cada una traída al primer plano por su variable; una tapa blanca oculta las inactivas |
| Título, subtítulo y rótulos de sección | — | Etiquetas estáticas |

## 3.2 Decisiones de diseño
- **Sin cálculos numéricos ni cadenas de texto en el Ladder.** El nivel y el texto de estado se animan con la propiedad *Traer a primer plano* y una tapa, evitando variables STRING y bloques MOVE: el Ladder permanece idéntico al validado y portable a OpenPLC.
- **Código de colores coherente** con la señalización industrial: verde = normal, amarillo = precaución (bajo / rebose), rojo = vacío / error.
- **Una sola lámpara de error** (H5): se eliminó la lámpara adicional del primer diseño al unificar las condiciones de error.

## 3.3 Capturas
![HMI en estado de rebose](https://raw.githubusercontent.com/santiagoguti17/ejercicio_TanqueQuimico/main/docs/img/codesys/hmi_04_rebose.png)
*Figura 6. HMI en estado de rebose (B3·B2·B1 = 111): tanque lleno, texto «REBOSE: ¡DETENER LLENADO!» y H3 intermitente.*
