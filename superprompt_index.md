# Superprompt — Sobral Memória `index.html`

## Contexto do Projeto
Você vai recriar o `index.html` de um site acadêmico chamado **"Sobral — Lugares de Memória"**. É um projeto da Faculdade Luciano Feijão (FLF), curso de Arquitetura e Urbanismo, disciplina de Tópico Interdisciplinar de Extensão 2. O tema central é o resgate das **memórias afetivas dos idosos** nos espaços públicos históricos de Sobral, Ceará.

---

## Stack e Arquitetura

- **HTML5 semântico** puro — sem frameworks
- CSS externo: `style.css` (design system com Custom Properties)
- JS externo: `script.js` (XMB canvas + tema + scroll)
- Ícones: **Flaticon UIcons Bold Rounded** via CDN
  ```html
  <link rel="stylesheet" href="https://cdn-uicons.flaticon.com/2.6.0/uicons-bold-rounded/css/uicons-bold-rounded.css" />
  ```
- Fontes Google: `Playfair Display` (serif, itálico para títulos) + `Cormorant Garamond` (serif para corpo)
- Imagens locais em `assets/`: `elderly_plaza_memory.png`, `elderly_river_memory.png`, `hands_old_photos.png`, `market_scene_memory.png`

---

## Classes do Design System (definidas no `style.css`)

| Classe | Uso |
|--------|-----|
| `.glass` | Card com Liquid Glass padrão (backdrop-filter + border translúcido) |
| `.glass-strong` | Glass mais opaco para cards de destaque |
| `.container` | `max-width: 1200px; margin: 0 auto; padding: 0 24px` |
| `.reveal` | Elemento com animação de entrada ao rolar (JS adiciona `.visible`) |
| `.sec-label` | Label de seção em maiúsculas com ícone UIcons |
| `.sec-title` | Título de seção com `font-family: Playfair Display`. `<em>` = cor âmbar itálica |
| `.sec-desc` | Parágrafo descritivo da seção |

### Tokens de cor (CSS Custom Properties)
```css
--amber / --amber-soft    /* dourado-quente primário */
--rose / --rose-soft      /* rosa-vinho */
--lavender / --lavender-soft /* lilás */
--teal / --teal-soft      /* verde-água */
--text-primary / --text-secondary / --text-tertiary
--glass-bg / --glass-bg-strong / --glass-border / --glass-highlight
```
O modo claro/escuro é gerenciado pelo `style.css` via `prefers-color-scheme` e atributo `data-theme` no `<html>`.

---

## Estrutura do `<body>`

```
<canvas id="xmb-canvas">           ← fundo animado (ribbons XMB)
<nav class="nav-bar">              ← fixo no topo, pill glassmorphism
<section id="hero">                ← tela cheia com badge, h1, subtitle, chips
<section id="sobre">               ← sobre a cidade: texto + imagem + 4 stats
<section id="lugares">             ← grid 3×2 de lugares de memória
<section id="vozes">               ← texto teórico + 3 depoimentos de idosos
<section id="metodologia">         ← 4 etapas do projeto + card institucional
<section id="galeria">             ← grid de fotos (1 tall + 4 normais)
<footer class="site-footer">       ← logo, descrição, tags
<script src="script.js">
```

---

## Detalhamento de Cada Seção

### NAV `.nav-bar`
- Logo italic `Sobral Memória` linkando para `#hero`
- Links: A Cidade (`#sobre`), Lugares (`#lugares`), Vozes (`#vozes`), Projeto (`#metodologia`), Galeria (`#galeria`)
- Cada link tem `<i class="fi fi-br-{icon}">` + `<span>` com texto (span some em mobile)
- Botão `#theme-btn` à direita: ícone `fi-br-moon` / `fi-br-sun`, `onclick="toggleTheme()"`

### HERO `#hero`
- `.hero-badge`: pill glass com `fi-br-shield-check` + "Tópico Interdisciplinar · Arquitetura & Urbanismo · FLF"
- `<h1 class="hero-title">`: "Sobral," + `<em>Lugar de Memória</em>` (em = gradiente âmbar→rose→lavander)
- `.hero-sub`: parágrafo italic descrevendo a proposta
- `.hero-chips`: 4 chips `.chip.glass`, cada um com `.chip-icon` (bg rgba + cor do token) + texto
  - Ícones: `fi-br-home` (âmbar), `fi-br-heart` (rose), `fi-br-clock` (teal), `fi-br-user` (lavender)
- `.scroll-cue`: `fi-br-angle-down` + "Role para explorar" (aria-hidden)

