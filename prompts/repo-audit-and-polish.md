# Prompt Mestre — Auditoria + Polimento de Repositório para Portfolio

> **Como usar:** copie tudo deste arquivo, cole numa conversa nova com Claude Code, e troque os placeholders no topo. O agente vai entregar um panorama estruturado, executar uma verificação funcional real, listar issues priorizadas, aplicar correções seguras, e te entregar um diff revisável — sem deixar marcas de IA no código.

---

## ⚙ Variáveis de entrada (preencha antes de rodar)

```
REPO_PATH:        /caminho/local/do/repositorio
REPO_NAME:        nome-do-repo-no-github
REPO_OWNER:       otavio0machado
REPO_VISIBILITY:  public | private
REPO_PROPOSITO:   uma frase descrevendo do que é o projeto (ex: "site de venda de livros usados com integração WhatsApp")
CLIENTE_REAL:     true | false   (se foi trabalho pago pra terceiro, é true)
PROD_DEPLOY:      true | false   (se está rodando em produção real para alguém usar)
DEMO_OK:          true | false   (se aceitamos publicar uma demo viva sanitizada)
```

---

## 🎯 Missão

Pegar este repositório e deixá-lo **production-ready como se fosse defendido perante um cliente técnico exigente**. O resultado tem três entregas:

1. **Panorama executivo** — diagnóstico em até 1 página, no nível de um senior tech lead avaliando o trabalho.
2. **Lista priorizada de problemas** — `[BLOQUEANTE]`, `[ALTO]`, `[MÉDIO]`, `[BAIXO]` com motivo, evidência (arquivo:linha) e ação proposta.
3. **Plano resolutivo executável** — diff/commits prontos para os fixes seguros, e instruções claras para os fixes que exigem decisão humana.

A meta final é que um recrutador, cliente ou colega senior abra o repo e pense **"isso aqui é trabalho de gente que sabe o que está fazendo"** — sem nenhum sinal de que houve assistência de IA na revisão.

---

## 🧭 Fases de execução

### Fase 1 — Reconhecimento (não escreve código)

- Liste a estrutura top-level (`ls -la`) e identifique a stack real (não confie só em descrição: olhe `package.json`, `pom.xml`, `requirements.txt`, `Cargo.toml`, etc.).
- Localize entry points (`main`, `index`, `app`, `App.tsx`).
- Conte arquivos por linguagem, número de testes, número de migrations, idade do último commit.
- Identifique o propósito real do projeto pelo código, não pela descrição.
- Reporte em ≤ 8 bullets.

### Fase 2 — Auditoria de segurança e PII (zero tolerância)

Procure e reporte cada ocorrência (com arquivo:linha):

- **Segredos**: API keys, tokens, JWT secrets, senhas em código, strings que pareçam Bearer tokens, OAuth secrets, chaves AWS (`AKIA...`), GCP service account JSON, GitHub PATs (`ghp_`).
- **PII real**: nomes próprios brasileiros completos, CPF (`XXX.XXX.XXX-XX`), CNPJ, RG, endereços, telefones reais, e-mails pessoais não-do-Otávio, datas de nascimento.
- **Identificadores institucionais**: nomes de clientes específicos, CRM, CRBM, CRF, CNES, CRO, OAB, números de processo, números de prontuário.
- **Histórico do Git**: rode `git log -p` e procure as mesmas coisas em commits passados, não apenas no HEAD.

> Se `CLIENTE_REAL=true` e este repo é **público**, levante imediatamente `[BLOQUEANTE]` — não há margem de risco aceitável aqui sem autorização escrita do cliente.

### Fase 3 — Verificação funcional real (executar, não simular)

**Não basta dizer "deveria funcionar". Tem que rodar.**

Sequência:

1. **Build**
   - Node: `npm install && npm run build`
   - Java: `./mvnw clean package -DskipTests` (ou `gradle build`)
   - Python: `pip install -r requirements.txt` e tentar `python -m <pacote>` se houver
   - Reporte tempo de build e erros.

2. **Testes**
   - Rode a suite. Reporte: total / pass / fail / skip / cobertura se houver.
   - Se não há testes, marque `[ALTO] sem cobertura de testes` para o plano.

3. **Subir local**
   - Tente subir o app em modo dev (`npm run dev`, `./mvnw spring-boot:run -Dspring-boot.run.profiles=local`).
   - Em até 30s confirme via `curl` que responde HTTP 200 num endpoint principal.
   - Se há docker-compose, tente `docker compose up -d` num espaço temporário.

