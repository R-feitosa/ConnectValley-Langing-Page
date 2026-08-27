# Connect Valley — Landing Page

Site oficial do **Connect Valley**, evento de negócios e inovação do
R. Feitosa Group, realizado em **16 e 17 de outubro de 2026, em Sobral (CE)**.

Em produção: [connectvaley.com.br](https://connectvaley.com.br)

## Stack

Página única em HTML estático, sem etapa de build. Tudo que a página precisa
é carregado por CDN:

- **Tailwind CSS** — estilos utilitários
- **Swiper 11** — carrosséis de palestrantes e patrocinadores
- **Google Fonts** — Anton SC, Montserrat, Poppins e Syne

## Estrutura

```
.
├── index.html          # a landing page inteira, incluindo CSS e JS inline
├── .htaccess           # configuração Apache de produção (cPanel)
└── assets/
    ├── hero-video*.mp4 # vídeos de fundo do hero
    ├── foto-*.JPG      # galeria de edições anteriores
    ├── *.jpg           # retratos dos palestrantes
    ├── mapa-evento.jpg # mapa do local
    ├── cronograma-*    # programação dos palcos 360 e Work
    └── logos/          # patrocinadores agrupados por cota
        ├── diamante/
        ├── ouro_plus/
        ├── ouro/
        └── prata/
```

## Rodando localmente

Não há dependências para instalar. Basta servir a pasta:

```bash
python -m http.server 8000
```

E abrir `http://localhost:8000`. Abrir o `index.html` direto pelo navegador
também funciona, mas um servidor local evita diferenças de comportamento nos
vídeos e nos caminhos relativos.

## Deploy

A hospedagem é cPanel. O conteúdo desta pasta corresponde a `public_html/`
no servidor — publicar é sincronizar os arquivos, sem build.

## Assets pendentes

O bloco "Novidades Connect 2026" na seção `#videos` já está pronto, esperando só
o endereço do vídeo. Ele vem do YouTube: cole a URL em `data-youtube`, no
próprio bloco dentro do `index.html`.

```html
<div class="video-player ..." data-youtube="https://youtu.be/SEU_VIDEO" ...>
```

Serve qualquer forma de endereço — o link da barra do navegador, o de
compartilhar, o de incorporar, o de Shorts, ou o ID de 11 caracteres sozinho.
Enquanto o atributo estiver vazio, o bloco inteiro sai da página
(`initOptionalMedia()`), então nada quebra até o vídeo ir ao ar.

Ainda em aberto: as **logos de patrocinadores faltantes** — seguir "Atualizando
patrocinadores" abaixo para cada marca nova.

### Por que YouTube, e não um arquivo no repositório

Os dois vídeos do hero já somam cerca de 16 MB e são o maior peso da página.
Como o deploy copia `assets/` inteiro para `public_html`, todo MP4 versionado
vira disco no servidor e banda a cada visita — sem streaming adaptativo, que o
Apache não faz. O YouTube resolve os três pontos de uma vez.

O embed é preguiçoso de propósito: até o clique a página mostra só a miniatura e
o botão de play, e nada do YouTube é carregado. Isso mantém a página leve e evita
cookie de terceiro antes do consentimento — daí também o domínio
`youtube-nocookie.com` no iframe.

### Convenção dos retratos de palestrante

Os arquivos em `assets/` são JPEG de verdade, `1080x1440` (3:4, o mesmo
`aspect-ratio` do card), com nome em minúsculas no formato `nome-sobrenome.jpg`.
Vale conferir os três pontos, porque cada um já causou problema:

- **Extensão x conteúdo** — renomear um PNG para `.jpg` não converte nada. O
  navegador até exibe, mas o arquivo fica 10x maior. Converta de fato.
- **Maiúsculas** — o servidor é Linux e diferencia; `Tayse-Feitosa.jpg` não
  atende uma referência a `tayse-feitosa.jpg`.
- **Peso** — mirar em ~150 KB (JPEG progressivo, qualidade 82). Acima disso a
  imagem está pesando mais que o necessário para um card de 320 px.

## Pontos conhecidos a revisar

- O `.htaccess` foi versionado como está em produção e ainda carrega blocos
  herdados do WordPress que redirecionam rotas não encontradas para
  `index.php`, que não existe neste site.
- Falta `<meta name="description">` e tags Open Graph, o que prejudica SEO e
  a prévia dos links compartilhados.
- Os vídeos do hero somam cerca de 16 MB e são o maior peso da página.

## Atualizando patrocinadores

As cotas mudam a cada edição. Para trocar uma marca, coloque o arquivo na
pasta da cota correspondente em `assets/logos/` e atualize a referência no
carrossel dentro do `index.html`, mantendo o `alt` no formato
`Logotipo <Marca> — Patrocinador <Cota>`.
