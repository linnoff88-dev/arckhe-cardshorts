# cardshorts

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![cardshorts](guia/assets/banner.jpg)](https://linnoff88-dev.github.io/

**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

## O que êÂ©

O cardshorts êÂ© uma ferramenta de linha de comando que cria vêÂ­deos curtos verticais (Reels, Shorts, TikTok) a partir de um arquivo de texto. Cada cena tem uma fala e uma tela pronta (um "card"). A ferramenta grava a narraêÂ§êÂ£o com uma voz clonada, confere se a voz falou certo, gera as imagens e entrega o MP4 com legenda palavra a palavra, cabendo em 30 segundos. êâ° para quem quer publicar vêÂ­deos provocativos com frequêÂªncia sem editar na mêÂ£o. Roda no seu computador, com placa de vêÂ­deo e os projetos de voz e imagem do ARCKHê instalados.

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/

Short 9:16 provocativo, narrado, a partir de **cards HTML** ê¢â¬â com um arquivo de roteiro e um comando.

```
roteiro.yaml ê¢ââ¬ê¢ââ¬ê¢âÂ¶ voz (Chatterbox local, voz clonada) ê¢ââ¬ê¢ââ¬ê¢âÂ¶ conferêÂªncia por transcriêÂ§êÂ£o (Whisper local)
             ê¢ââ¬ê¢ââ¬ê¢âÂ¶ imagens (FLUX.2 klein local) ê¢ââ¬ê¢ââ¬ê¢âÂ¶ cards (Chrome headless) ê¢ââ¬ê¢ââ¬ê¢âÂ¶ MP4 1080êâ1920
                 com legenda palavra a palavra, trilha opcional e velocidade ajustada para caber em 30 s
```

Tudo roda na sua mêÂ¡quina. Nada pago, nenhuma API externa.

## Uso rêÂ¡pido

```bash
./cardshorts.py estilos                              # estilos e moldes disponêÂ­veis
./cardshorts.py novo meu-video --estilo extrato      # cria meu-video/roteiro.yaml
./cardshorts.py cards meu-video/roteiro.yaml         # sêÂ³ imagens + cards + previa.png (rêÂ¡pido)
./cardshorts.py montar meu-video/roteiro.yaml        # vêÂ­deo completo
./cardshorts.py musica musicas/minha.mp3 "calm lofi" # trilha local (MusicGen-small)
```

## Roteiro

```yaml
titulo: "VocêÂª êâ humanoides"
saida: versus.mp4
estilo: versus
max_seg: 30
legenda: true
musica: true            # musicas/tensa.mp3 ou caminho
voz: {ref: nei}         # <pasta de vozes>/nei.wav
imagens:
  dupla: {prompt: "two modern faceless humanoid robots...", w: 1088, h: 640}
cenas:
  - fala: "De um lado, quem trabalha. Do outro, os humanoides."
    molde: capa-dupla
    campos: {foto_cima: "img:equipe", foto_baixo: "img:dupla", faixa: "VOCêÅ  êâ ELES", tag_cima: "...", tag_baixo: "..."}
  - fala: "..."
    html: |               # card livre quando nenhum molde serve
      <section class="card" style="background:#000">...</section>
```

Escreva as falas como se fala: nêÂºmeros por extenso, "ARCKHê ponto club".

## Estilos

| estilo | cara | moldes |
|---|---|---|
| `versus` | editorial P&B + vermelho | capa-dupla, tela-dividida, pergunta-foto, foto-cheia, cta-faixa, cta-lista |
| `cracha` | alvo, crachêÂ¡s carimbados, cêÂ©dula | capa-mira, dois-crachas, cedula, cracha-destaque |
| `documento` | papel datilografado | capa-pergunta, formulario, aviso, formulario-preenchido, certificado |
| `extrato` | nota fiscal / extrato | recibo-total, recibo, recibo-carimbo, recibo-enquete |
| `celular` | interface de telefone | notificacao, busca, fila, enquete |

Exemplos prontos em `exemplos/<estilo>/roteiro.yaml`.

## O que o comando garante

- **Fala fiel:** cada trecho êÂ© transcrito; abaixo de 85 % de semelhanêÂ§a com o texto, gera de novo (atêÂ© 3êâ).
- **Sem balbucio:** corta o que a voz solta depois da êÂºltima palavra.
- **Cabe no tempo:** acelera a fala atêÂ© 1,2êâ para caber em `max_seg`; se nêÂ£o couber, para e pede corte.
- **Cache:** voz e imagens sêÂ³ sêÂ£o geradas de novo quando o texto/prompt muda (`.obra/`).

## Requisitos

- ARCKHêvox (projeto ARCKHê) com `tts_direct.py` (Chatterbox) e `transcrever_v1.py` (Whisper)
- servidor de imagem compatêÂ­vel com `POST /generate` (ARCKHêimg, FLUX.2 klein)
- `ffmpeg` com libass, Chrome headless shell do Playwright, Python 3 + PyYAML
- Caminhos ajustêÂ¡veis por variêÂ¡veis `CS_*` ou `config.yaml` (veja o topo de `cardshorts.py`)

## Skill

`skill/SKILL.md` ensina o agente (Claude Code / Codex) a usar o motor com as regras de conteêÂºdo.
Instale com um link: `ln -s $PWD/skill ~/.claude/skills/cardshorts`.

Trilha de exemplo gerada com MusicGen-small (Meta, CC-BY-NC). LicenêÂ§a do cêÂ³digo: MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>