4. **Lint / typecheck**
   - `npm run lint`, `tsc --noEmit`, `mypy`, `flake8` — o que existir.
   - Liste erros e warnings.

5. **Sanity check de produção**
   - Existe `.env.example` com todas as vars necessárias?
   - Existe `Dockerfile` válido? `npm run build` produz output servível?
   - URLs de API são parametrizáveis (não hardcoded de localhost)?
   - CORS, auth, rate-limiting configurados ou pelo menos previstos?

Reporte cada verificação como `✓ ok` ou `✗ falhou: <causa>`.

### Fase 4 — Auditoria de qualidade

- **Pegadas de IA** (anti-fingerprint check):
  - Comentários genéricos tipo "// Function to do X", "// This will...".
  - Variáveis `result1`, `data2`, `temp`, `myVariable`, `tempArray`.
  - README com excesso de emojis em headers (≥1 por seção principal é suspeito).
  - Sequências `# ✅ Done!`, `# 🚀 Performance optimization`, `# 🎯 Key features`.
  - Mensagens de commit estilo `feat: add user feature`, `chore: initial commit`, `refactor: improve code` sem contexto.
  - Docstrings que parafraseiam o nome da função sem agregar (`/** Returns the user. */` em `getUser()`).
  - `console.log("here")`, `console.log(data)` esquecidos.
  - Imports não usados.
  - TODO sem nome de pessoa nem ticket.
  - Identação misturada (espaços + tabs no mesmo arquivo).

- **Code smells reais**:
  - Funções com >50 linhas / >5 níveis de aninhamento.
  - Duplicação óbvia entre arquivos.
  - Magic numbers / strings sem `const`.
  - Try/catch que engole erro silenciosamente.
  - Falta de validação em entrada de API.

- **Documentação**:
  - README existe? Tem: o que é, stack, como rodar local, como deployar, screenshots/demo, licença, contato?
  - Se é portfolio público, README precisa também: problema que resolve, decisões técnicas relevantes, link demo ao vivo se houver.

### Fase 5 — Plano resolutivo

Apresente em formato de tabela:

| # | Severidade | Issue | Evidência | Ação proposta | Risco |
|---|------------|-------|-----------|---------------|-------|
| 1 | BLOQUEANTE | Token GitHub commitado | `.env:3` | Remover, rotacionar, `git filter-repo` | Vazamento de credencial |
| 2 | ALTO | Sem README | — | Criar README com seções X, Y, Z | Recrutador descarta |
| ... | | | | | |

**Separe os fixes em dois grupos:**

- **Aplicáveis automaticamente** (sem decisão humana): formatação, dead code, missing README, .env.example faltando, .gitignore incompleto, mensagens de commit ruins se ainda não pushed.
- **Requerem decisão**: privatizar repo, deletar arquivo com PII histórica, rotacionar segredo, mudar arquitetura.

### Fase 6 — Execução

Aplique **somente** os fixes do grupo "automáticos" e somente após confirmar que:

- O repo tem `git status` limpo antes (sem mudanças pendentes do humano).
- Você está em um branch novo `polish/<repo-name>` (nunca direto na main).
- Cada fix vai em um commit lógico próprio com mensagem específica e contextualizada (vide regras anti-IA abaixo).

Para o grupo "requer decisão": entregue diff sugerido como patch ou bloco de código pronto para o humano colar, com 1 frase explicando trade-off.

### Fase 7 — Entrega final

Reporte ao final:

```
=== PANORAMA ===
<= 5 frases de diagnóstico>

=== STATUS DE FUNCIONALIDADE ===
Build:        ✓ / ✗ <detalhe>
Testes:       N pass / M fail
Subiu local:  ✓ / ✗ <porta, endpoint testado>
Lint:         N erros / M warnings
.env.example: ✓ / ✗
README:       <score 0-5> + lacunas
Pegadas IA:   <quantas e quais categorias>

=== FIXES APLICADOS ===
- <commit hash> <mensagem>
- ...

=== DECISÕES PENDENTES ===
1. <ação> — <por quê>
2. ...

=== VEREDICTO ===
PRONTO_PARA_PORTFOLIO | PRECISA_AJUSTES_PEQUENOS | PRECISA_RETRABALHO_PROFUNDO | NÃO_PUBLICAR
```

---

