---
name: cardshorts
description: >-
  Gera SHORTS 9:16 provocativos narrados com a voz do Nei a partir de cards HTML (estilos versus,
  cracha, documento, extrato, celular): roteiro.yaml ê¢â â voz local conferida por transcriêÂ§êÂ£o ê¢â â imagens
  FLUX.2 klein ê¢â â cards ê¢â â MP4 com legenda palavra a palavra e trilha, abaixo de 30 s. Gatilho:
  "/cardshorts", "faz um short provocativo", "vêÂ­deo de cards", "3 versêÂµes diferentes desse short",
  "no estilo do extrato / do versus / do celular", "short com os dados do X".
---

# cardshorts ê¢â¬â shorts de cards narrados

Motor: `~/projetos/cardshorts/cardshorts.py` (repo linnoff88-dev/cardshorts). Tudo local: voz Chatterbox
(ARCKHêvox), conferêÂªncia Whisper large-v3, imagens FLUX.2 klein (ARCKHêimg :8000), cards no Chrome
headless, montagem ffmpeg. Nada pago, nenhuma API externa (o envio ao Telegram êÂ© sêÂ³ quando pedido).

## Fluxo

1. **Ideia ê¢â â 1 frase-choque.** Cada vêÂ­deo defende UMA ideia. A cena 1 êÂ© o gancho: imagem forte
   no quadro 0 e a frase-choque como 1êÂª fala (nada de "Imagine queê¢â¬Â¦").
2. **Escolher o estilo** (`./cardshorts.py estilos` lista moldes e campos). Pedido de "N versêÂµes
   diferentes" = N **estilos diferentes**, nêÂ£o o mesmo molde com outro texto (o Nei reclamou que
   F2/F3/F4 saêÂ­ram iguais).
   - `versus` ê¢â¬â editorial P&B + vermelho, X êâ Y, telas divididas. O preferido do Nei.
   - `cracha` ê¢â¬â alvo, crachêÂ¡s SUBSTITUêÂDO, cêÂ©dula com plano em branco.
   - `documento` ê¢â¬â formulêÂ¡rio datilografado EM BRANCO ê¢â â preenchido, aviso, certificado.
   - `extrato` ê¢â¬â nota fiscal/extrato: previsto êâ usado êâ "entregue a vocêÂª".
   - `celular` ê¢â¬â notificaêÂ§êÂ£o, busca, fila, enquete SIM/NêÆO.
3. **Criar a pasta** em `~/projetos/output/<serie>/<video>/`:
   `./cardshorts.py novo <pasta> --estilo <estilo>` e editar o `roteiro.yaml`.
4. **PrêÂ©via barata primeiro:** `./cardshorts.py cards <roteiro>` gera imagens + cards + `previa.png`.
   Olhe a prêÂ©via (Read) antes de gastar tempo com a voz: texto saindo do card, legenda (faixa
   y 1450ê¢â¬â1720) cobrindo têÂ­tulo, foto cortando cabeêÂ§a.
5. **Montar:** `./cardshorts.py montar <roteiro>` (ê¢â°Ë2 min por fala na 1êÂª vez; depois tudo vem do
   cache `.obra/`). Ele confere cada fala por transcriêÂ§êÂ£o (refaz atêÂ© 3êâ se < 85%), corta o balbucio
   final, ajusta a velocidade (atêÂ© 1,2êâ) para caber em `max_seg` e falha se nêÂ£o couber.
6. **Conferir:** ler a linha "legenda:" do log (erros de transcriêÂ§êÂ£o ê¢â â `correcoes:` no roteiro) e
   extrair 4 quadros do MP4 para olhar.
7. **Entregar:** `./cardshorts.py enviar <roteiro> "texto"` manda pelo bot v3 quando o Nei pedir.
   YouTube = yt-pubx (short **sem** miniatura).

## Regras de texto

- Falas **escritas como se fala**: nêÂºmeros por extenso ("quatrocentos e trinta e seis milhêÂµes"),
  "ARCKHê ponto club", siglas que a voz erra viram grafia fonêÂ©tica ("êÂ³ êÂ©sse uêÂ³rk").
- 30 s ê¢â°Ë 65ê¢â¬â75 palavras no total. 4ê¢â¬â5 cenas. Uma fala por card, 3ê¢â¬â9 s cada.
- Fecho com CTA: ARCKHê AI grêÂ¡tis (OSWork, GestêÂ£o de IA e Agentes).

## Regras de conteêÂºdo (nêÂ£o negociêÂ¡veis)

- **PolêÂ­tica:** nunca gerar imagem ou voz de pessoa real (candidato, autoridade). Foto real sêÂ³ a
  que o Nei forneceu, com etiqueta "FOTO ORIGINAL". Sobre planos de candidatos, **perguntar**
  ("eles têÂªm um plano?"), nêÂ£o afirmar o que nêÂ£o foi pesquisado.
- **NêÂºmeros pêÂºblicos:** sêÂ³ de fonte oficial ATUAL (ex.: painel do PBIA, pbia.cgee.org.br/resultados),
  com a fonte no card (`fonte`) e no `fontes.md` da sêÂ©rie. RelatêÂ³rio antigo nêÂ£o vale como estado atual.
- Cena inventada (notificaêÂ§êÂ£o, fila) leva "(mensagem imaginêÂ¡ria)" / "ilustrativo".
- Humanoides: modelos atuais (cabeêÂ§a lisa com visor preto, corpo branco/cinza), sem nome de marca
  na imagem gerada, com etiqueta "ilustrativo". Nada de andrêÂ³ide de rosto humano de filme.

## Voz

`voz: {ref: nei}` com exaggeration 0.6 / cfg 0.6 (padrêÂ£o). **NêÂ£o** usar 0.85/0.35: deixa o "s"
chiado (sotaque carioca), o Nei reprovou. Vozes ficam em `~/projetos/timesmkt3/media/voice-refs/`.
Se a voz ainda incomodar, as saêÂ­das melhores sêÂ£o pagas/externas (clone ElevenLabs) e precisam de
autorizaêÂ§êÂ£o explêÂ­cita do Nei.

## HTML livre

Quando nenhum molde serve: `html: |` na cena com um `<section class="card" style="background:ê¢â¬Â¦">`.
Classes prontas em `estilos/comum.css` (`ph`, `bw`, `anton`, `tag`, `stamp`, `rd`, `y`) e no CSS do
estilo da cena. Imagens geradas: `src="img:nome"`. Bom molde novo vira `<template>` no
`moldes.html` do estilo (e um exemplo em `exemplos/`).
