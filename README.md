# LP · Patrícia Almeida Nutri

Landing page para **Patrícia Almeida**, nutricionista em Maracanaú/CE.
**Estrutura e movimentos:** [`templates/dark-impact`](../../templates/dark-impact/).
**Paleta:** referência [brunaburti.com.br](https://www.brunaburti.com.br/).

Rodar local: `python -m http.server 4321` nesta pasta → http://127.0.0.1:4321

## O que mudou em relação ao template

| | Dark Impact (oficina) | Aqui (nutri) |
|---|---|---|
| Base | escura o tempo todo | **clara** — só hero e CTA final são escuros |
| Fundo | `#0a0a0a` preto | `#faf7f2` off-white quente |
| Acento | `#f63600` laranja | `#7b8b80` sálvia + `#b68e7c` rosé |
| Display | Anton, condensada, **caixa-alta** | Fraunces, serifada alta, **caixa mista** |
| Textura hero | linhas de fuga (velocidade) | borrão de luz que desce com a rolagem |
| Raio dos cards | 4px seco | 8px |
| Seções | 11 | 12 (entrou "Investimento") |

**Por que Fraunces em caixa mista:** serifada de display em caixa-alta fica pesada
e datada. A quebra de caixa é o que separa "editorial de bem-estar" de "cartaz".
Fraunces cumpre o papel da Cormorant da referência, com mais personalidade.

## Ritmo das seções

A referência é clara do começo ao fim e usa **faixas sálvia sólidas** como respiro.
Aqui é o mesmo, com dois blocos escuros mantidos do Dark Impact:

```
§2  HERO           escuro #22271f  ← foto + gradientes
§3  credenciais    claro  #faf7f2
§4  método         claro  #faf7f2  (cards #efebe2)
§5  sobre mim      claro  #efebe2
§6  como funciona  FAIXA SÁLVIA #4d5c52  ← o respiro
§7  comparativo    claro  #faf7f2  (coluna vencedora em sálvia sólida)
§8  depoimentos    claro  #faf7f2
§9  investimento   claro  #efebe2  (cards brancos)
§10 CTA final      escuro #22271f
§11 contato        claro  #faf7f2
§12 rodapé         escuro #22271f
```

A nav pill inverte sozinha (`.on-dark`) enquanto passa sobre seção escura —
sem isso ela sumiria em cima do hero.

### Comparativo (§7) — hierarquia entre as colunas

As duas colunas eram visualmente quase idênticas: mesma largura, mesmo fundo
transparente, mesmo tamanho de título. A única diferença era um filete de 2px
no topo — invisível de relance, então o comparativo não comparava nada.

Agora a coluna vencedora **é o único bloco escuro no meio da página clara**:

| | Perdedora (`.comp-col--off`) | Vencedora (`.comp-col--on`) |
|---|---|---|
| Fundo | transparente | sálvia sólida `#4d5c52` |
| Título | 17px, cinza `--ink-3` | 21px, off-white |
| Itens | 14px, **riscados** | 15px, off-white |
| Elevação | nenhuma | sombra + selo "Recomendado" |
| Posição | recuada 18px (≥768px) | alinhada ao topo |

O `line-through` nos itens da esquerda faz o trabalho semântico: aquilo é o que
a paciente **não** vai viver. O recuo de 18px só existe no desktop; no mobile as
colunas empilham e a ordem já conta a história (problema → solução).

Contraste conferido na coluna escura: título 6.62:1, itens 6.08:1, selo 6.62:1.

### Borrão do hero (`.hero-glow`)

Um halo terracota+sálvia desfocado (`blur(60px)`) no topo do hero. Desce até
190px e some (opacidade 1 → 0.15) conforme o hero sai da tela.

O progresso vem da variável `--scroll` (0 a 1), escrita pelo script **dentro do
mesmo `requestAnimationFrame` da nav** — de propósito, pra não criar um segundo
listener de `scroll` concorrente. Em `prefers-reduced-motion` ele congela no
lugar, mas continua visível.

Substituiu os círculos concêntricos que existiam antes: os arcos cruzavam o
rosto da foto e apareciam como riscos claros sobre o fundo.

## ⚠ Antes de publicar — o que ainda falta

### 1. Número do WhatsApp (bloqueia tudo)
Os **11 CTAs** apontam para o placeholder `5585000000000`. Nada converte até isso ser trocado.

```bash
# na pasta da LP, com o número real (DDI+DDD, só dígitos):
sed -i 's/5585000000000/5585999999999/g' index.html
```
Trocar também o texto visível `(85) 0 0000-0000` na seção §11 (Contato).

### 2. Fotos — 2 de 6 aplicadas

**Já entraram** (em `img/`, WebP + JPG de fallback, com `srcset`):

| Onde | Arquivo | Origem |
|---|---|---|
| §2 Hero | `hero-{700,1000,1600}` | foto de jaleko |
| §5 Sobre | `sobre-{600,900}` | foto com a toranja |

⚠️ **As duas têm marca d'água de IA generativa do Gemini** (o brilho ✦ no canto).
Confirmar com a Patrícia se ela topa publicar foto gerada por IA — numa LP de
saúde, se a paciente chegar na consulta e for outra pessoa, quebra exatamente a
confiança que a página tenta construir. A foto de jaleco, aliás, **não parece
a mesma pessoa** do Instagram dela. O ideal é substituir por foto real.

O hero **não é recortado no build**: entrega a foto original e o enquadramento
sai do `object-position` + a regra `.hero-media` (coluna de 54% à direita no
desktop, fundo inteiro no mobile). Tentei montar um canvas 16:9 antes e ficou
ruim — espelhar/esticar duplicava a Patrícia e criava emenda visível.

**Mapa (§11): pronto.** `<iframe>` do Google Maps apontando para
`-3.8797373,-38.6123937`, coordenadas resolvidas a partir do link curto que a
Patrícia mandou — e que batem com o endereço já escrito na página. Vai com
`loading="lazy"` (o iframe do Maps é pesado e fica no fim da página) e com
`title` descritivo para leitor de tela. O botão "Traçar rota" também passou a
usar as coordenadas em vez de busca por texto, que é mais confiável.

**Ainda faltam 3**, todas com `<div class="ph" data-ph="...">`:

| Onde | O que | Proporção |
|---|---|---|
| §4 card 01 | Prato equilibrado / alimentação | 16:10 |
| §4 card 02 | Treino ou fontes de proteína | 16:10 |
| §4 card 03 | Consulta / conversa | 16:10 |

Quando as 3 entrarem, **apague o bloco `<style>` dos `.ph`** no `<head>`.

### 3. Depoimentos (§8) — decisão dela
A seção está com **vagas em branco de propósito**. Dois motivos:
o perfil no Doctoralia ainda não tem avaliação publicada, e depoimento de
paciente é conteúdo regulado — a Res. CFN 599/2018 veda "antes e depois" e
promessa de resultado.

**Ou** ela envia depoimentos reais com autorização, **ou** apague a §8 inteira
e a entrada correspondente no menu. Não invente texto aqui.

**Como preencher:** cada depoimento é um `<li class="depo-card">` dentro de
`#depo-track`. Duplique o `<li>`, troque texto, nome e data — o carrossel se
ajusta sozinho, não há contagem fixa em lugar nenhum.

**O carrossel é nativo e infinito** (`overflow-x` + `scroll-snap`), sem
biblioteca. As setas apenas empurram o `scrollLeft`, então arrastar, roda do
mouse, trackpad e setas do teclado continuam funcionando.

**Como o loop funciona:** o script clona os cards em blocos (quantos couberem
na viewport com folga) e começa no bloco do meio. Quando o scroll cruza a
fronteira de um bloco, o `scrollLeft` salta um bloco inteiro para trás ou para
frente — como os cards são idênticos, o salto é invisível e o carrossel nunca
chega ao fim. O número de clones é recalculado no `resize` (12 em desktop, 8 em
mobile, por exemplo).

Detalhes que importam:

- O salto desliga o `scroll-snap` por um instante; com o snap ligado o navegador
  briga com o reposicionamento e o pulo fica visível.
- Os clones levam `aria-hidden="true"` e `tabindex="-1"` nos links, para não
  serem lidos nem focados várias vezes. Verificado: 0 focáveis indevidos.
- **Sem JS** os clones não são criados e sobra um carrossel comum, finito e
  arrastável — degrada bem.
- As setas nunca desabilitam, já que sempre há para onde ir.

### 4. Conferir com ela
- **CRN-11 22369** — veio do perfil no Doctoralia; o Instagram só diz "CRN 22369". Confirmar o número da região.
- **Preços R$ 250 / R$ 180** — do Doctoralia em 06/08/2026. Confirmar se seguem válidos.
- **Endereço** Rua 38, 196 — Altos, Jereissati Setor A, Maracanaú/CE 61900-640. ✅ confirmado pelo link do Maps que ela enviou.
- **Horário de atendimento** — hoje está genérico ("combinados por WhatsApp").

### 5. Tailwind CDN → build
O `cdn.tailwindcss.com` é só protótipo. Compilar antes de subir.

### 6. Imagem de OG
Falta `og:image`. Gerar 1200×630 e adicionar no `<head>` (senão o link
compartilhado no WhatsApp vai sem cartão).

## Conteúdo — de onde veio

Tudo factual foi tirado do perfil dela no Doctoralia e do Instagram
[@nutripatyalmeida\_](https://instagram.com/nutripatyalmeida_):
nutrição comportamental, emagrecimento, hipertrofia, saúde integrada,
"nutrição individualizada e humanizada", foco na pessoa e não no diagnóstico,
sem dieta restritiva, comer sem culpa, mudança gradual, presencial + teleconsulta,
particular sem convênio.

O texto de vendas (manchetes, bullets, comparativo) foi escrito em cima disso —
**revisar com ela antes de publicar**, porque coloca palavras na boca dela.

## Acessibilidade — leia antes de mexer nas cores

Auditado no navegador percorrendo os nós de texto: **0 reprovas de contraste AA**.

**A paleta da Bruna Burti é bonita mas tem contraste fraco.** Vários tons dela
reprovam AA quando usados em texto pequeno, então cada acento existe em duas
versões: a original (decorativa, ou sobre fundo escuro) e uma rebaixada para
texto. Os tons rebaixados mantêm o **mesmo matiz oklch** dela — só desce o L.

| Papel | Bruna | Aqui | Por quê |
|---|---|---|---|
| Sálvia botão | `#7b8b80` | `#45584d` | o dela com texto branco dá **3.1:1** |
| Faixa sálvia | `#7b8b80` | `#4d5c52` | idem, texto claro por cima |
| Rosé eyebrow (claro) | `#b68e7c` | `#755141` | o dela dá **2.75:1** sobre off-white |
| Legenda | `#8a9098` | `#555c63` | o dela dá **3.0:1** |

Sobre as **seções escuras** o `#b68e7c` e o `#7b8b80` originais passam e são usados
tal e qual — é lá que a paleta dela brilha.

Uma pegadinha que apareceu: os cards de vidro claro (`rgba(250,247,242,.12)`) sobre
a faixa sálvia **clareavam o fundo** e derrubavam o texto para 4.16:1. Por isso o
vidro daquela faixa é escuro (`.card-vidro`), não claro.

Se mexer nas cores, rode a auditoria de novo antes de publicar.

Também verificado: foco visível no teclado, `prefers-reduced-motion` desliga todas
as animações, menu mobile com `aria-expanded`, sem scroll horizontal em 390px,
reveal funciona sem JS (progressive enhancement).
