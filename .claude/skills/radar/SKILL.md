---
name: radar
description: Roda o radar de IA do dia, acionando os agentes do time na ordem (pesquisador, verificador, redator, guarda na etapa SITE, divulgador, guarda na etapa INSTAGRAM), uma etapa por vez, e mostra a primeira linha do briefing no fim. Use quando pedirem para rodar o radar, rodar o radar de hoje ou gerar o briefing do dia.
---

# Radar do dia

Esta skill roda o time do radar, uma etapa por vez, na ordem abaixo. Cada etapa só começa quando a anterior termina e deu certo.

## Antes de começar

1. Leia `RADAR.md` e `CLAUDE.md`.
2. Descubra a data de hoje e use o formato AAAA-MM-DD.
3. Veja se `diario/AAAA-MM-DD.md` de hoje já existe. Se existir, **pare e pergunte** se é para rodar de novo, porque isso sobrescreve os arquivos de hoje (`fontes/`, `verificacao/`, `diario/`, `index.html` e `instagram/`). Só siga com um sim claro.

## Como acionar cada agente

Acione cada etapa com a ferramenta Agent, usando o nome do agente como `subagent_type` e rodando em primeiro plano, porque a etapa seguinte depende dela.

Se o agente não aparecer na lista de tipos disponíveis (isso acontece quando ele foi criado durante a sessão), use o tipo `general-purpose` com o modelo `sonnet` e peça que ele leia `.claude/agents/<nome>.md` e siga o corpo do arquivo à risca, usando só as ferramentas do cabeçalho.

Em todo pedido, informe a pasta do projeto e a data de hoje.

## As etapas, nesta ordem

1. **pesquisador:** grava `fontes/AAAA-MM-DD.md`. Se ele gravar menos de cinco itens, avise e pergunte se é para seguir.
2. **verificador:** grava `verificacao/AAAA-MM-DD.md`. Se nenhum item der CONFERE, pare e avise.
3. **redator:** grava `diario/AAAA-MM-DD.md` e `index.html`.
4. **guarda, etapa SITE:** confere o briefing e a página.
   - Se disser **NÃO PUBLIQUE**, pare. Mostre a tabela do guarda e pergunte como resolver. Não corrija nada por conta própria e não siga para o divulgador.
5. **divulgador:** só roda depois do PODE PUBLICAR da etapa SITE. Grava `instagram/AAAA-MM-DD/`.
6. **guarda, etapa INSTAGRAM:** confere o post.
   - Se disser **NÃO PUBLIQUE**, mostre a tabela e pergunte como resolver.

Depois de cada etapa, diga em uma linha o que ela entregou antes de começar a próxima.

## No fim

Mostre, nesta ordem:

1. **A primeira linha do briefing**, copiada de `diario/AAAA-MM-DD.md`, exatamente como está.
2. Uma linha por etapa com o resultado (quantos itens, quantos conferiram, a decisão do guarda em cada etapa).
3. Onde estão os arquivos do dia.
4. Um lembrete: nada foi publicado. O Instagram fica com a pessoa, e o envio para o GitHub só acontece se ela pedir.

## Nunca

- Nunca pule uma etapa nem mude a ordem.
- Nunca rode o divulgador sem o PODE PUBLICAR da etapa SITE.
- Nunca publique no Instagram nem envie para o GitHub sem pedido.
- Nunca apague um dia anterior.
