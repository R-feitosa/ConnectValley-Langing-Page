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

Duas atualizações do Connect 2026 já estão implementadas no `index.html`, mas
dependem de arquivos que ainda não chegaram. Enquanto o arquivo não existe, o
bloco correspondente é removido da página em tempo de execução
(`initOptionalMedia()`), então nada aparece quebrado. Basta colocar o arquivo no
caminho abaixo — não há nenhuma outra alteração a fazer:

| Arquivo | Onde aparece |
| --- | --- |
| `assets/novidades-connect-2026.mp4` | bloco "Novidades Connect 2026" na seção `#videos` (o vídeo do diálogo do Dr. e da Dra.) |
| `assets/taisi.jpg` | card da Dra. Taísi no carrossel `#palestrantes` (retrato 3:4, mesmo enquadramento dos demais) |

Ainda em aberto, e sem estrutura pronta porque dependem de material novo:

- **Foto do Severino** — trocar o arquivo `assets/severino-neto.jpg` pela versão
  atualizada, mantendo o mesmo nome e enquadramento 3:4.
- **Logos de patrocinadores faltantes** — seguir "Atualizando patrocinadores"
  abaixo para cada marca nova.

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
