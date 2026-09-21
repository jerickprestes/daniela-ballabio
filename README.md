# Ballabio Arquitetura & Design — Site institucional

Site institucional de uma página (one-page) para a **Ballabio Arquitetura & Design**, com foco no serviço **Facility Representações** (curadoria e conexão com fornecedores, fabricantes e prestadores de serviços para projetos de arquitetura e design de interiores).

Cliente: **Daniela Ballabio** (design de interiores).

---

## Stack

Site **100% estático** — sem build step, sem framework, sem dependências de servidor.

- **HTML/CSS/JS puro** (`index.html`, `main.css`)
- **[GSAP](https://gsap.com) + ScrollTrigger** (`assets/js/`) — animações e parallax, baixados localmente (não via CDN)
- **Fonte: Encode Sans Expanded** (`assets/fonts/`) — hospedada localmente em `.woff2`, pesos 100/300/400/500/600/700/800 (Google Fonts, self-hosted)
- Nenhuma dependência de build (Webpack, Vite etc.) — basta abrir `index.html` num navegador ou subir a pasta inteira pra qualquer hospedagem estática

## Estrutura de arquivos

```
index.html                        — a página inteira (todas as seções)
main.css                          — todo o CSS do site
assets/
  fonts/                          — Encode Sans Expanded (.woff2, vários pesos)
  js/
    gsap.min.js
    ScrollTrigger.min.js
  images/
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

```bash
python3 -m http.server 8000
```

e acessar `http://localhost:8000`.
