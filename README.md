# dWallet / DrumWave — site institucional (2026)

Site de marketing estático, multi-página, sem build step. Marca dWallet
("Your life creates data"), empresa DrumWave.

> **Status: primeira versão, ainda em ajustes.** Este repositório está sendo
> entregue à equipe de TI para colocar em produção, mas o conteúdo e o código
> ainda vão passar por revisões antes da versão final. Ver [O que falta](#o-que-falta--próximos-passos)
> abaixo.

## Para a equipe de TI

Este é um site **100% estático** — HTML, CSS e JS puros, sem Node, sem
build, sem dependências de servidor. Qualquer host de arquivos estáticos
serve (GitHub Pages, Netlify, Vercel, Hostinger, S3, etc.).

- **Domínio final:** ainda não decidido. Por isso pedimos uma **URL de
  teste/preview** (subdomínio temporário, GitHub Pages, ou o que for mais
  simples de configurar) para compartilhamento interno enquanto os ajustes
  continuam — antes de apontar o domínio definitivo.
- **Build:** nenhum. É só servir os arquivos da raiz do repositório.
- **Entry point:** [`index.html`](index.html).
- **Rotas:** todas as páginas são arquivos `.html` separados (sem router,
  sem SPA) — ver [Páginas](#páginas) abaixo.
- **HTTPS / redirecionamento www → apex (ou vice-versa):** a definir junto
  com a escolha do domínio final.

Se surgir dúvida sobre qual arquivo corresponde a qual URL, ou sobre como
apontar o domínio quando ele for definido, essas são as únicas
configurações que faltam — o código em si não exige nada além de servir
arquivos estáticos.

## O que é

Site institucional da dWallet/DrumWave, com:

- **Home** ([`index.html`](index.html)) — experiência de marca guiada por
  scroll, com atos animados e uma timeline de vida interativa (idade 18 →
  70), mostrando como dados pessoais acumulam valor ao longo da vida.
- **Business** ([`business.html`](business.html)) — página voltada a
  empresas/parceiros, apresentando o produto do ponto de vista B2B.
- **Contact** ([`contact.html`](contact.html)) — página de contato.
- **Resources** ([`resources.html`](resources.html) +
  [`resources/post.html`](resources/post.html)) — listagem e leitor de
  posts (notícias, imprensa, conteúdo institucional), alimentados por
  [`assets/data/resources.json`](assets/data/resources.json).
- **Privacy Policy** ([`privacy-policy.html`](privacy-policy.html)).

Todas as páginas compartilham nav/footer no mesmo padrão e linkam entre si.

## Estrutura do repositório

```
index.html              Home — experiência de scroll + timeline
business.html            Página Business (B2B)
contact.html             Página de contato
resources.html            Listagem de recursos/posts
resources/post.html       Leitor de post individual (?slug=...)
privacy-policy.html       Política de privacidade

assets/
  img/                   Imagens do site (fotos, ícones, logo)
  js/                    GSAP + ScrollTrigger, bundlados localmente
  data/resources.json    Dados dos posts de Resources (gerado via CMS/)

CMS/
  build_resources_json.py   Converte export CSV do Webflow em resources.json
  *.csv                      Export bruto do CMS antigo (Webflow)

Figma/                  Referência de design (NÃO faz parte do site publicado
                         — está no .gitignore, existe só localmente)

site-antigo/             Trechos do site anterior (Webflow) mantidos como
                         referência histórica (analytics, formulário
                         HubSpot). Não é carregado pelas páginas atuais —
                         ver site-antigo/README.md antes de reaproveitar
                         qualquer trecho.

download-images.sh       Baixa as fotos da home a partir de assets/img/manifest.json
```

## Como abrir localmente

Basta abrir `index.html` no navegador — não precisa de servidor.

- GSAP e ScrollTrigger já estão bundlados em `assets/js/`, então a animação
  funciona offline.
- Fontes (Titillium Web + Open Sans) vêm do Google Fonts e caem para a fonte
  padrão do sistema se estiver offline.
- As fotos da home já estão commitadas em `assets/img/`. Se algum arquivo
  faltar, os `<img>` caem automaticamente para a URL hospedada do
  `manifest.json` como fallback — rode `bash download-images.sh` para
  baixar tudo de novo localmente.

## Conteúdo de Resources (CMS)

`assets/data/resources.json` é gerado a partir de um export CSV do Webflow
antigo:

```bash
cd CMS
python3 build_resources_json.py
```

Isso lê o CSV mais recente na pasta e regrava
`assets/data/resources.json`. Rodar de novo sempre que o export do CMS for
atualizado. Hoje isso é um processo manual — não há integração automática
com nenhum CMS.

## O que já está pronto

- Home, Business, Contact, Resources (listagem + post) e Privacy Policy —
  navegáveis e linkadas entre si.
- Timeline interativa da home (drag, teclado, sincronizada com scroll).
- Responsivo (mobile/desktop) e respeita `prefers-reduced-motion`.
- Fotos e assets da home já commitados (não dependem de internet).
- Conteúdo de Resources carregado a partir de dados reais migrados do CMS
  antigo (Webflow).

## O que falta / próximos passos

Esta é uma **primeira versão** para revisão interna, não a versão final.
Ainda pendente:

- **Domínio final** — a decidir.
- **Analytics/tracking** — o site antigo tinha GA4, Google Ads, PostHog e
  Cookie Script (ver `site-antigo/README.md`); nada disso foi reimplementado
  ainda no site novo. Precisa de decisão consciente sobre quais manter.
- **Formulário de contato/newsletter** — o site antigo integrava com
  HubSpot; a página `contact.html` atual ainda precisa de uma revisão sobre
  como (ou se) isso será reconectado.
- **Revisão geral de conteúdo e copy** — textos, imagens e páginas ainda
  vão passar por ajustes.
- **SEO** — meta tags, sitemap, Search Console, JSON-LD de Organization
  (existia no site antigo) ainda precisam ser revisados/reintroduzidos.
- Design de referência em `Figma/` ainda está sendo usado como fonte de
  verdade para ajustes visuais pendentes (pasta local apenas, fora do git).

Qualquer ajuste feito a partir daqui deve ser tratado como iteração sobre
esta base — não como retrabalho do zero.
