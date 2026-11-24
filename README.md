# cardshorts

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![cardshorts](guia/assets/banner.jpg)](https://linnoff88-dev.github.io/

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

## O que Ã©

O cardshorts Ã© uma ferramenta de linha de comando que cria vÃ­deos curtos verticais (Reels, Shorts, TikTok) a partir de um arquivo de texto. Cada cena tem uma fala e uma tela pronta (um "card"). A ferramenta grava a narraÃ§Ã£o com uma voz clonada, confere se a voz falou certo, gera as imagens e entrega o MP4 com legenda palavra a palavra, cabendo em 30 segundos. Ã‰ para quem quer publicar vÃ­deos provocativos com frequÃªncia sem editar na mÃ£o. Roda no seu computador, com placa de vÃ­deo e os projetos de voz e imagem do ARCKHÉ instalados.

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/

Short 9:16 provocativo, narrado, a partir de **cards HTML** â€” com um arquivo de roteiro e um comando.

```
roteiro.yaml â”€â”€â–¶ voz (Chatterbox local, voz clonada) â”€â”€â–¶ conferÃªncia por transcriÃ§Ã£o (Whisper local)
             â”€â”€â–¶ imagens (FLUX.2 klein local) â”€â”€â–¶ cards (Chrome headless) â”€â”€â–¶ MP4 1080Ã—1920
                 com legenda palavra a palavra, trilha opcional e velocidade ajustada para caber em 30 s
```

Tudo roda na sua mÃ¡quina. Nada pago, nenhuma API externa.

## Uso rÃ¡pido

```bash
./cardshorts.py estilos                              # estilos e moldes disponÃ­veis
./cardshorts.py novo meu-video --estilo extrato      # cria meu-video/roteiro.yaml
./cardshorts.py cards meu-video/roteiro.yaml         # sÃ³ imagens + cards + previa.png (rÃ¡pido)
./cardshorts.py montar meu-video/roteiro.yaml        # vÃ­deo completo
./cardshorts.py musica musicas/minha.mp3 "calm lofi" # trilha local (MusicGen-small)
```

## Roteiro

```yaml
titulo: "VocÃª Ã— humanoides"
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
    campos: {foto_cima: "img:equipe", foto_baixo: "img:dupla", faixa: "VOCÃŠ Ã— ELES", tag_cima: "...", tag_baixo: "..."}
  - fala: "..."
    html: |               # card livre quando nenhum molde serve
      <section class="card" style="background:#000">...</section>
```

Escreva as falas como se fala: nÃºmeros por extenso, "ARCKHÉ ponto club".

## Estilos

| estilo | cara | moldes |
|---|---|---|
| `versus` | editorial P&B + vermelho | capa-dupla, tela-dividida, pergunta-foto, foto-cheia, cta-faixa, cta-lista |
| `cracha` | alvo, crachÃ¡s carimbados, cÃ©dula | capa-mira, dois-crachas, cedula, cracha-destaque |
| `documento` | papel datilografado | capa-pergunta, formulario, aviso, formulario-preenchido, certificado |
| `extrato` | nota fiscal / extrato | recibo-total, recibo, recibo-carimbo, recibo-enquete |
| `celular` | interface de telefone | notificacao, busca, fila, enquete |

Exemplos prontos em `exemplos/<estilo>/roteiro.yaml`.

## O que o comando garante

- **Fala fiel:** cada trecho Ã© transcrito; abaixo de 85 % de semelhanÃ§a com o texto, gera de novo (atÃ© 3Ã—).
- **Sem balbucio:** corta o que a voz solta depois da Ãºltima palavra.
- **Cabe no tempo:** acelera a fala atÃ© 1,2Ã— para caber em `max_seg`; se nÃ£o couber, para e pede corte.
- **Cache:** voz e imagens sÃ³ sÃ£o geradas de novo quando o texto/prompt muda (`.obra/`).

## Requisitos

- ARCKHÉvox (projeto ARCKHÉ) com `tts_direct.py` (Chatterbox) e `transcrever_v1.py` (Whisper)
- servidor de imagem compatÃ­vel com `POST /generate` (ARCKHÉimg, FLUX.2 klein)
- `ffmpeg` com libass, Chrome headless shell do Playwright, Python 3 + PyYAML
- Caminhos ajustÃ¡veis por variÃ¡veis `CS_*` ou `config.yaml` (veja o topo de `cardshorts.py`)

## Skill

`skill/SKILL.md` ensina o agente (Claude Code / Codex) a usar o motor com as regras de conteÃºdo.
Instale com um link: `ln -s $PWD/skill ~/.claude/skills/cardshorts`.

Trilha de exemplo gerada com MusicGen-small (Meta, CC-BY-NC). LicenÃ§a do cÃ³digo: MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>