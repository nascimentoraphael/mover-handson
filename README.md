# A fila da 9ª chamada MOVER Hands-On (DR-SP)

Painel interativo, em um único arquivo HTML, que mostra a ordem de submissão das **417 propostas do DR-SP** na 9ª chamada do MOVER Hands-On Produtividade e simula até onde o recurso chega.

O regulamento distribui os **R$ 14,4 milhões** nacionais por ordem de chegada, não por mérito. Todas as propostas paulistas entraram em 12 min 01 s (de 15:00:20 a 15:12:21 de 28/08/2026). O painel responde a uma pergunta: **quem ficaria dentro do corte para um dado volume de recurso**.

## Como usar

Abra o `index.html` em qualquer navegador. Não há build, servidor nem dependências externas: HTML, CSS, JS e dados estão no mesmo arquivo.

## O que o painel mostra

| Bloco | Função |
|---|---|
| Linha do tempo | Cada traço é uma proposta no segundo em que entrou; a altura é o valor pedido. A área cinza é o acumulado do DR-SP e a linha tracejada marca onde o recurso simulado acaba. |
| Controle de recurso | Define a fatia hipotética do DR-SP (R$ 1 mi a R$ 24,84 mi). Atalhos: "metade" (R$ 7,2 mi) e "tudo" (R$ 14,4 mi). |
| Filtros | Unidade executora, valor pedido, porte, escopo, faixa de horário e busca por empresa ou cidade. |
| Indicadores | Propostas na seleção, recurso pedido, ticket médio, posição média na fila e quantas ficam dentro do corte. |
| Unidades | Ranking de unidades na seleção; clicar inclui ou exclui do filtro. |
| Composição | Distribuição da seleção por escopo, valor e porte. |
| Tabela | Lista ordenável com acumulado e status de corte; exporta a seleção em CSV (`;`, UTF-8 com BOM, abre direto no Excel). |

Referências rápidas do corte, pela ordem da fila:

| Recurso simulado | Última proposta dentro | Horário |
|---|---|---|
| R$ 7,2 mi | nº 109 | 15:01:29 |
| R$ 14,4 mi | nº 223 | 15:02:45 |

A unidade **1.27** tem destaque fixo em vermelho (chip e borda na tabela).

## Dados

Embutidos no bloco `<script id="rows" type="application/json">`. Um objeto por proposta:

| Campo | Conteúdo |
|---|---|
| `p` | Posição na fila (1 a 417) |
| `t` | Segundos após 15:00:00 |
| `e` | Empresa (razão social como veio da plataforma) |
| `u` | Unidades executoras (lista; pode ter 0 a 3) |
| `c` | Cidade |
| `g` | Porte: Micro, Pequena, Média, Grande |
| `s` | Escopo: Lean, Digitalização, Lean + Digitalização, Não identificado |
| `v` | Recurso pedido (R$ 40 mil, 80 mil ou 120 mil) |
| `k` | Acumulado do DR-SP até esta proposta |

**Fonte:** relatório de ideias da Plataforma Inovação (28/08/2026) e regulamento da categoria MOVER Hands-On Produtividade.

**Tratamento aplicado:** entram só as 417 propostas que chegaram à 2ª etapa; 71 pararam na 1ª e 5 registros de teste foram descartados. A unidade é extraída do nome da proposta.

Para atualizar, substitua o JSON mantendo os campos. Se mudar a janela de tempo, ajuste também os limites fixos `20` e `741` (sliders `t0`/`t1`, `F.t` e `drawQueue`) e o `max` do controle de recurso.

## Limites da análise

Leia antes de tirar conclusão:

* **A divisão de R$ 7,2 mi para SP é hipótese de trabalho.** Nada no regulamento garante essa fatia.
* **A fila real é nacional.** A posição de SP depende de quando os outros 26 DRs submeteram, o que este relatório não mostra. O corte simulado é um cenário, não uma previsão.
* **Propostas com duas unidades contam para as duas** no gráfico de unidades; a soma por unidade passa de 417.
* **Há empresas repetidas** na fila (ex.: GFERTEC 3 vezes; MAHLE, Pressmatic, TTB, Stratus, VPOL, Labor, Fuerza, Trink, Embrastec, SPA Turbo e RP Tudogaz 2 vezes cada). O painel não deduplica; cada submissão ocupa sua posição e consome recurso no acumulado.
* **3 propostas sem unidade identificada** e 2 com escopo "Não identificado".

## Problemas conhecidos no código

* A razão social da proposta 403 veio com o prefixo `Razão Social:\t`; convém limpar na origem.
* As colunas "Acumulado DR-SP" e "Corte" usam a mesma chave de ordenação (`data-k="k"`); ordenar por "Corte" exibe o rótulo "acumulado dr-sp" no contador.
* Nomes de empresa entram via `innerHTML` sem escape. Sem risco com os dados atuais, mas vale escapar se o JSON passar a vir de fonte externa.
* A escala do gráfico (`maxV`, `maxK`) é fixa; precisa de ajuste se os valores mudarem.

## Acessibilidade

Chips e cabeçalhos da tabela são navegáveis por teclado (Enter/Espaço), com foco visível. Respeita `prefers-reduced-motion`. Layout responsivo abaixo de 900 px.
