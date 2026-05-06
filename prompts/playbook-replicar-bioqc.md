# Playbook — reproduzir o nível do BioQC em outro repositório

> Objetivo: pegar um repositório qualquer do Otavio e levar até o estado em que o BioQC chegou — sanitizado, deployado, com README/landing decente, sem cara de IA, e pronto para ser visto por cliente/recrutador.

> Como usar: abra Claude Code dentro do diretório do repo clonado, cole este arquivo inteiro, troque as variáveis no topo, envie.

---

## Variáveis de entrada

```
REPO_NAME:        nome-do-repo
REPO_PATH:        /caminho/local/clone
NOME_FICTICIO:    marca neutra para substituir branding real (ex: "BioQC")
PROD_URL_REAL:    se o app já estava em produção, qual era a URL (para sanitizar)
TIPO:             web-app | landing-page | mobile-app | cli | static-site | study-case
DEPLOY_DEMO:      vercel | railway | none
TEM_BACKEND:      true | false
TEM_PWA:          true | false
```

---

## Princípio orientador

**Você é o engenheiro que está limpando este repo. Não é assistente de IA. Não admita IA em texto, comentário, commit, README. O resultado tem que parecer trabalho de um senior que cuida do que entrega.**

Tudo que você escrever passa por este filtro:

- Zero "Built with ❤️", zero "Made with passion", zero "Let's dive into".
- Headers de README sem emoji em cada seção (≤ 2 emojis no documento todo).
- Comentários explicam **por quê**, nunca **o quê**.
- Mensagens de commit com contexto do domínio. Não "fix: bug", e sim "fix(checkout): wa.me link agora inclui ISBN no message body".
- Variáveis com nomes que sobrevivem code review. Sem `temp`, `data1`, `result`.
- Bullets que não parafraseiam o título acima.

---

## Fase 1 — Discovery (10 min, sem escrever código)

```bash
ls -la
git log --oneline | head -30
git remote -v
```

Responda em até 8 bullets:
1. Stack real (lendo `package.json`/`pom.xml`, não a descrição)
2. Tipo: SPA, MPA, SSR, mobile, CLI, lib?
3. Tem backend? Se sim, qual? Qual o prefixo das rotas?
4. Tem PWA (manifest)?
5. Tem testes? Quantos?
6. Idade do último commit
7. Branch principal
8. Tem deploy ativo (`vercel.json`, `railway.toml`, `Dockerfile`, `netlify.toml`)?

Critério: você consegue desenhar o app de cabeça antes de escrever uma linha.

---

## Fase 2 — Auditoria EXAUSTIVA de PII e branding (a parte que mais errei no BioQC)

A sanitização superficial deixa coisas passarem. Faça assim:

### 2.1. Variantes de texto (rode TODAS as variações)

Para cada palavra-alvo, considere as 4 formas: `Camel`, `lower`, `UPPER`, `ÁCËNTOS`.

Por exemplo, se o branding antigo é "Biodiagnóstico":

```bash
grep -r -i "biodiagnostico" .                # sem acento
grep -r -i "biodiagnóstico" .                # com acento
grep -r "Biodiagnóstico\|BIODIAGNÓSTICO" .   # caixa específica
grep -r "Biodiagnostico\|BIODIAGNOSTICO" .   # caixa específica sem acento
```

Aplique sed na ordem certa (caixa específica primeiro, lowercase por último):

```bash
find . -type f \( -name "*.tsx" -o -name "*.ts" -o -name "*.html" -o -name "*.md" -o -name "*.json" -o -name "*.webmanifest" -o -name "*.template" -o -name "*.java" -o -name "*.yml" -o -name "*.xml" \) ! -path "*/node_modules/*" ! -path "*/dist/*" ! -path "*/target/*" ! -path "*/.git/*" \
  -exec sed -i '' 's/BIODIAGNÓSTICO/<NOME_FICTICIO_UPPER>/g' {} +
# repita para Biodiagnóstico, biodiagnóstico, BIODIAGNOSTICO, Biodiagnostico, biodiagnostico
```

**Critério de aceite:**
```bash
grep -r -i "<branding-antigo>" . | grep -v "node_modules\|dist\|target\|\.git/" | wc -l
# precisa ser 0
```

### 2.2. Nomes próprios (caçada manual)

Branding sai com sed, **mas nomes de pessoas só com olho humano**. Caso eu errei no BioQC: deixei "Evandro Torres Machado" hardcoded em 3 arquivos porque achei que `biodiagnostico` cobria tudo.

