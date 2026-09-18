# Raio-X Jurídico da Escola

Página de captação de leads para uso em eventos (Geedu Connect e similares),
desenvolvida para a **Clara Machado Advocacia & Consultoria**.

Site estático puro — HTML, CSS e JavaScript sem framework e sem back-end.
Os leads capturados ficam salvos no `localStorage` do próprio dispositivo
usado para aplicar o diagnóstico (ideal: um único tablet/celular dedicado
ao estande).

## Estrutura do projeto

```
raiox-jurico-escola/
├── index.html          → estrutura da página (telas: intro, perguntas, formulário, resultado, painel da equipe)
├── css/
│   └── styles.css      → todo o estilo visual (identidade Clara Machado Advocacia)
├── js/
│   └── app.js          → toda a lógica (perguntas, pontuação, formulário, painel da equipe, CSV)
├── assets/
│   └── logo.png        → logo usada no cabeçalho e como favicon
├── vercel.json          → configuração mínima de deploy (headers de segurança)
└── README.md
```

## Antes de publicar — configurações obrigatórias

Abra `js/app.js` e edite o objeto `CONFIG` no topo do arquivo:

```js
var CONFIG = {
  firmWhatsApp: "5579900000000", // <-- troque pelo WhatsApp comercial real (formato 55DDDNUMERO, só dígitos)
  teamPin: "2026",               // <-- código de acesso do painel da equipe no estande
  storageKey: "raiox_leads_v1"
};
```

Sem isso, o botão "Falar com a equipe agora" do resultado vai abrir um
WhatsApp que não existe.

## Testar localmente

Não precisa de instalação. Duas opções:

**Opção 1 — abrir direto no navegador**
Dê duplo clique em `index.html`.

**Opção 2 — servidor local (recomendado, evita qualquer bloqueio de `file://`)**
```bash
npx serve .
# ou
python3 -m http.server 8080
```
Depois acesse `http://localhost:8080` (ou a porta indicada).

## Deploy no Vercel

**Opção A — pelo painel do Vercel (mais simples, sem terminal)**
1. Acesse [vercel.com](https://vercel.com) e crie uma conta (pode usar GitHub, Google ou e-mail).
2. Clique em **Add New → Project**.
3. Escolha **Deploy sem Git** / arraste a pasta `raiox-jurico-escola` inteira
   para a área de upload (ou importe de um repositório Git, se preferir
   subir o projeto para o GitHub antes).
4. Em "Framework Preset", deixe **Other** (é um site estático, não precisa
   de build).
5. Clique em **Deploy**. Em menos de um minuto você recebe uma URL pública
   (`algo.vercel.app`).

**Opção B — pelo terminal (Vercel CLI)**
```bash
npm install -g vercel     # instala a CLI (uma vez só)
cd raiox-jurico-escola    # entre na pasta do projeto
vercel login              # autentica com sua conta
vercel                    # gera um link de preview
vercel --prod             # publica na URL de produção
```

**Domínio próprio:** depois do primeiro deploy, em
`Project Settings → Domains`, você pode apontar um domínio ou subdomínio
próprio (ex. `raiox.claramachadoadvocacia.com.br`) para esta URL.

## Rotina de uso no evento

1. Abra a URL publicada no tablet/celular que ficará no estande.
2. Deixe a tela na tela inicial ("Iniciar diagnóstico").
3. Ao final de cada atendimento, toque em **Novo diagnóstico** para reiniciar
   para o próximo visitante — os dados do lead anterior já foram salvos.
4. Ao final do evento, toque em **Acesso da equipe** (rodapé), digite o
   código (`teamPin`), e clique em **Baixar CSV** para exportar todos os
   leads do dia. Depois disso, **Limpar todos os dados** deixa o
   dispositivo pronto para o próximo evento.

## Importante sobre os dados

Como não há back-end, os leads ficam **apenas no navegador do dispositivo
usado**. Isso significa:
- Use sempre o **mesmo aparelho** durante todo o evento, para acumular
  todos os leads num único lugar.
- Exporte o CSV **antes** de limpar os dados ou de desinstalar/atualizar o
  navegador.
- Para integrar automaticamente com uma base própria (Supabase, CRM etc.),
  é necessário adicionar uma chamada de API em `js/app.js`, na função
  `saveLead()` — ponto de extensão já isolado no código para isso.
