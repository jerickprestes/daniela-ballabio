# Ballabio Arquitetura & Design — Site institucional

Site institucional de uma página (one-page) da **Ballabio Arquitetura & Design**, com foco no serviço **Gerenciamento de Obras** (projetos arquitetônicos e urbanísticos, design de interiores, paisagismo e regularização, com curadoria e conexão com fornecedores, fabricantes e prestadores de serviços).

Cliente: **Daniela Ballabio** (design de interiores).

---

## Stack

Site **100% estático**, sem build, sem framework e sem dependências de servidor.

- **HTML/CSS/JS puro** (`index.html`, `main.css`)
- **[GSAP](https://gsap.com) + ScrollTrigger** (`assets/js/`), baixados localmente (não usa CDN). Hoje animam só o "levantar" dos pilares e a barra de progresso B2B/B2C
- **Fonte: Encode Sans Expanded** (`assets/fonts/`), em `.woff2`, pesos 100/300/400/500/600/700/800 (Google Fonts, self-hosted)
- Para rodar, basta abrir `index.html` no navegador ou subir a pasta para qualquer hospedagem estática

## Estrutura de arquivos

```
index.html                        — a página inteira (todas as seções + JS no fim)
main.css                          — todo o CSS do site
assets/
  fonts/                          — Encode Sans Expanded (.woff2)
  js/
    gsap.min.js
    ScrollTrigger.min.js
  images/
    logo-ballabio-gold.webp       — logo dourado do hero
    logo-rinaldo.svg              — logo do parceiro Rinaldo em tom escuro (#130E01), usado no cabeçalho da faixa Render em destaque
    favicon.svg / favicon.ico / apple-touch-icon.png — ícone da marca em degradê dourado sobre `#130E01` (o .ico tem 16/32/48px; o apple-touch-icon é quadrado, 180px, porque o iOS arredonda sozinho)
    sobre.jpg                     — foto de fundo da seção Serviços (com overlay escuro)
    cta-bg.jpg                    — foto de fundo da seção Projetos/galerias (com overlay escuro)
    diferencial-bg.jpg            — NÃO usada atualmente (mantida no projeto)
    hero-frame.jpg                — NÃO usada atualmente (mantida no projeto)
    partners/                     — 10 logos de parceiros (SVG, e PNG no caso da Ornare)
    projetos/                     — 63 imagens (projeto-01.jpg a projeto-63.jpg); 42 são usadas nas galerias
    destaque/                     — 4 renders do Rinaldo com logo (rinaldo-1 a rinaldo-4.jpg), usados na faixa Render em destaque
    obras/                        — 21 fotos de obra (obra-01.jpg a obra-21.jpg) + miniaturas -thumb (hoje não usadas)
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
| — | **Render em destaque** (`#destaque`) | Claro | Faixa larga em slider logo abaixo do hero, com logo do Rinaldo, e subtítulo "Parceria Destaque" (sem o título "Render em destaque" visível; o rótulo continua no aria-label) |
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

- [ ] **Fotos definitivas das galerias**: Render de casa, Paisagismo e Planejados usam fotos provisórias de `projetos/`, separadas por tema. **Render em destaque** já usa as 4 fotos do Rinaldo (`assets/images/destaque/`) e **Fotos de Obras** usa fotos reais de obra. Troque editando `GALLERIES`
- [ ] **Número de WhatsApp**: o código usa `5512981315103`. Confirmar se é o número final do cliente
- [ ] **Título "Como trabalhamos"** na seção Sobre: o PDF de edições só trazia o texto novo, então o título foi escolhido como neutro. Ajustar se houver um definido
- [ ] **Parágrafo da seção Projetos** ainda diz "em breve com fotos dos projetos reais da Daniela"
- [ ] **Alternância de cores**: Diferencial, Para quem é e Parceiros ficam claras em sequência (decisão do cliente: Parceiros claro, galerias escuro)
- [ ] Imagens não usadas (`diferencial-bg.jpg`, `hero-frame.jpg`, miniaturas `obra-XX-thumb.jpg` e 21 fotos de `projetos/`) podem ser removidas se não forem necessárias
- [ ] Sem CMS: qualquer troca de foto, texto ou logo exige editar o HTML/CSS/JS. Futuro painel administrativo em PHP (cPanel/Hostinger) para controlar a quantidade de fotos das galerias e os textos das seções

## Como rodar localmente

Não precisa de instalação. Abra `index.html` no navegador ou sirva a pasta com qualquer servidor estático:

```bash
python3 -m http.server 8000
```

e acesse `http://localhost:8000`.
