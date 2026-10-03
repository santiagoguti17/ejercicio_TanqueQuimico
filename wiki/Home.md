# Automatización del monitoreo de nivel de un tanque de líquido químico

**RA#2.3 · Automatización y Control de Procesos (IIoT) · Universidad de La Sabana**
**Autor:** Santiago (`santiagoguti17`) · Ingeniería Informática

> **Pregunta guía.** ¿Cómo puede utilizarse la automatización industrial para monitorear los niveles de un tanque de líquido químico, reduciendo el consumo de energía y el desperdicio de líquido?

## 1. Resumen
Se diseñó e implementó la lógica combinacional de un sistema de monitoreo de nivel con **tres sensores** (B1, B2, B3) y **cinco indicadores luminosos** (H1–H5). La solución se desarrolló en dos plataformas:

| Plataforma | Enfoque | Resultado |
|---|---|---|
| **CODESYS** (simulación) | Ladder + HMI; incluye un elemento controlado por tiempo (reloj de parpadeo con temporizadores TON) | [Diseño](CODESYS-Ladder.md) · [HMI](CODESYS-HMI.md) · [Validación](CODESYS-Validacion.md) |
| **OpenPLC + ESP32** (hardware real) | Mismo Ladder, proceso controlado por **eventos de sensores** | [Diseño](OpenPLC-Diseno.md) · [Montaje](Montaje-Fisico.md) · [Validación](OpenPLC-Validacion.md) |

El sistema distingue cuatro estados válidos del tanque (vacío, nivel bajo, nivel correcto y rebose) y trata como **error de sensor** (H5) cualquier combinación físicamente imposible, tal como exige el enunciado.

## 2. Contenido de la Wiki
1. [Análisis y diseño lógico](Analisis-y-Diseno-Logico.md) — enunciado, tabla de verdad, ecuaciones y circuito lógico.
2. [CODESYS: Ladder](CODESYS-Ladder.md) — redes, documentación y decisiones de diseño.
3. [CODESYS: HMI](CODESYS-HMI.md) — visualización, animaciones y etiquetas.
4. [CODESYS: validación en simulación](CODESYS-Validacion.md) — pruebas de todas las combinaciones.
5. [OpenPLC: diseño](OpenPLC-Diseno.md) — migración del Ladder, variables y mapeo de pines.
6. [Montaje físico](Montaje-Fisico.md) — componentes, esquema y conexiones.
7. [OpenPLC: validación con hardware](OpenPLC-Validacion.md) — protocolo y resultados de pruebas.
8. [Análisis: energía y desperdicio](Analisis-Energia-y-Desperdicio.md) — respuesta a la pregunta guía.
9. [Uso de inteligencia artificial](Uso-de-IA.md) — declaración de uso de herramientas de IA.
10. [Referencias](Referencias.md) — formato IEEE.

## 3. Estructura del repositorio
```
ejercicio_TanqueQuimico/
├── codesys/EjercicioTanqueQuimico_HMI.project      # Proyecto CODESYS (Ladder + HMI)
├── openplc/OpenPLC_TanqueQuimico/                  # Proyecto OpenPLC Editor (Ladder + pin mapping)
├── docs/img/                                       # Capturas y fotos usadas en la Wiki
└── wiki/                                           # Páginas de la Wiki (Markdown)
```

## 4. Cómo reproducir
- **CODESYS:** abrir `codesys/EjercicioTanqueQuimico_HMI.project` con CODESYS V3.5 SP22 Patch 3 → *Online → Simulación* → *Compilar* → *Login* → *Iniciar* → abrir la visualización `Visualization` y accionar los botones B1, B2 y B3.
- **OpenPLC:** abrir `openplc/OpenPLC_TanqueQuimico/project.json` con OpenPLC Editor, seleccionar el puerto COM de la ESP32 y subir el programa (ver [Montaje físico](Montaje-Fisico.md) para el cableado).

## 5. Alcance y limitaciones
- Los sensores de nivel se **simulan con un DIP switch** (1 = el líquido alcanza el sensor); no se emplearon sensores de nivel líquido reales.
- Los actuadores son LED indicadores; el enunciado no solicita control de bombas o válvulas.
- El enunciado no define un proceso por lotes, por lo que no se implementó conteo de lotes.
