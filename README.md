# Futi Arena · Biscoitê × Neymar Jr.

Jogo de futebol de botão em 3D, feito para celular, da campanha **Biscoitê × Neymar Jr.**

- **Jogar sozinho:** contra o computador, em 4 níveis.
- **Jogar com um amigo:** 2 jogadores no mesmo aparelho.
- **Duas arenas:** Arena Futi (azul, de dia) e Arena Fúria (noturna, preto/branco/dourado), as duas com arquibancadas, torcida, refletores e painéis com as marcas Biscoitê e NJ.
- **Copa Futi:** campeonato mata-mata de 16 times (oitavas → quartas → semifinal → final). Quem vence a final libera o personagem **Dourado** em todos os modos.
- **Cartas Neymar Jr.:** sorteadas a cada partida, para você e para o rival (o sorteio é totalmente aleatório). Cada carta tem um poder; só as cartas **douradas** também aumentam os atributos do boneco.

É um **site 100% estático**: não tem servidor, banco de dados nem etapa de build. Basta servir os arquivos desta pasta.

---

## 1. Estrutura das pastas

```
futi-arena/
├── index.html              ← o jogo inteiro (código, texturas e fotos já estão dentro dele)
├── assets/
│   ├── three.min.js        ← motor 3D (three.js r128), servido localmente
│   ├── favicon.svg         ← ícone da aba do navegador
│   ├── og-image.png        ← imagem de pré-visualização ao compartilhar o link
│   └── fonts/
│       ├── fredoka.css     ← declara a fonte Fredoka
│       ├── fredoka-500.woff2
│       ├── fredoka-600.woff2
│       └── fredoka-700.woff2
├── vercel.json             ← regras de cache (não tem rewrite nem build)
├── .gitignore
└── README.md
```

Todos os caminhos são **relativos**, e o jogo não usa nenhum recurso externo: nada de CDN, Google Fonts ou API. Funciona em qualquer servidor estático.

---

## 2. Rodar no seu computador (passo a passo)

> Não dá para abrir o `index.html` com dois cliques: o navegador bloqueia alguns recursos quando o arquivo é aberto direto do disco (`file://`). Use um servidor local, como abaixo. Leva 1 minuto.

### Opção A: Python (já vem no Mac e na maioria dos Linux)
1. Abra o Terminal.
2. Entre na pasta do jogo:
   ```bash
   cd caminho/para/futi-arena
   ```
3. Inicie o servidor:
   ```bash
   python3 -m http.server 8080
   ```
4. No navegador, abra **http://localhost:8080**
5. Para parar o servidor, volte ao Terminal e aperte `Ctrl + C`.

### Opção B: Node.js
1. Na pasta do jogo, rode:
   ```bash
   npx serve .
   ```
2. Abra o endereço que aparecer no Terminal (normalmente **http://localhost:3000**).

### Testar no celular na mesma rede Wi-Fi
1. Descubra o IP do computador. No Mac: Ajustes → Wi-Fi → Detalhes. No Windows: `ipconfig`.
2. No celular, abra `http://SEU-IP:8080` (exemplo: `http://192.168.0.15:8080`).

---

## 3. Subir no GitHub (passo a passo)

1. Crie um repositório vazio no GitHub: **New repository**, nome `futi-arena`, **sem** README.
2. No Terminal, dentro da pasta `futi-arena`:
   ```bash
   git init
   git add .
   git commit -m "Futi Arena: primeira versão"
   git branch -M main
   git remote add origin https://github.com/SEU-USUARIO/futi-arena.git
   git push -u origin main
   ```
3. Atualize a página do repositório e confira se os arquivos apareceram.

> O maior arquivo é o `index.html`, com cerca de 6,4 MB. Fica bem abaixo do limite de 100 MB por arquivo do GitHub.

---

## 4. Publicar na Vercel (passo a passo)

1. Entre em **https://vercel.com** e faça login com sua conta do GitHub.
2. Clique em **Add New… → Project**.
3. Na lista de repositórios, clique em **Import** ao lado de `futi-arena`.
4. Na tela de configuração:
   - **Framework Preset:** `Other`
   - **Root Directory:** deixe a raiz (`./`)
   - **Build Command:** deixe **vazio** (desligue o botão "Override" se ele estiver ligado)
   - **Output Directory:** deixe **vazio** (a raiz é o próprio site)
   - **Install Command:** deixe **vazio**
5. Clique em **Deploy**. Em cerca de 1 minuto a Vercel mostra o link, no formato `https://futi-arena-xxxx.vercel.app`.
6. A cada `git push` na branch `main`, a Vercel publica a nova versão sozinha.

### Usar um domínio próprio (exemplo: jogo.biscoite.com.br)
1. Na Vercel, abra o projeto e vá em **Settings → Domains → Add**.
2. Digite o domínio e siga as instruções. Normalmente é criar um registro **CNAME** apontando para `cname.vercel-dns.com` no painel de DNS do domínio.
3. Depois que o DNS propagar, atualize a tag `og:image` no `index.html` com o endereço completo. Exemplo: `https://jogo.biscoite.com.br/assets/og-image.png`. Assim o WhatsApp e as redes sociais mostram a imagem ao compartilhar o link.

---

## 5. Como funciona o progresso salvo

- O progresso fica guardado **no próprio navegador do jogador** (localStorage), numa única chave versionada: `futi-arena:save:v1`.
- O que fica salvo:
  - o campeonato em andamento (fase, chaveamento e resultados);
  - o Dourado desbloqueado;
  - a mão de cartas;
  - o último boneco;
  - a arena, o nível, o som e o tutorial já visto;
  - estatísticas simples.
- O progresso é salvo no fim de cada partida, a cada mudança de fase e quando o jogador troca de aba ou fecha a página.
- Se o jogador sair no meio de uma partida da Copa, ao voltar recomeça aquela mesma partida, sem perder a fase.
- **O save é por navegador e por domínio.** Trocar de navegador, de aparelho ou de endereço começa do zero. Um mesmo jogador terá saves separados em `futi-arena-xxxx.vercel.app` e em `jogo.biscoite.com.br`.
- Em navegação anônima, ou quando o navegador bloqueia o armazenamento, o jogo funciona normalmente e mostra uma vez o aviso *"Seu progresso não será salvo neste modo de navegação"*.
- Se o save estiver corrompido, o jogo começa limpo sem travar.
- Para apagar tudo: **Personalizar → Apagar progresso**, e confirme.

---

## 6. Modo de teste (QA)

Serve para testar a Copa e o Dourado sem jogar o campeonato inteiro. **Vem desligado neste pacote.**

1. Abra o `index.html` num editor de texto e procure, no começo do código:
   ```js
   var QA_ENABLED=false;
   ```
2. Troque para `true` e salve.
3. Abra o jogo com `?qa=1` no fim do endereço. Exemplo: `http://localhost:8080/?qa=1`
4. Na tela do chaveamento aparecem os botões **"QA ▸ vencer jogo"**, **"QA ▸ perder jogo"** e **"QA ▸ liberar Dourado"**.
5. **Antes de publicar, volte para `false`.** Com `false`, o `?qa=1` não faz nada.

---

## 7. Recursos externos

Nenhum. O motor 3D (three.js r128, licença MIT) e a fonte Fredoka (licença SIL Open Font License) estão na pasta `assets/`. As demais fontes, imagens e texturas estão embutidas no `index.html`.
