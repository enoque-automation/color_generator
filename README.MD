# 🎨 Solid Color Generator

Ferramenta web para geração, visualização e exportação de cores sólidas em alta resolução. Sem dependências externas — apenas HTML, CSS e JavaScript puro.

---

## ✨ Funcionalidades

### Seleção de Cor
- **Entrada por código Hex** — digite diretamente no campo `#RRGGBB` (ex: `#181818`, `#FF4F4F`)
- **Quadro cromático interativo** — seletor de saturação e luminosidade com arrastar do mouse ou toque
- **Barra de matiz (Hue Strip)** — deslize para escolher qualquer tom do espectro de 0° a 360°
- **Histórico de cores** — últimas 20 cores utilizadas, exibidas como swatches clicáveis

### Informações da Cor
Cada cor selecionada exibe automaticamente:

| Campo | Descrição |
|---|---|
| **R · G · B** | Valores decimais dos canais vermelho, verde e azul (0–255) |
| **HSL** | Matiz (hue), saturação e luminosidade |
| **Luminância** | Luminância relativa calculada conforme WCAG 2.1 |
| **Contraste** | Indica se o texto ideal sobre a cor é claro (`↑ Claro`) ou escuro (`↓ Escuro`) |
| **Barra de luminância** | Indicador visual da posição da cor no espectro preto–branco |

### Exportação
- **Formatos de saída pré-definidos:** 800×800, 1080×1080, 1920×1920, 2560×2560, 3840×3840, 4096×4096
- **Tamanho customizado:** defina largura e altura livremente (até 8192 px)
- **Formato:** PNG, proporção 1:1
- **Nome do arquivo:** `solid-{HEX}-{LARGURA}x{ALTURA}.png` (ex: `solid-181818-1080x1080.png`)

### Interface
- Preview ao vivo em tempo real no painel principal
- Botão **Copiar** para área de transferência com feedback visual (toast)
- Layout responsivo com painel de controles deslizável
- Tema escuro com tipografia editorial

---

## 🚀 Como Usar

### 1. Abrir o arquivo
Basta abrir `color-generator.html` em qualquer navegador moderno. Nenhuma instalação ou servidor necessário.

```
Abrir com: Chrome, Firefox, Safari, Edge (versões modernas)
```

### 2. Selecionar uma cor

**Via código Hex:**
```
1. Clique no campo de texto ao lado do "#"
2. Digite os 6 caracteres hexadecimais (ex: FF4F4F)
3. A cor é aplicada em tempo real ao digitar
```

**Via quadro cromático:**
```
1. Arraste o cursor no quadro colorido para escolher saturação/luminosidade
2. Use a barra horizontal abaixo para mudar o tom (matiz)
3. Mouse e toque são suportados
```

**Via histórico:**
```
1. Clique em qualquer swatch exibido na seção "Histórico"
2. A cor é aplicada imediatamente
```

### 3. Definir o tamanho da imagem

```
• Clique em um dos botões pré-definidos: 800×800, 1080×1080, etc.
• Ou preencha os campos "largura" e "altura" com valores customizados
• O tamanho atual é exibido no rodapé
```

### 4. Baixar a imagem

```
• Clique no botão "Baixar Imagem"
• O arquivo PNG será salvo automaticamente
```

---

## 🗂 Estrutura do Arquivo

O projeto é um único arquivo HTML autocontido:

```
color-generator.html
│
├── <style>             — Todo o CSS (variáveis, layout, componentes)
│   ├── :root           — Design tokens (cores, fontes)
│   ├── Layout          — Header, main grid (preview + controls), footer
│   ├── Preview         — Quadro de preview 1:1 com sombra dinâmica
│   ├── Controls        — Painel lateral deslizável
│   ├── Hex Input       — Campo de entrada + botão copiar
│   ├── Color Info      — Grid de chips informativos
│   ├── Picker Canvas   — Quadro cromático e barra de matiz
│   ├── Size Selector   — Botões de tamanho + inputs customizados
│   ├── History         — Swatches de histórico
│   └── Toast           — Feedback de ações
│
└── <script>            — Toda a lógica em JavaScript puro
    ├── Estado          — currentHex, hue, outputW/H, histórico
    ├── Utilitários     — hexToRgb, rgbToHex, rgbToHsl, hslToRgb, luminance
    ├── applyColor()    — Aplica cor e atualiza toda a UI
    ├── drawColorPad()  — Renderiza o gradiente no canvas de seleção
    ├── drawHueStrip()  — Renderiza a barra de matiz
    ├── Interações      — Mouse e touch para picker e hue strip
    ├── Download        — Geração de canvas offscreen + link de download
    └── Histórico       — Gerenciamento e renderização dos swatches
```

---

## 🔧 Detalhes Técnicos

### Cálculo de Luminância
A luminância relativa segue a fórmula da especificação **WCAG 2.1**:

```
L = 0.2126 × R_lin + 0.7152 × G_lin + 0.0722 × B_lin
```

Onde cada canal linear é calculado como:
- Se `c/255 ≤ 0.03928`: `c_lin = c / 12.92`
- Caso contrário: `c_lin = ((c + 0.055) / 1.055) ^ 2.4`

### Indicação de Contraste
O limiar `L > 0.18` é usado para sugerir o texto sobre a cor:
- `↑ Claro` — use texto branco ou claro sobre a cor
- `↓ Escuro` — use texto preto ou escuro sobre a cor

### Canvas de Seleção de Cor
O quadro cromático é renderizado com dois gradientes sobrepostos via `Canvas API`:
1. **Horizontal:** branco → cor pura do matiz selecionado
2. **Vertical:** transparente → preto

A cor é lida com `getImageData()` no pixel exato do clique/toque.

### Exportação
A imagem é gerada em um `<canvas>` offscreen com as dimensões configuradas, preenchido com `fillRect()` na cor atual e exportado via `canvas.toDataURL('image/png')`.

### ResizeObserver
Os dois canvas (quadro cromático e barra de matiz) são monitorados via `ResizeObserver`, garantindo que se redesenhem corretamente em qualquer tamanho de janela.

---

## 🎨 Design

| Propriedade | Valor |
|---|---|
| Tema | Dark |
| Fonte display | [Syne](https://fonts.google.com/specimen/Syne) — 400, 600, 800 |
| Fonte mono | [DM Mono](https://fonts.google.com/specimen/DM+Mono) — 300, 400, 500 |
| Cor de fundo | `#0c0c0c` |
| Cor de superfície | `#141414` |
| Borda | `#222222` |
| Destaque | `#f0f0f0` |

---

## 🌐 Compatibilidade

| Recurso | Requisito |
|---|---|
| Canvas API | Todos os browsers modernos |
| ResizeObserver | Chrome 64+, Firefox 69+, Safari 13.1+ |
| Clipboard API | Chrome 66+, Firefox 63+, Safari 13.1+ (HTTPS ou localhost) |
| CSS Grid / Custom Properties | Todos os browsers modernos |
| Touch Events | Mobile browsers modernos |

> **Nota:** O botão **Copiar** requer que a página seja servida via `https://` ou `localhost` para que a Clipboard API funcione. Ao abrir como `file://`, pode não funcionar em alguns browsers.

---

## 📁 Arquivos

```
color-generator.html   — Aplicação completa (único arquivo)
README.md              — Esta documentação
```

---

## 📄 Licença

Projeto de uso livre. Sem dependências de terceiros (fontes carregadas via Google Fonts CDN).