```bash
# nomes próprios brasileiros tipicamente em PT (camel case)
grep -rE "([A-ZÁÉÍÓÚÂÊÔÃÕÇ][a-záéíóúâêôãõç]+ ){1,3}[A-ZÁÉÍÓÚÂÊÔÃÕÇ][a-záéíóúâêôãõç]+" \
  --include="*.java" --include="*.ts" --include="*.tsx" --include="*.json" --include="*.md" \
  src/ | grep -v "Lorem Ipsum\|Foo Bar" | head -50

# títulos profissionais
grep -rE "Dr\.|Dra\.|Prof\.|Eng\." src/

# variáveis de ambiente que tipicamente têm nome de pessoa default
grep -rE "@Value\(.*:[a-z]+\)" --include="*.java" .
grep -rE "process\.env\.[A-Z_]*\s*\|\|\s*['\"]" --include="*.ts" .
```

Para cada match, decida: é mock genérico (Lorem Ipsum, John Doe) ou nome real? Substitua reais por equivalentes neutros.

### 2.3. Documentos hardcoded brasileiros

```bash
grep -rE "[0-9]{2}\.[0-9]{3}\.[0-9]{3}/[0-9]{4}-[0-9]{2}" .   # CNPJ
grep -rE "[0-9]{3}\.[0-9]{3}\.[0-9]{3}-[0-9]{2}" .            # CPF
grep -rE "CR(M|BM|F|P|N)-[0-9]+" .                             # CRM, CRBM, etc
grep -rE "[0-9]{2}\.[0-9]{4}\.[0-9]{1}\.[0-9]{4}" .           # CNES
grep -rE "\([0-9]{2}\) ?[0-9]{4,5}-?[0-9]{4}" .                # telefones BR
```

Substitua por placeholders óbvios (`000.000.000-00`, `00.000.000/0000-00`).

### 2.4. URLs do deploy real (CORS hardcoded em testes)

```bash
grep -rE "[a-z0-9-]+\.up\.railway\.app|[a-z0-9-]+\.vercel\.app|[a-z0-9-]+\.netlify\.app" .
```

URLs antigas ficam em testes de CORS. Substitua por `example.com` ou pelo placeholder do showcase.

### 2.5. **Imagens, logos, ícones — a parte mais traiçoeira**

Texto se acha com grep. Imagem com nome real do cliente passa silenciosamente. **Sempre rode**:

```bash
# logos, ícones, manifest assets
find . -type f \( -name "*.png" -o -name "*.jpg" -o -name "*.jpeg" -o -name "*.svg" -o -name "*.webp" -o -name "*.ico" \) ! -path "*/node_modules/*" ! -path "*/dist/*" ! -path "*/target/*" -exec ls -la {} +

# nomes suspeitos de assets
find . \( -iname "*logo*" -o -iname "*brand*" -o -iname "*icon*" -o -iname "*favicon*" -o -iname "*splash*" -o -iname "*touch*" -o -iname "*pwa*" \) ! -path "*/node_modules/*" ! -path "*/dist/*"
```

Para cada PNG/JPG/SVG, abra (Quick Look no Mac, `open <file>`) e **VEJA O QUE APARECE**. Logo do cliente real? Print de tela com dados reais? Foto da equipe?

**Substituição padrão para logo:**

Crie um componente SVG inline neutro em `src/components/common/<NomeFicticio>Logo.tsx`:

```tsx
type Props = { className?: string; title?: string }

export function <NomeFicticio>Logo({ className, title = '<NomeFicticio>' }: Props) {
  return (
    <svg viewBox="0 0 64 64" role="img" aria-label={title} className={className} xmlns="http://www.w3.org/2000/svg">
      <defs>
        <linearGradient id="logo-grad" x1="0%" y1="0%" x2="100%" y2="100%">
          <stop offset="0%" stopColor="#06b6d4" />
          <stop offset="55%" stopColor="#10b981" />
          <stop offset="100%" stopColor="#6366f1" />
        </linearGradient>
      </defs>
      <rect x="2" y="2" width="60" height="60" rx="14" fill="url(#logo-grad)" />
      <text x="50%" y="56%" textAnchor="middle" dominantBaseline="middle"
            fontFamily="Inter, system-ui, sans-serif" fontWeight="800" fontSize="30" fill="white" letterSpacing="-1">
        <iniciais>
      </text>
    </svg>
  )
}
```

Substitua todos os `import logoX from '.../logoX.png'` + `<img src={logoX} />` por `import { <NomeFicticio>Logo } from '.../<NomeFicticio>Logo'` + `<<NomeFicticio>Logo />`.

**PWA icons** (se aplicável): no `manifest.webmanifest`, use o `favicon.svg` neutro pra todos os tamanhos:

