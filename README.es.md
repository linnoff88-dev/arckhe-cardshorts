# cardshorts

[![cardshorts](guia/assets/banner-es.jpg)](https://linnoff88-dev.github.io/

**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

## QuêÂ© es

cardshorts es una herramienta de lêÂ­nea de comandos que crea videos cortos verticales (Reels, Shorts, TikTok) a partir de un archivo de texto. Cada escena tiene un diêÂ¡logo y una pantalla lista (un "card"). La herramienta graba la narraciêÂ³n con una voz clonada, verifica que la voz haya dicho bien el texto, genera las imêÂ¡genes y entrega el MP4 con subtêÂ­tulos palabra por palabra, en 30 segundos como mêÂ¡ximo. Es para quien quiere publicar videos provocadores con frecuencia sin editar a mano. Corre en tu computadora, con tarjeta de video y los proyectos de voz e imagen de ARCKHê instalados.

## ê°Å¸ââ GuêÂ­a de uso

GuêÂ­a completa (landing + paso a paso): **https://linnoff88-dev.github.io/

Short 9:16 provocador y narrado, a partir de **cards HTML**, con un archivo de guion y un comando.

```
roteiro.yaml ê¢ââ¬ê¢ââ¬ê¢âÂ¶ voz (Chatterbox local, voz clonada) ê¢ââ¬ê¢ââ¬ê¢âÂ¶ verificaciêÂ³n por transcripciêÂ³n (Whisper local)
             ê¢ââ¬ê¢ââ¬ê¢âÂ¶ imêÂ¡genes (FLUX.2 klein local) ê¢ââ¬ê¢ââ¬ê¢âÂ¶ cards (Chrome headless) ê¢ââ¬ê¢ââ¬ê¢âÂ¶ MP4 1080êâ1920
                 con subtêÂ­tulos palabra por palabra, mêÂºsica opcional y velocidad ajustada para caber en 30 s
```

Todo corre en tu mêÂ¡quina. Nada de pago, ninguna API externa.

## Uso rêÂ¡pido

```bash
./cardshorts.py estilos                              # estilos y moldes disponibles
./cardshorts.py novo meu-video --estilo extrato      # crea meu-video/roteiro.yaml
./cardshorts.py cards meu-video/roteiro.yaml         # solo imêÂ¡genes + cards + previa.png (rêÂ¡pido)
./cardshorts.py montar meu-video/roteiro.yaml        # video completo
./cardshorts.py musica musicas/minha.mp3 "calm lofi" # mêÂºsica local (MusicGen-small)
```

## Guion

```yaml
titulo: "VocêÂª êâ humanoides"
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
    campos: {foto_cima: "img:equipe", foto_baixo: "img:dupla", faixa: "VOCêÅ  êâ ELES", tag_cima: "...", tag_baixo: "..."}
  - fala: "..."
    html: |               # card libre cuando ningêÂºn molde sirve
      <section class="card" style="background:#000">...</section>
```

Escribe los diêÂ¡logos como se habla: nêÂºmeros en letras, "ARCKHê ponto club".

## Estilos

| estilo | apariencia | moldes |
|---|---|---|
| `versus` | editorial en blanco y negro + rojo | capa-dupla, tela-dividida, pergunta-foto, foto-cheia, cta-faixa, cta-lista |
| `cracha` | diana, credenciales selladas, boleta | capa-mira, dois-crachas, cedula, cracha-destaque |
| `documento` | papel mecanografiado | capa-pergunta, formulario, aviso, formulario-preenchido, certificado |
| `extrato` | factura / estado de cuenta | recibo-total, recibo, recibo-carimbo, recibo-enquete |
| `celular` | interfaz de telêÂ©fono | notificacao, busca, fila, enquete |

Ejemplos listos en `exemplos/<estilo>/roteiro.yaml`.

## Lo que garantiza el comando

- **Habla fiel:** cada fragmento se transcribe; por debajo de 85 % de similitud con el texto, lo genera de nuevo (hasta 3êâ).
- **Sin balbuceo:** corta lo que la voz suelta despuêÂ©s de la êÂºltima palabra.
- **Cabe en el tiempo:** acelera el habla hasta 1,2êâ para que quepa en `max_seg`; si no cabe, se detiene y pide recortar.
- **CachêÂ©:** la voz y las imêÂ¡genes solo se generan de nuevo cuando cambia el texto/prompt (`.obra/`).

## Requisitos

- ARCKHêvox (proyecto ARCKHê) con `tts_direct.py` (Chatterbox) y `transcrever_v1.py` (Whisper)
- servidor de imêÂ¡genes compatible con `POST /generate` (ARCKHêimg, FLUX.2 klein)
- `ffmpeg` con libass, Chrome headless shell de Playwright, Python 3 + PyYAML
- Rutas ajustables con variables `CS_*` o `config.yaml` (ver el inicio de `cardshorts.py`)

## Skill

`skill/SKILL.md` le enseêÂ±a al agente (Claude Code / Codex) a usar el motor con las reglas de contenido.
InstêÂ¡lala con un enlace: `ln -s $PWD/skill ~/.claude/skills/cardshorts`.

MêÂºsica de ejemplo generada con MusicGen-small (Meta, CC-BY-NC). Licencia del cêÂ³digo: MIT.