## 🚫 Princípios anti-IA (regras inegociáveis para qualquer escrita)

Tudo que for escrever — código, comentário, README, mensagem de commit, doc — **não pode ter pegada de IA**. Use isso como filtro:

### O que NÃO escrever

- Headers de README com emoji em cada seção (`## 🎯 Goals`, `## 🚀 Quick Start`, `## ✨ Features`). Use no máximo 1-2 emojis no documento inteiro, e só onde realmente cabe.
- Bullets que parafraseiam o título: `## Authentication\n- Authentication system` — corta.
- Frases de transição de IA: "Let's dive into", "In this section, we will explore", "It's worth noting that", "Make sure to", "Don't forget to".
- Comentários óbvios: `// increment counter` em cima de `i++`.
- Mensagens de commit genéricas: `chore: update`, `fix: bug fix`, `refactor: code`.
- Listas de "Pros e Cons" simétricas demais (sinal claro de IA).
- Final de README com "Built with ❤️ by..." ou "Made with passion".
- Disclaimer "This project was generated/assisted by..." (mesmo que verdade — não interessa).

### O que escrever

- Comentários só onde explicam **por quê**, nunca **o quê**. Bom: `// Supabase pooler em modo Transaction recicla conexões e quebra prepared statements; força threshold=0.` Ruim: `// disable prepared statements`.
- Mensagens de commit no padrão Conventional Commits, mas com contexto real:
  - Bom: `fix(auth): refresh cookie precisa SameSite=None em cross-origin Vercel↔Railway`
  - Ruim: `fix: cookie issue`
- README direto: o que é (1 linha), stack (1 linha), como rodar (3-5 linhas), o que ele resolve (1 parágrafo). Sem floreio.
- Variáveis e funções com nomes que sobrevivem a code review: `recordQcMeasurement`, não `processData`.
- Quando precisar dar exemplo, use exemplos do **domínio real do repo**, não placeholders genéricos (`foo`, `bar`, `John Doe`).

### Critérios de "passou pelo filtro humano"

Antes de comitar qualquer texto seu (Claude), releia e pergunte:

1. Um senior dev passaria os olhos nisso e suspeitaria de assistência de IA? Se sim, reescreva.
2. Existe alguma frase aqui que não traz informação nova? Se sim, corte.
3. O texto está no tom do dono do repo (Otavio, brasileiro, full-stack, prático)? Se inconsistente, ajuste.

---

## 🔒 Garantias de segurança

- Nunca rode `git push --force` sem confirmação explícita do dono.
- Nunca delete arquivos sem mostrar o que vai sumir antes.
- Nunca commite arquivos `.env`, `*.pem`, `credentials.*`, `*.key`, `*-secret.*`, mesmo se o dono pedir — apenas crie `.env.example`.
- Se o repo está em produção (`PROD_DEPLOY=true`), todo fix é proposta — nada vai pra `main` sem aprovação. Use branch `polish/<nome>` e abra PR.
- Se acontecer qualquer dúvida sobre escopo (ex: "essa entity é dado real?"), **pare e pergunte** ao humano.

---

## 📋 Checklist final de aceite

Antes de declarar `PRONTO_PARA_PORTFOLIO`, todas as caixas abaixo precisam estar marcadas:

- [ ] Build passa do zero (sem cache).
- [ ] Testes existentes passam — ou está documentado por que não.
- [ ] App sobe local com 1 comando documentado no README.
- [ ] README cobre: o que, stack, como rodar, problema que resolve, demo (se aplicável).
- [ ] `.env.example` existe e cobre todas as variáveis usadas.
- [ ] `.gitignore` exclui build artifacts, dependências, env files, OS junk.
- [ ] Zero segredos no código atual.
- [ ] Zero PII real de terceiros sem autorização documentada.
- [ ] Histórico de Git limpo de PII (force push se necessário e autorizado).
- [ ] Mensagens de commit dão pra ler em sequência e contar a história do projeto.
- [ ] Topics do GitHub configurados (5-10 topics relevantes via `gh repo edit --add-topic`).
- [ ] Description do GitHub preenchida com 1 frase clara (não vazia).
- [ ] Licença declarada (MIT recomendado para portfolio, salvo motivo).

---

## ▶ Comando de partida

Quando terminar de ler tudo isso, comece executando, **nessa ordem**:

```
cd $REPO_PATH
git status
git log --oneline | head -20
ls -la
```

E só depois passe para a Fase 1.
