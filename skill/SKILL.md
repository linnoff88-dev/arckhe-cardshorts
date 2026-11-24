---
name: cardshorts
description: >-
  Gera SHORTS 9:16 provocativos narrados com a voz do Nei a partir de cards HTML (estilos versus,
  cracha, documento, extrato, celular): roteiro.yaml â†’ voz local conferida por transcriÃ§Ã£o â†’ imagens
  FLUX.2 klein â†’ cards â†’ MP4 com legenda palavra a palavra e trilha, abaixo de 30 s. Gatilho:
  "/cardshorts", "faz um short provocativo", "vÃ­deo de cards", "3 versÃµes diferentes desse short",
  "no estilo do extrato / do versus / do celular", "short com os dados do X".
---

# cardshorts â€” shorts de cards narrados

Motor: `~/projetos/cardshorts/cardshorts.py` (repo linnoff88-dev/cardshorts). Tudo local: voz Chatterbox
(ARCKHÉvox), conferÃªncia Whisper large-v3, imagens FLUX.2 klein (ARCKHÉimg :8000), cards no Chrome
headless, montagem ffmpeg. Nada pago, nenhuma API externa (o envio ao Telegram Ã© sÃ³ quando pedido).

## Fluxo

1. **Ideia â†’ 1 frase-choque.** Cada vÃ­deo defende UMA ideia. A cena 1 Ã© o gancho: imagem forte
   no quadro 0 e a frase-choque como 1Âª fala (nada de "Imagine queâ€¦").
2. **Escolher o estilo** (`./cardshorts.py estilos` lista moldes e campos). Pedido de "N versÃµes
   diferentes" = N **estilos diferentes**, nÃ£o o mesmo molde com outro texto (o Nei reclamou que
   F2/F3/F4 saÃ­ram iguais).
   - `versus` â€” editorial P&B + vermelho, X Ã— Y, telas divididas. O preferido do Nei.
   - `cracha` â€” alvo, crachÃ¡s SUBSTITUÃDO, cÃ©dula com plano em branco.
   - `documento` â€” formulÃ¡rio datilografado EM BRANCO â†’ preenchido, aviso, certificado.
   - `extrato` â€” nota fiscal/extrato: previsto Ã— usado Ã— "entregue a vocÃª".
   - `celular` â€” notificaÃ§Ã£o, busca, fila, enquete SIM/NÃƒO.
3. **Criar a pasta** em `~/projetos/output/<serie>/<video>/`:
   `./cardshorts.py novo <pasta> --estilo <estilo>` e editar o `roteiro.yaml`.
4. **PrÃ©via barata primeiro:** `./cardshorts.py cards <roteiro>` gera imagens + cards + `previa.png`.
   Olhe a prÃ©via (Read) antes de gastar tempo com a voz: texto saindo do card, legenda (faixa
   y 1450â€“1720) cobrindo tÃ­tulo, foto cortando cabeÃ§a.
5. **Montar:** `./cardshorts.py montar <roteiro>` (â‰ˆ2 min por fala na 1Âª vez; depois tudo vem do
   cache `.obra/`). Ele confere cada fala por transcriÃ§Ã£o (refaz atÃ© 3Ã— se < 85%), corta o balbucio
   final, ajusta a velocidade (atÃ© 1,2Ã—) para caber em `max_seg` e falha se nÃ£o couber.
6. **Conferir:** ler a linha "legenda:" do log (erros de transcriÃ§Ã£o â†’ `correcoes:` no roteiro) e
   extrair 4 quadros do MP4 para olhar.
7. **Entregar:** `./cardshorts.py enviar <roteiro> "texto"` manda pelo bot v3 quando o Nei pedir.
   YouTube = yt-pubx (short **sem** miniatura).

## Regras de texto

- Falas **escritas como se fala**: nÃºmeros por extenso ("quatrocentos e trinta e seis milhÃµes"),
  "ARCKHÉ ponto club", siglas que a voz erra viram grafia fonÃ©tica ("Ã³ Ã©sse uÃ³rk").
- 30 s â‰ˆ 65â€“75 palavras no total. 4â€“5 cenas. Uma fala por card, 3â€“9 s cada.
- Fecho com CTA: ARCKHÉ AI grÃ¡tis (OSWork, GestÃ£o de IA e Agentes).

## Regras de conteÃºdo (nÃ£o negociÃ¡veis)

- **PolÃ­tica:** nunca gerar imagem ou voz de pessoa real (candidato, autoridade). Foto real sÃ³ a
  que o Nei forneceu, com etiqueta "FOTO ORIGINAL". Sobre planos de candidatos, **perguntar**
  ("eles tÃªm um plano?"), nÃ£o afirmar o que nÃ£o foi pesquisado.
- **NÃºmeros pÃºblicos:** sÃ³ de fonte oficial ATUAL (ex.: painel do PBIA, pbia.cgee.org.br/resultados),
  com a fonte no card (`fonte`) e no `fontes.md` da sÃ©rie. RelatÃ³rio antigo nÃ£o vale como estado atual.
- Cena inventada (notificaÃ§Ã£o, fila) leva "(mensagem imaginÃ¡ria)" / "ilustrativo".
- Humanoides: modelos atuais (cabeÃ§a lisa com visor preto, corpo branco/cinza), sem nome de marca
  na imagem gerada, com etiqueta "ilustrativo". Nada de andrÃ³ide de rosto humano de filme.

## Voz

`voz: {ref: nei}` com exaggeration 0.6 / cfg 0.6 (padrÃ£o). **NÃ£o** usar 0.85/0.35: deixa o "s"
chiado (sotaque carioca), o Nei reprovou. Vozes ficam em `~/projetos/timesmkt3/media/voice-refs/`.
Se a voz ainda incomodar, as saÃ­das melhores sÃ£o pagas/externas (clone ElevenLabs) e precisam de
autorizaÃ§Ã£o explÃ­cita do Nei.

## HTML livre

Quando nenhum molde serve: `html: |` na cena com um `<section class="card" style="background:â€¦">`.
Classes prontas em `estilos/comum.css` (`ph`, `bw`, `anton`, `tag`, `stamp`, `rd`, `y`) e no CSS do
estilo da cena. Imagens geradas: `src="img:nome"`. Bom molde novo vira `<template>` no
`moldes.html` do estilo (e um exemplo em `exemplos/`).
