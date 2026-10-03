# ejercicio_TanqueQuimico

**RA#2.3 — Automatización del monitoreo de nivel de un tanque de líquido químico**
Automatización y Control de Procesos (IIoT) · Universidad de La Sabana
Autor: Santiago Gutiérrez de Piñeres

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

## Documentación
La documentación completa está en la **[Wiki del repositorio](https://github.com/santiagoguti17/ejercicio_TanqueQuimico/wiki)**:
diseño lógico, Ladder y HMI en CODESYS, validación en simulación, diseño en OpenPLC, montaje físico, validación con hardware, análisis, uso de IA y referencias.

## Contenido
```
codesys/EjercicioTanqueQuimico_HMI.project   # Proyecto CODESYS V3.5 SP22 Patch 3
openplc/OpenPLC_TanqueQuimico/               # Proyecto OpenPLC Editor 4 (placa ESP32 WROOM)
docs/img/                                    # Capturas y fotografías usadas en la Wiki
```
