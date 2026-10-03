# ejercicio_TanqueQuimico

**RA#2.3 — Automatización del monitoreo de nivel de un tanque de líquido químico**
Automatización y Control de Procesos (IIoT) · Universidad de La Sabana

Lógica combinacional en **Ladder** para monitorear el nivel de un tanque con tres sensores (B1, B2, B3) y cinco lámparas (H1–H5), implementada en:

- **CODESYS** — simulación con HMI ([`codesys/`](codesys/)).
- **OpenPLC + ESP32 WROOM** — prototipo con hardware real ([`openplc/`](openplc/)).

| Combinación (B3 B2 B1) | Estado | Lámpara |
|:-:|---|:-:|
| 000 | Tanque vacío | H4 |
| 001 | Nivel bajo | H2 |
| 011 | Nivel correcto | H1 |
| 111 | Rebose | H3 |
| 010 · 100 · 101 · 110 | Error de sensor | H5 |

## Documentación (Wiki)
Las páginas están en [`wiki/`](wiki/). Comenzar por [`wiki/Home.md`](wiki/Home.md).

1. [Análisis y diseño lógico](wiki/Analisis-y-Diseno-Logico.md)
2. [CODESYS: Ladder](wiki/CODESYS-Ladder.md) · [HMI](wiki/CODESYS-HMI.md) · [Validación](wiki/CODESYS-Validacion.md)
3. [OpenPLC: diseño](wiki/OpenPLC-Diseno.md) · [Montaje físico](wiki/Montaje-Fisico.md) · [Validación con hardware](wiki/OpenPLC-Validacion.md)
4. [Análisis: energía y desperdicio](wiki/Analisis-Energia-y-Desperdicio.md)
5. [Uso de inteligencia artificial](wiki/Uso-de-IA.md)
6. [Referencias](wiki/Referencias.md)

## Contenido
```
codesys/EjercicioTanqueQuimico_HMI.project   # Proyecto CODESYS V3.5 SP22 Patch 3
openplc/OpenPLC_TanqueQuimico/               # Proyecto OpenPLC Editor 4 (placa ESP32 WROOM)
docs/img/                                    # Capturas y fotografías
wiki/                                        # Wiki en Markdown
```