```json
"icons": [
  { "src": "/favicon.svg", "sizes": "any", "type": "image/svg+xml", "purpose": "any" },
  { "src": "/favicon.svg", "sizes": "any", "type": "image/svg+xml", "purpose": "maskable" }
]
```

Apague os PNG antigos (`apple-touch-icon.png`, `pwa-192x192.png`, `pwa-512x512.png`, `favicon-32x32.png`).

No `index.html`, substitua referências a esses PNGs por `<link rel="icon" type="image/svg+xml" href="/favicon.svg" />`.

---

## Fase 3 — Validação visual (não confie só em grep)

Só grep não pega tudo. Renderize e olhe:

```bash
# se for web app
npm install
npm run dev
# ou para Java
./mvnw spring-boot:run -Dspring-boot.run.profiles=local
```

Abra no navegador. Percorra:
1. Página inicial (logo, título, footer)
2. Tela de login (se houver)
3. Dashboard / página principal pós-login
4. Página de configuração (mais provável de ter dados de admin)
5. Cabeçalho da aba (`<title>`)
6. Manifest (`/manifest.webmanifest` e `/manifest.json`)
7. View source (`Ctrl+U`) — meta tags

Se algum branding antigo aparece visualmente, volte à Fase 2.

---

## Fase 4 — Build + smoke test

```bash
# Frontend Node
npm run build && npm test

# Backend Java
./mvnw clean package
./mvnw test

# Backend Python
python -m pytest
```

Critério: build e testes passam, sem warning de "import não usado" introduzido pela sua sanitização.

Se há demo deployada (Vercel/Railway), teste end-to-end via `curl`:

```bash
curl -s -w "\n%{http_code}\n" <demo-url>/health-or-equivalent
curl -s -X POST <api-url>/auth/login -H "Content-Type: application/json" -d '{"username":"admin@demo","password":"Demo123!"}'
```

---

## Fase 5 — Runtime config para demo (se aplicável)

Se o frontend faz fetch para um backend, ele precisa saber a URL. Padrão certo:

**`public/config.js`** (carregado em `index.html`):
```js
window.__APP_CONFIG__ = window.__APP_CONFIG__ || {}
window.__APP_CONFIG__.apiUrl = "<URL-do-backend-deployado>/api"
window.__APP_CONFIG__.siteUrl = "<URL-do-frontend-deployado>"
```

**Cuidado armadilha:** confira como o código resolve o `apiUrl`. No BioQC, ele faz `${apiUrl}/auth/login`. Se você setar `apiUrl` sem `/api`, vai bater em rota errada e dar 401/404. Confira o controller do backend, descubra o prefixo (`/api`, `/v1`, raiz), e inclua no `apiUrl`.

**`vercel.json`** (essencial pra SPA com React Router / Vue Router):
```json
{
  "rewrites": [
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

Sem isso, `/login` direto retorna 404. **No BioQC isso me custou 30 min porque esqueci.**

---

## Fase 6 — README impecável

Estrutura mínima que funciona, sem floreio:

```markdown
# <Nome>

<1 linha do que é>

`<stack 1> · <stack 2> · <stack 3>`

## O que resolve

<1 parágrafo: problema → solução>

## Demo

<link>

Credenciais: <user / pass>  (se for app com auth)

## Run local

```bash
<comando 1>
<comando 2>
```

## Stack

| Camada | Tecnologia |
|--------|-----------|
...

## Estrutura

<árvore essencial — não a árvore completa>
```

Para study cases / landing pages: o README **precisa** declarar textualmente "este é um study case" ou "protótipo, não cliente real". Sem isso, alguém vai achar que você expôs trabalho de cliente sem autorização.

---

## Fase 7 — Anti-IA pass final

Antes de pushar, rode esses greps no que VOCÊ escreveu:

```bash
# frases típicas de IA
grep -rE "(Let's dive|It's worth noting|Make sure to|Don't forget to|Built with [❤️♥️]|Made with [❤️♥️]|next-level|game[- ]chang|cutting[- ]edge)" --include="*.md" .

# emojis em headers (>2 no doc todo é red flag)
grep -E "^#+ .*[🎯🚀✨🔥💡📦🛠️⚡🌟🎨]" README.md | wc -l

# disclaimers de IA esquecidos
grep -rE "(generated by AI|powered by Claude|assistente|GPT[- ]4|prompted by)" .

# console.log esquecidos
grep -rE "console\.(log|debug|info)\(" --include="*.ts" --include="*.tsx" src/