### SOBRE `#sobre`
- Header: `.sec-label` `fi-br-globe`, `.sec-title` com `<em>`, `.sec-desc`
- `.about-grid.reveal` com `.about-main` (colspan full):
  - `.glass-strong.about-text`: 3 parágrafos sobre Sobral
  - `.about-img`: imagem `assets/elderly_plaza_memory.png`
- 4 cards `.glass.stat.reveal`:
  - 2ª (ámbbar, `fi-br-marker`), 1773 (rose, `fi-br-calendar`), 200K+ (lavender, `fi-br-users`), IPHAN (teal, `fi-br-star`)

### LUGARES `#lugares`
- Header centralizado: `.sec-label` `fi-br-marker`, título, desc
- `.places-grid`: grid 3 colunas (2 em tablet, 1 em mobile)
- 6 `<article class="glass place reveal">`:

| Lugar | Imagem | Tag (ícone, cor) |
|-------|--------|-----------------|
| Praça da Sé | Wikimedia | `fi-br-home` âmbar |
| Teatro São João | Wikimedia | `fi-br-star` lavender |
| Estação Ferroviária | Wikimedia | `fi-br-building` rose |
| Rio Acaraú | `assets/elderly_river_memory.png` | `fi-br-leaf` teal |
| Mercado Central | `assets/market_scene_memory.png` | `fi-br-shop` âmbar |
| Igreja do Rosário | picsum | `fi-br-cross` teal |

Cada card tem: `.place-img` (foto), `.place-body` com `.place-tag`, `<h3 class="place-name">`, `.place-desc`, e `.place-quote` (`.place-quote-icon` `fi-br-comment-alt` + `<p>`com `<strong>` + citação em itálico)

### VOZES `#vozes`
- `.voices-grid`: 2 colunas
- Esquerda `.voices-text.reveal`: 3 parágrafos teóricos + `<blockquote class="quote-box glass">` com citação de Pierre Nora e `<cite>`
- Direita `.testimonials.reveal` (`role="list"`): 3 `.glass-strong.testimony` (`role="listitem"`)
  - M — Dona Maria das Graças, 78 anos, âmbar→rose
  - J — Seu João Bezerra, 83 anos, teal→lavender
  - L — Dona Lurdes Melo, 71 anos, rose→âmbar
  - Cada um: `.testimony-head` (avatar gradiente + `.testimony-info`), `.testimony-body` com `fi-br-quote-right`

### METODOLOGIA `#metodologia`
- `.steps-grid`: 4 cards `.glass.step.reveal`
  1. Pesquisa de Campo — `fi-br-search` âmbar
  2. Entrevistas com Idosos — `fi-br-microphone` rose
  3. Análise & Catalogação — `fi-br-list` lavender
  4. Divulgação Digital — `fi-br-laptop` teal
- `.glass-strong.org-card.reveal`: ícone `fi-br-graduation-cap` (gradiente âmbar→rose), nome "Faculdade Luciano Feijão — FLF", detalhes + 2 `.org-meta-item` (`fi-br-marker`, `fi-br-user`)

### GALERIA `#galeria`
- `.gallery-grid.reveal`: grid 3 colunas, 2 linhas
- `.gal.tall` (span 2 linhas): `assets/elderly_plaza_memory.png`
- 4 `.gal` normais: `assets/hands_old_photos.png`, `assets/elderly_river_memory.png`, `assets/market_scene_memory.png`, Catedral Wikimedia
- Cada gal tem `.gal-over` (overlay gradient para baixo) e `.gal-cap` (caption hover italic)

### FOOTER `footer.site-footer`
- `.glass-strong.footer-inner.reveal`
- `.footer-logo`: "Sobral — *Lugar de Memória*" (italic)
- `.footer-desc`: texto acadêmico
- `.footer-chips`: 6 badges pill glass
- `.footer-copy`: © 2025

---

## Regras de Acessibilidade
- `aria-hidden="true"` em todos os ícones decorativos e no `<canvas>`
- `aria-label` em `<nav>`, `<article>`, `<button>` e na lista de depoimentos
- `role="navigation"`, `role="list"`, `role="listitem"` nos elementos corretos
- `<blockquote>` + `<cite>` para a citação de Pierre Nora
- Hierarquia `h1` → `h2` → `h3` respeitada
- `loading="lazy"` em todas as `<img>`
- `onerror` fallback para imagens externas do Wikimedia

---

## Idioma e Tom
- Português brasileiro (pt-BR)
- Texto dos títulos em Playfair Display, corpo em Cormorant Garamond
- `<em>` sempre em itálico dentro dos títulos `<h1>` e `<h2>`
- Citações de idosos entre aspas duphas portuguesas (`"…"`) em font-style italic
- Estilo acadêmico-poético, evocativo e nostálgico
