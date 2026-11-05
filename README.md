# Ballabio Arquitetura & Design — Site institucional

<<<<<<< HEAD
Site institucional de uma página (one-page) da **Ballabio Arquitetura & Design**, com foco no serviço **Gerenciamento de Obras** (projetos arquitetônicos e urbanísticos, design de interiores, paisagismo e regularização, com curadoria e conexão com fornecedores, fabricantes e prestadores de serviços).
=======
Site institucional de uma página (one-page) para a **Ballabio Arquitetura & Design**, com foco no serviço **Facility Representações** (curadoria e conexão com fornecedores, fabricantes e prestadores de serviços para projetos de arquitetura e design de interiores).
>>>>>>> 7c119abc339ce6eb8a030f33b2e3c753559ffe5d

Cliente: **Daniela Ballabio** (design de interiores).

---

## Stack

<<<<<<< HEAD
Site **100% estático**, sem build, sem framework e sem dependências de servidor.

- **HTML/CSS/JS puro** (`index.html`, `main.css`)
- **[GSAP](https://gsap.com) + ScrollTrigger** (`assets/js/`), baixados localmente (não usa CDN). Hoje animam só o "levantar" dos pilares e a barra de progresso B2B/B2C
- **Fonte: Encode Sans Expanded** (`assets/fonts/`), em `.woff2`, pesos 100/300/400/500/600/700/800 (Google Fonts, self-hosted)
- Para rodar, basta abrir `index.html` no navegador ou subir a pasta para qualquer hospedagem estática
=======
Site **100% estático** — sem build step, sem framework, sem dependências de servidor.

- **HTML/CSS/JS puro** (`index.html`, `main.css`)
- **[GSAP](https://gsap.com) + ScrollTrigger** (`assets/js/`) — animações e parallax, baixados localmente (não via CDN)
- **Fonte: Encode Sans Expanded** (`assets/fonts/`) — hospedada localmente em `.woff2`, pesos 100/300/400/500/600/700/800 (Google Fonts, self-hosted)
- Nenhuma dependência de build (Webpack, Vite etc.) — basta abrir `index.html` num navegador ou subir a pasta inteira pra qualquer hospedagem estática
>>>>>>> 7c119abc339ce6eb8a030f33b2e3c753559ffe5d

## Estrutura de arquivos

```
<<<<<<< HEAD
index.html                        — a página inteira (todas as seções + JS no fim)
main.css                          — todo o CSS do site
assets/
  fonts/                          — Encode Sans Expanded (.woff2)
=======
index.html                        — a página inteira (todas as seções)
main.css                          — todo o CSS do site
assets/
  fonts/                          — Encode Sans Expanded (.woff2, vários pesos)
>>>>>>> 7c119abc339ce6eb8a030f33b2e3c753559ffe5d
  js/
    gsap.min.js
    ScrollTrigger.min.js
  images/
<<<<<<< HEAD
    logo-ballabio-gold.webp       — logo dourado do hero
    favicon.svg / favicon.ico / apple-touch-icon.png — ícone da marca em degradê dourado sobre `#130E01` (o .ico tem 16/32/48px; o apple-touch-icon é quadrado, 180px, porque o iOS arredonda sozinho)
    sobre.jpg                     — foto de fundo da seção Serviços (com overlay escuro)
    cta-bg.jpg                    — foto de fundo da seção Projetos/galerias (com overlay escuro)
    diferencial-bg.jpg            — NÃO usada atualmente (mantida no projeto)
    hero-frame.jpg                — NÃO usada atualmente (mantida no projeto)
    partners/                     — 10 logos de parceiros (SVG, e PNG no caso da Ornare)
    projetos/                     — 63 imagens (projeto-01.jpg a projeto-63.jpg); 42 são usadas nas galerias
    obras/                        — 11 fotos de obra (obra-01.jpg a obra-11.jpg) + miniaturas -thumb (hoje não usadas)
```

## Paleta de cores

Definida em `:root` no topo do `main.css`. Dourado e preto/branco, com o escuro e o claro fixos da marca.

| Variável | Valor | Papel |
|---|---|---|
| `--ink` | `#130E01` | Tom mais escuro: fundos escuros, texto sobre fundo claro, HUD do hero |
| `--ink-2` | `#2E2614` | Escuro secundário: texto corrido em seções claras, formulário |
| `--paper` | `#FFFEFB` | Tom mais claro: fundos claros, texto sobre fundo escuro |
| `--paper-2` | `#F3EEE2` | Claro secundário: painel B2C |
| `--gold` | `#C9A24A` | Dourado sólido (linhas, ícones, foco) |
| `--gold-soft` | `#E6C97A` | Dourado claro, usado sobre fundo escuro (eyebrows, itálicos, setas) |
| `--gold-deep` | `#8A6A1F` | Dourado escuro, usado sobre fundo claro (eyebrows, chips, títulos) |
| `--gold-grad` | degradê `#A97C26 → #D9B45C → #F3DC8A → #BE8F36` | Botões, selos, barras de progresso, ícones dos serviços, marcadores |
| `--gold-grad-hover` | versão mais clara do degradê | Hover dos botões |
| `--gold-line` | degradê horizontal com pontas transparentes | Divisórias finas douradas |
| `--noise` | SVG de ruído embutido (data URI) | Textura leve dos botões dourados |

Os botões dourados (`.btn-primary`, `.submit-btn`) têm degradê, borda clara fina, sombra interna e o ruído aplicado por `::before` (`opacity: 0.10`, com `mix-blend-mode: multiply`; reduzir o `opacity` clareia o dourado). Sobre fundo claro, texto corrido fica em `--ink`/`--ink-2` (o dourado tem pouco contraste sobre branco, então só aparece em elementos decorativos, títulos grandes e botões).

⚠️ **Ao trocar a paleta**: restam alguns `rgba(...)` fixos no CSS (overlays, sombras, bordas). Os componentes RGB atuais são `19,14,1` (`--ink`), `255,254,251` (`--paper`) e `201,162,74` (`--gold`). Busque por esses valores para atualizar.

## Largura do conteúdo

Todas as seções usam a mesma margem lateral, definida pela variável `--section-inset` no `:root` do `main.css`:

- **Desktop:** `15vw` de cada lado, ou seja, o conteúdo ocupa **70vw**
- **Celular (≤900px):** `12vw` de cada lado, a mesma margem que a seção Serviços já tinha

O `padding` lateral vem da própria `<section>` (e do rodapé); a classe `.wrap` não tem mais largura máxima nem padding. Para alargar ou estreitar o site inteiro, basta mudar essa variável. Só o cabeçalho fixo (`.topnav`) e o marquee de parceiros fogem da regra: o cabeçalho usa `6vw`, e o marquee fica dentro da margem com as pontas esmaecidas.

## Seções da página (nessa ordem)

| # | Seção | Fundo | Conteúdo |
|---|---|---|---|
| 1 | **Hero** (`#hero`) | Claro (`--paper`, sólido) | Só o logo dourado, o nome ("Ballabio Arquitetura & Design") e o slogan, centralizados. Texto e menu em preto |
| — | **Render em destaque** (`#destaque`) | Claro | Faixa larga em slider logo abaixo do hero |
| 2 | **Sobre** (`#sobre`) | Claro | Texto "Como trabalhamos" + bloco do serviço **Gerenciamento de Obras** (lista de 4 itens, "Sob orçamento" e botão de contato), sem cards |
| 3 | **Serviços** (`#servicos`) | **Escuro** (foto `sobre.jpg` + overlay) | Grid de 11 serviços + 1 item de fechamento |
| 4 | **Diferencial** (`#diferencial`) | Claro | Título "Por que nos contratar?", subtítulo "Facilitamos o seu projeto" e 5 pilares |
| 5 | **Para quem é** (`#publico`) | Claro | Dois painéis B2B / B2C |
| 6 | **Parceiros** (`#parceiros`) | Claro | Marquee infinito com 10 logos, cada um com tag de função + chips de região (Vale do Paraíba, Litoral, São Paulo) |
| 7 | **Projetos / galerias** (`#projetos`) | **Escuro** (foto `cta-bg.jpg` + overlay) | 4 sliders (Render de casa, Fotos de Obras, Paisagismo, Planejados), vídeos das obras (`#galeria-obras`), frase de propósito e CTA |
| 8 | **Rodapé** (`#rodape`) | Claro | Slogan e direitos |

O formulário de contato fica num painel que sobe da base da tela (`#contactDrawer`) e abre por qualquer elemento com o atributo `data-open-contact` (botão flutuante do WhatsApp e todos os "Entre em contato").

## Parceiros e tags

Cada item do marquee tem o logo e uma tag de função logo abaixo (`.partner-tag`). A lista é duplicada no HTML para o loop contínuo; a segunda cópia fica com `aria-hidden`. Ao incluir ou remover um parceiro, altere as duas cópias.

| Logo | Parceiro | Tag |
|---|---|---|
| `dcoracao.svg` | D'Coração | Ambientes |
| `jf-home-solutions.svg` | JF Home Solutions | Automação residencial e predial |
| `villarta.svg` | Villarta | Elevadores |
| `masao.svg` | Masao | Paisagismo |
| `forccis.svg` | Fórccis | Projetos e Construções |
| `stilo.svg` | Stilo | Movelaria |
| `rinaldo.svg` | Rinaldo | Arquitetura e Urbanismo |
| `master-vale.svg` | Master Vale | Climatização e Refrigeração + 2ª tag menor "Autorizada oficial da LG" |
| `ornare.png` | Ornare | Movelaria planejada |
| `a-monogram.svg` | Arantes | Marmoraria |

## Galerias (sliders)

- Cada galeria é um `<div data-gallery="chave">` no `index.html`; as fotos ficam no objeto **`GALLERIES`**, no `<script>` no fim do arquivo (um caminho por linha). O JS monta sozinho os slides, os botões anterior/próximo e o arrastar com o mouse
- Chaves: `destaque` (faixa abaixo do hero), `render`, `obras`, `paisagismo`, `planejados`
- Desktop: 4 imagens por vez (a faixa em destaque mostra 1 grande, com a próxima aparecendo). ≤1000px: 2 por vez. ≤560px: 1 por vez
- Navegação: botões (avançam uma "página"), arrastar com o mouse (encaixa no slide mais próximo), rolagem por toque no celular e setas do teclado com o slider focado
- **Lightbox**: clicar numa imagem abre o lightbox, que navega só dentro daquela galeria (setas na tela e do teclado, `Esc` fecha). Um arraste não abre o lightbox
- **Vídeos das obras**: Reels do Instagram, definidos em `OBRAS_VIDEOS` (basta o ID do link). O embed só carrega enquanto o vídeo está na tela

## Comportamentos importantes

- **Header**: no topo da página, só o botão de menu aparece; ao rolar vira vidro claro e mostra o logo e a frase **"DANIELA BALLABIO - ARQUITETURA e DESIGN"** (texto, não imagem)
- **Menu off-canvas** escuro com links para as seções
- **Formulário de contato → WhatsApp**: ao enviar, o JS monta uma mensagem com os dados e abre `https://wa.me/<número>?text=...` numa nova aba. **Não há backend**. O número fica na variável `whatsappNumber` no fim do `index.html`
- **Botão flutuante do WhatsApp**: canto inferior direito, verde (fora da paleta de propósito); abre o formulário de contato
- **Barra B2B/B2C** preenche como progress bar ao entrar na tela

## Nomenclatura

O serviço agora se chama **Gerenciamento de Obras**. O nome antigo não aparece mais em nenhum lugar do site nem neste README. O título da página, o rodapé e a mensagem do WhatsApp continuam com "Ballabio Arquitetura & Design".

## Pendências conhecidas

- [ ] **Fotos definitivas das galerias**: Render em destaque, Render de casa, Paisagismo e Planejados usam fotos provisórias de `projetos/`, separadas por tema. Só **Fotos de Obras** usa fotos reais de obra. Troque editando `GALLERIES`
- [ ] **Número de WhatsApp**: o código usa `5512981315103`. Confirmar se é o número final do cliente
- [ ] **Título "Como trabalhamos"** na seção Sobre: o PDF de edições só trazia o texto novo, então o título foi escolhido como neutro. Ajustar se houver um definido
- [ ] **Parágrafo da seção Projetos** ainda diz "em breve com fotos dos projetos reais da Daniela"
- [ ] **Alternância de cores**: Diferencial, Para quem é e Parceiros ficam claras em sequência (decisão do cliente: Parceiros claro, galerias escuro)
- [ ] Imagens não usadas (`diferencial-bg.jpg`, `hero-frame.jpg`, miniaturas `obra-XX-thumb.jpg` e 21 fotos de `projetos/`) podem ser removidas se não forem necessárias
- [ ] Sem CMS: qualquer troca de foto, texto ou logo exige editar o HTML/CSS/JS. Futuro painel administrativo em PHP (cPanel/Hostinger) para controlar a quantidade de fotos das galerias e os textos das seções

## Como rodar localmente

Não precisa de instalação. Abra `index.html` no navegador ou sirva a pasta com qualquer servidor estático:
=======
    sobre.jpg                     — foto de fundo compartilhada (Hero + Sobre + Serviços, com parallax)
    diferencial-bg.jpg            — fundo da seção Diferencial
    cta-bg.jpg                    — fundo da seção Projetos
    hero-frame.jpg                — não usada atualmente (mantida no projeto)
    wordmark-dark.svg             — logo "Ballabio Arquitetura & Design" em curvas, versão escura
    wordmark-light.svg            — mesmo logo, versão clara
    partners/                     — 9 logos de parceiros, vetorizados e em azul-marinho (#0D1B2A)
      a-monogram.svg              — ⚠️ nome da marca ainda não confirmado (ver "Pendências")
      dcoracao.svg                — D'Coração Ambientes
      jf-home-solutions.svg       — JF Home Solutions
      villarta.svg                — Villarta Elevadores
      masao.svg                   — Masao Paisagismo
      forccis.svg                 — Fórccis Projetos e Construções
      stilo.svg                   — Stilo Movelaria
      rinaldo.svg                 — Rinaldo Arquiteto e Urbanista
      master-vale.svg             — Master Vale Climatização e Refrigeração
    projetos/                     — 53 fotos reais de projetos da Daniela (projeto-01.jpg a projeto-53.jpg)
prototypes/
  hero-radial.html + .css         — protótipo isolado da composição radial da hero (histórico de design, não usado em produção)
```

## Paleta de cores atual

Definida em `:root` no topo do `main.css`. Os **nomes das variáveis são só rótulos internos** (herdados de uma paleta anterior verde/teal) — não representam mais as cores reais.

| Variável | Hex | Papel |
|---|---|---|
| `--carbon-black` | `#0D1B2A` | Fundo escuro principal / texto sobre fundo claro |
| `--charcoal-brown` | `#1B263B` | Segunda cor escura (cards, superfícies) |
| `--sage-green` | `#FCA311` | Destaque principal — botões, CTAs, ícones (laranja) |
| `--deep-teal` | `#415A77` | Destaque secundário |
| `--muted-teal` | `#778DA9` | Destaque mais claro (hover, itálicos) |
| `--alabaster-grey` | `#D8DFE6` | Cinza-azulado claro (cards/seções claras) |
| `--white-smoke` | `#F5F6F4` | Fundo claro / texto sobre fundo escuro |

⚠️ **Importante ao trocar de paleta**: existem ~15 valores de cor **fixos** em `main.css` (formato `rgba(R,G,B,alpha)` ou `rgb(R G B / alpha%)`, nos overlays de fundo, sombras de cards etc.) que **não** são amarrados às variáveis CSS. Ao trocar a paleta, é preciso atualizar manualmente esses valores fixos também (buscar por `rgba(13,27,42` e `rgba(119,141,169` — os componentes RGB do carbon-black e do muted-teal atuais), ou eles ficam "presos" na cor antiga.

## Fonte / Logo

O wordmark (`wordmark-dark.svg` / `wordmark-light.svg`) não é uma imagem — é texto convertido em curvas vetoriais via `fontTools`, usando a fonte Encode Sans Expanded peso 100 (thin), caixa alta, com tracking (espaçamento entre letras) de ~0.14em. Ao trocar o nome da marca, os SVGs precisam ser regenerados com esse mesmo processo pra manter a proporção e o estilo — não é um simples find-and-replace de texto.

## Seções da página (nessa ordem)

1. **Hero** (`#hero`) — diagrama radial: logo em glassmorphism no centro (com o ícone + wordmark sobrepostos acima do vidro), texto de abertura abaixo. No mobile, o diagrama vira um logo pequeno simples.
2. **Sobre** (`#sobre`) — texto institucional + 3 cards de preço (1 ativo "Facility Representações", 2 bloqueados "Em breve")
3. **Serviços** (`#servicos`) — grid de 11 serviços + 1 card de fechamento, cards escuros sólidos, sem efeito hover
4. **Diferencial** (`#diferencial`) — 5 pilares numerados
5. **Para quem é** (`#publico`) — dois painéis B2B / B2C
6. **Parceiros** (`#parceiros`) — marquee infinito com os 9 logos de parceiros (grayscale → cor no hover) + chips de região atendida
7. **Projetos** (`#projetos`) — galeria masonry com carregamento sob demanda (16 fotos por clique em "Carregar mais fotos", de um total de 53) + bloco de citação/propósito + CTA
8. **Contato** (`#brainstorm`, no `<footer>`) — formulário que, ao enviar, monta uma mensagem e abre o WhatsApp com o texto pronto

## Comportamentos importantes

- **Header**: transparente no topo da página (logo/marca escondidos, só o botão de menu aparece); vira vidro claro com a marca visível ao rolar
- **Parallax**: Hero + Sobre + Serviços compartilham uma única foto de fundo (`sobre.jpg`) com efeito de parallax fixo (a imagem "flutua" fixa na tela enquanto o conteúdo rola por cima)
- **Galeria**: as fotos **não** são todas carregadas de uma vez — só as 16 primeiras vêm no HTML inicial; o resto é injetado via JavaScript ao clicar em "Carregar mais fotos" (lista completa de dados em `PROJECT_PHOTOS`, dentro do `<script>` no fim do `index.html`). O lightbox (clique numa foto pra ampliar) sempre navega pela lista completa de 53, independente de quantas já foram carregadas na tela
- **Formulário de contato → WhatsApp**: ao enviar, o JS monta uma mensagem com os dados preenchidos e abre `https://wa.me/5512981452330?text=...` numa nova aba. **Não há backend** — é só esse redirecionamento
- **Botão fixo do WhatsApp**: canto inferior direito, sempre visível, mesmo número (`5512981452330`)

## Pendências conhecidas

- [ ] **Nome da marca do `a-monogram.svg`** não foi confirmado — está com `alt="Parceiro"` como texto temporário no marquee
- [ ] A pasta `prototypes/` contém um protótipo antigo da hero radial, mantido só como histórico de design — pode ser removida com segurança se não for mais necessária
- [ ] Sem sistema de gerenciamento de conteúdo (CMS): qualquer troca de foto, texto ou logo exige editar o HTML/CSS diretamente. Ver `handoff.md` para a conversa sobre um possível painel administrativo em PHP, caso a hospedagem final seja cPanel/Hostinger

## Como rodar localmente

Não precisa de servidor nem de instalação. Basta abrir `index.html` direto no navegador, ou servir a pasta com qualquer servidor estático simples, por exemplo:
>>>>>>> 7c119abc339ce6eb8a030f33b2e3c753559ffe5d

```bash
python3 -m http.server 8000
```

<<<<<<< HEAD
e acesse `http://localhost:8000`.
=======
e acessar `http://localhost:8000`.
>>>>>>> 7c119abc339ce6eb8a030f33b2e3c753559ffe5d