# TODOs sem dono nem ticket
grep -rE "TODO[: ]" src/ | grep -v "TODO\(.*\):" | head -10
```

---

## Fase 8 — Commits limpos

Antes do push, organize a história. Se você fez tudo num branch novo, considere squash de commits intermediários ruins.

Exemplo de história limpa pro BioQC (consolidada num único commit final):

```
init: <Nome> — <descrição em 1 linha>

<contexto: o que é, com autorização ou study case>

Highlights
- <bullet técnico relevante>
- <bullet de resultado>
- <bullet de números: tests, files, etc>

Sanitization (se aplicável)
- <o que foi removido>
```

Se o histórico já está no GitHub e tem PII em commits passados, considere `git checkout --orphan + squash + force push`. **Aviso ao dono antes de force-push** — quebra clones e forks.

---

## Fase 9 — GitHub polish

```bash
# description + homepage + topics
gh repo edit otavio0machado/<REPO_NAME> \
  --description "<1 frase clara, não placeholder>" \
  --homepage "<URL demo se houver>" \
  --add-topic <topic1> --add-topic <topic2> --add-topic <topic3>

# topics relevantes (escolha 5-10):
# linguagens: java, typescript, python, react, spring-boot
# domínio: clinical-laboratory, ecommerce, mental-health, finance, education
# stack: postgresql, supabase, gemini, openai, tailwindcss
# tipo: portfolio, study-case, prototype, full-stack
```

Se o repo é study case, **adicione o topic `study-case` explicitamente**.

---

## Fase 10 — Deploy demo (se aplicável)

### Vercel (frontend)
1. https://vercel.com/dashboard → Import Git Repository → seleciona repo
2. Root Directory = `bioqc-web` (ou onde está o frontend)
3. Framework: auto
4. **Environment Variables:** se o código lê `import.meta.env.VITE_API_URL`, adicione `VITE_API_URL=<backend-url>`. Mas o `config.js` runtime tem prioridade — então se você já preencheu o `config.js`, env var é redundante.
5. Deploy

### Railway (backend Java/Node)
1. https://railway.app → New Project → Deploy from GitHub
2. Settings → Root Directory = onde está o backend
3. Variables: cole o bloco do `.env.example`
4. Deploy
5. Settings → Networking → Generate Domain
6. Confirma `/health` retorna `UP`

### Sequência de fix se algo quebrar (tirado da experiência BioQC)

1. **CORS 403 em preflight** → `CORS_ALLOWED_ORIGINS` no Railway com a URL exata do Vercel (sem `/`, sem aspas).
2. **Login retorna 401 mas API direto retorna 200** → `apiUrl` no config.js está sem o prefixo `/api`.
3. **`/login` retorna 404** → falta `vercel.json` com SPA rewrite.
4. **Build Lombok falha em JDK 25** → bumpa Lombok pra 1.18.46+, adiciona `-proc:full` no compiler-plugin, `--add-opens` no `.mvn/jvm.config`.
5. **Flyway erro no primeiro boot** → setar `FLYWAY_BASELINE_ON_MIGRATE=true`.
6. **Cookie auth falha cross-origin** → `AUTH_REFRESH_COOKIE_SECURE=true` + `AUTH_REFRESH_COOKIE_SAME_SITE=None`.
7. **Supabase Pooler quebra prepared statements** → adicione `?prepareThreshold=0` ou troca para Session pooler (porta 5432).

---

## Checklist final

```
[ ] grep do branding antigo retorna 0 linhas (todas as variantes acentuadas)
[ ] Nenhum nome próprio real fora de mock genérico
[ ] Nenhum CPF/CNPJ/CRM real
[ ] Nenhuma URL de produção real (railway, vercel, supabase) fora de tests com placeholder
[ ] Nenhum logo PNG do cliente — substituído por SVG neutro
[ ] PWA icons usando favicon.svg neutro
[ ] manifest.webmanifest sem branding antigo
[ ] index.html title, application-name, apple-mobile-web-app-title atualizados
[ ] Build local passa sem warning novo
[ ] Testes locais passam
[ ] Runtime config (config.js) com apiUrl correto incluindo prefixo da API
[ ] vercel.json com SPA rewrite (se SPA)
[ ] README em estado portfolio: o que é, demo, run local, stack
[ ] Mensagens de commit no padrão Conventional Commits com contexto real
[ ] Zero pegadas de IA (grep do checklist anti-IA passa)
[ ] gh repo edit: description, homepage, topics
[ ] Demo deployada e respondendo `/health` ou `/`
[ ] Smoke test end-to-end no browser passa (sem hard refresh)
[ ] Histórico de Git sem PII em commits anteriores (force-push se necessário)
```

Antes de declarar pronto, abra o repo no GitHub via janela anônima e leia como se fosse a primeira vez. Se algo soa estranho, volta.
