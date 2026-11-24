# cardshorts

[![cardshorts](guia/assets/banner-es.jpg)](https://linnoff88-dev.github.io/

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

## QuÃ© es

cardshorts es una herramienta de lÃ­nea de comandos que crea videos cortos verticales (Reels, Shorts, TikTok) a partir de un archivo de texto. Cada escena tiene un diÃ¡logo y una pantalla lista (un "card"). La herramienta graba la narraciÃ³n con una voz clonada, verifica que la voz haya dicho bien el texto, genera las imÃ¡genes y entrega el MP4 con subtÃ­tulos palabra por palabra, en 30 segundos como mÃ¡ximo. Es para quien quiere publicar videos provocadores con frecuencia sin editar a mano. Corre en tu computadora, con tarjeta de video y los proyectos de voz e imagen de ARCKHÉ instalados.

## ðŸ“– GuÃ­a de uso

GuÃ­a completa (landing + paso a paso): **https://linnoff88-dev.github.io/

Short 9:16 provocador y narrado, a partir de **cards HTML**, con un archivo de guion y un comando.

```
roteiro.yaml â”€â”€â–¶ voz (Chatterbox local, voz clonada) â”€â”€â–¶ verificaciÃ³n por transcripciÃ³n (Whisper local)
             â”€â”€â–¶ imÃ¡genes (FLUX.2 klein local) â”€â”€â–¶ cards (Chrome headless) â”€â”€â–¶ MP4 1080Ã—1920
                 con subtÃ­tulos palabra por palabra, mÃºsica opcional y velocidad ajustada para caber en 30 s
```

Todo corre en tu mÃ¡quina. Nada de pago, ninguna API externa.

## Uso rÃ¡pido

```bash
./cardshorts.py estilos                              # estilos y moldes disponibles
./cardshorts.py novo meu-video --estilo extrato      # crea meu-video/roteiro.yaml
./cardshorts.py cards meu-video/roteiro.yaml         # solo imÃ¡genes + cards + previa.png (rÃ¡pido)
./cardshorts.py montar meu-video/roteiro.yaml        # video completo
./cardshorts.py musica musicas/minha.mp3 "calm lofi" # mÃºsica local (MusicGen-small)
```

## Guion

```yaml
titulo: "VocÃª Ã— humanoides"
saida: versus.mp4
estilo: versus
max_seg: 30
legenda: true
musica: true            # musicas/tensa.mp3 o una ruta
voz: {ref: nei}         # <carpeta de voces>/nei.wav
imagens:
  dupla: {prompt: "two modern faceless humanoid robots...", w: 1088, h: 640}
cenas:
  - fala: "De um lado, quem trabalha. Do outro, os humanoides."
    molde: capa-dupla
    campos: {foto_cima: "img:equipe", foto_baixo: "img:dupla", faixa: "VOCÃŠ Ã— ELES", tag_cima: "...", tag_baixo: "..."}
  - fala: "..."
    html: |               # card libre cuando ningÃºn molde sirve
      <section class="card" style="background:#000">...</section>
```

Escribe los diÃ¡logos como se habla: nÃºmeros en letras, "ARCKHÉ ponto club".

## Estilos

| estilo | apariencia | moldes |
|---|---|---|
| `versus` | editorial en blanco y negro + rojo | capa-dupla, tela-dividida, pergunta-foto, foto-cheia, cta-faixa, cta-lista |
| `cracha` | diana, credenciales selladas, boleta | capa-mira, dois-crachas, cedula, cracha-destaque |
| `documento` | papel mecanografiado | capa-pergunta, formulario, aviso, formulario-preenchido, certificado |
| `extrato` | factura / estado de cuenta | recibo-total, recibo, recibo-carimbo, recibo-enquete |
| `celular` | interfaz de telÃ©fono | notificacao, busca, fila, enquete |

Ejemplos listos en `exemplos/<estilo>/roteiro.yaml`.

## Lo que garantiza el comando

- **Habla fiel:** cada fragmento se transcribe; por debajo de 85 % de similitud con el texto, lo genera de nuevo (hasta 3Ã—).
- **Sin balbuceo:** corta lo que la voz suelta despuÃ©s de la Ãºltima palabra.
- **Cabe en el tiempo:** acelera el habla hasta 1,2Ã— para que quepa en `max_seg`; si no cabe, se detiene y pide recortar.
- **CachÃ©:** la voz y las imÃ¡genes solo se generan de nuevo cuando cambia el texto/prompt (`.obra/`).

## Requisitos

- ARCKHÉvox (proyecto ARCKHÉ) con `tts_direct.py` (Chatterbox) y `transcrever_v1.py` (Whisper)
- servidor de imÃ¡genes compatible con `POST /generate` (ARCKHÉimg, FLUX.2 klein)
- `ffmpeg` con libass, Chrome headless shell de Playwright, Python 3 + PyYAML
- Rutas ajustables con variables `CS_*` o `config.yaml` (ver el inicio de `cardshorts.py`)

## Skill

`skill/SKILL.md` le enseÃ±a al agente (Claude Code / Codex) a usar el motor con las reglas de contenido.
InstÃ¡lala con un enlace: `ln -s $PWD/skill ~/.claude/skills/cardshorts`.

MÃºsica de ejemplo generada con MusicGen-small (Meta, CC-BY-NC). Licencia del cÃ³digo: MIT.
