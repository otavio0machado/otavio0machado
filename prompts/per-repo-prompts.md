# Prompts individuais — um por repositório

Cada bloco abaixo é um prompt completo. Para usar:
1. `gh repo clone otavio0machado/<nome-do-repo>` — clone o repo localmente.
2. Abra Claude Code dentro do diretório clonado (`cd ~/clones/<nome>` e `claude`).
3. Copie o prompt do repo correspondente, cole, envie.

---

## 1. `ancora`

```
Você está dentro do diretório do repositório `ancora` — um sistema pessoal de regulação de vida baseado em DBT (Dialectical Behavior Therapy) e ACT (Acceptance and Commitment Therapy), construído pelo Otávio em TypeScript. Repo público, sem clientes envolvidos, sem deploy em produção. Aceita publicar demo sanitizada se ajudar o portfolio.

Sua missão é entregar este repo em estado "production-ready de portfolio" — um senior dev olhando deveria pensar "ok, isso aqui é trabalho sólido". Faça o seguinte, nessa ordem:

1. Reconhecimento: liste estrutura, identifique stack real (lendo package.json, não a descrição), conte arquivos, testes, idade dos commits. Reporte em até 8 bullets.

2. Auditoria de PII e segredos: procure tokens, API keys, e-mails reais, nomes completos, CPF, CRP de psicólogos, dados clínicos pessoais. Inclua busca no histórico do Git (git log -p). Reporte arquivo:linha de cada ocorrência. Como é app de saúde mental pessoal, dados sensíveis aqui são especialmente críticos.

3. Verificação funcional real: rode `npm install && npm run build`, depois `npm test` se houver, depois `npm run dev` e teste com curl que sobe. Cada etapa: ✓ ok ou ✗ <causa>.

4. Auditoria de qualidade: pegadas de IA (comentários óbvios, variáveis tipo result1/temp/data, README com emoji em cada header, commits "chore: initial commit"), code smells reais (funções gigantes, duplicação, magic numbers, try/catch que engole erro).

5. Plano resolutivo: tabela com [BLOQUEANTE/ALTO/MÉDIO/BAIXO] | issue | arquivo:linha | ação proposta.

6. Aplique fixes seguros (formatação, dead code, README ausente, .env.example faltando, .gitignore incompleto) num branch novo `polish/ancora`. Um commit lógico por fix, mensagens contextualizadas (não "chore: update", coisa do tipo "docs: README com setup local + screenshot do dashboard de regulação").

7. Para fixes que exigem decisão (privatizar, deletar histórico, mudar arquitetura), entregue diff sugerido + 1 frase de trade-off. Não execute sem o humano confirmar.

REGRAS INEGOCIÁVEIS para qualquer texto que você escrever (código, comentário, README, commit):
- Zero emojis em headers de README. No máximo 1-2 no documento todo, e só se realmente caber.
- Comentários só explicam por quê, nunca o quê. Bom: "Supabase pooler em modo Transaction quebra prepared statements; força threshold=0". Ruim: "// initialize variable".
- Mensagens de commit no padrão Conventional Commits com contexto real do domínio. Ruim: "fix: bug". Bom: "fix(emocoes): pico de ansiedade não estava sendo persistido se categoria fosse vazia".
- Nunca use frases tipo "Let's dive into", "It's worth noting", "Make sure to", "Built with ❤️", "Made with passion".
- Nunca admita assistência de IA. O dono do repo é o Otávio, ponto.
- Nada de "Pros e Cons" simétrico demais.
- Variáveis com nomes que sobrevivem code review: nunca temp, data1, result.

Entregue ao final, neste formato exato:

=== PANORAMA ===
<= 5 frases>

=== STATUS DE FUNCIONALIDADE ===
Build: ✓/✗ <detalhe>
Testes: N pass / M fail
Subiu local: ✓/✗ <porta, endpoint testado>
Lint: N erros / M warnings
.env.example: ✓/✗
README: <score 0-5> + lacunas
Pegadas IA: <quantas e quais>

=== FIXES APLICADOS ===
- <hash> <mensagem>
- ...

=== DECISÕES PENDENTES ===
1. <ação> — <por quê>

=== VEREDICTO ===
PRONTO_PARA_PORTFOLIO | PRECISA_AJUSTES_PEQUENOS | PRECISA_RETRABALHO_PROFUNDO | NÃO_PUBLICAR
```

---

## 2. `app-livros`

```
Você está dentro do diretório do repositório `app-livros` — site para vender livros e apostilas usados com integração de IA (Gemini) e checkout via WhatsApp, em TypeScript. Repo público, sem clientes externos, sem produção ativa. Aceita demo sanitizada.

Missão: deixar este repo em estado portfolio. É um dos 6 que serão pinnados, então o impacto é alto.

Execute nessa ordem:

1. Reconhecimento: estrutura, stack (lendo package.json), entry points, número de páginas/componentes, dependências relevantes (Gemini SDK, WhatsApp link). 8 bullets.

2. Auditoria PII + segredos: API keys da Gemini hardcoded, tokens, números de WhatsApp reais, e-mails de fornecedores, nomes completos de autores reais, ISBNs específicos (ISBN é dado público mas associado a estoque pode revelar livros pessoais). Inclua git log -p.

3. Verificação funcional: npm install, npm run build, npm run dev, abra a homepage, teste fluxo: listar livro → clicar comprar → ver redirecionamento WhatsApp. Reporte cada passo.

4. Verificação específica do domínio:
- A integração com Gemini está com fallback se API key faltar?
- O link de WhatsApp usa wa.me/... com mensagem pré-preenchida funcional?
- Imagens de capa têm fallback se URL quebrar?
- Há rate limiting na chamada Gemini ou pode estourar quota?

5. Auditoria de qualidade: pegadas de IA, code smells, README. README precisa: o que é, screenshot, como rodar local, como usar Gemini (env var GEMINI_API_KEY, link pra obter), como customizar mensagem do WhatsApp.

6. Aplique fixes seguros num branch `polish/app-livros`. Commits específicos do domínio.

7. Decisões pendentes com diff sugerido.

REGRAS INEGOCIÁVEIS:
- Zero emojis em headers de README. Use no máximo 1-2 em todo o doc.
- Comentários explicam por quê, não o quê.
- Commits com contexto: "feat(checkout): mensagem do WhatsApp inclui ISBN + valor + nome do comprador" não "feat: checkout".
- Sem "Built with ❤️", sem "Made with passion", sem "Let's dive into".
- Variáveis: bookListing, não data; cartItem, não temp.
- Nunca admita assistência de IA.

Entregue no formato exato:

=== PANORAMA ===
=== STATUS DE FUNCIONALIDADE ===
Build / Testes / Subiu local / Lint / .env.example / README score / Pegadas IA
=== FIXES APLICADOS ===
=== DECISÕES PENDENTES ===
=== VEREDICTO ===
```

---

## 3. `flashcards_generator`

```
Você está dentro do diretório do repositório `flashcards_generator` — gerador de flashcards em TypeScript. É o único repo público do Otávio com tração externa (1 star de outra pessoa), 8.5MB, então tem código real. Vai ser pinnado.

Missão: portfolio-ready. Foco extra em fazer rodar, porque ter star sem README significa que alguém topou tentar e provavelmente travou.

Execute:

1. Reconhecimento: olhe package.json, identifique se é CLI / web app / extensão / API. Identifique se gera flashcards via IA (qual modelo?) ou só estrutura cards. 8 bullets.

2. PII + segredos: API keys de OpenAI/Anthropic/Gemini, e-mails, prompts hardcoded com dados pessoais, decks de teste com nomes reais.

3. Verificação funcional: npm install, build, run. Tente gerar um flashcard de exemplo (input simples, output cards). Documente o que funcionou.

4. Verificação específica:
- Onboarding pra primeiro uso é claro? (a 1 star sugere que alguém quis usar — torne fácil pra eles).
- Tem exemplo de input/output no README?
- Output é exportável (Anki, Quizlet, JSON)?
- Tratamento de erro se a IA retornar formato inesperado?

5. Qualidade + IA fingerprints + commits.

6. Fixes seguros em branch `polish/flashcards-generator`. README precisa ser bom — esse repo já tem audiência mínima.

7. Decisões pendentes.

REGRAS INEGOCIÁVEIS: idem aos outros (zero emoji em headers, commits com contexto, sem "Built with ❤️", variáveis nomeadas, sem admitir IA).

Atenção especial: README atual provavelmente é vazio ou ruim — gaste tempo nele. Inclua: o que faz (1 frase), input esperado, output esperado, como rodar, como configurar a IA backend, exemplo concreto. Adicione screenshot ou GIF se conseguir gerar.

Formato de entrega: idêntico aos anteriores (PANORAMA / STATUS / FIXES APLICADOS / DECISÕES PENDENTES / VEREDICTO).
```

---

## 4. `portfolio`

```
Você está dentro do diretório do repositório `portfolio` — site portfolio pessoal do Otávio, descrito como "Cosmic portfolio — immersive experience with NASA videos, liquid glass & starfield". HTML/CSS/JS, 12.6MB. Será pinnado.

Atenção: este repo tem 12.6MB — parte significativa provavelmente são vídeos NASA. Verifique:
- Vídeos estão direto no repo (ruim) ou referenciados via URL (bom)?
- Se estão no repo, considere mover pra CDN ou GitHub LFS.

Missão: portfolio-ready, com extra atenção em performance (porque é portfolio, e portfolio lento é antipropaganda) e licenciamento (vídeos NASA são domínio público mas precisa atribuir).

Execute:

1. Reconhecimento: estrutura, peso real dos assets, build pipeline (se houver), bibliotecas usadas (Three.js? GSAP? vanilla?). 8 bullets.

2. PII + segredos: e-mails, telefones, nomes de empresas reais sem autorização (se referencia clientes anteriores), API keys (Google Analytics, Mapbox, etc.).

3. Funcionalidade: abra index.html no navegador via servidor estático (`npx serve .` ou similar). Confirme:
- Carrega em < 5s em rede normal?
- Funciona em mobile (responsivo)?
- Vídeos não bloqueiam render inicial?
- Não há JS error no console?

4. Verificação específica:
- Atribuição NASA correta (rodapé, texto alt, doc)?
- Imagens com lazy loading?
- Lighthouse score (rode lighthouse e reporte performance / accessibility / SEO).
- Meta tags Open Graph corretas (compartilhamento social)?

5. Qualidade + fingerprints. Para portfolio, README pode ser curto, mas precisa ter: link demo, stack visual, decisões de design (por que cosmic, por que NASA), screenshot, contato.

6. Fixes em branch `polish/portfolio`. Otimização de assets entra aqui (compressão de imagem, conversão de vídeo para WebM se PSDable, etc.).

7. Decisões pendentes.

REGRAS INEGOCIÁVEIS:
- Como é PORTFOLIO, capricho extra. Mas sem cair em IA-style.
- Zero "Made with passion".
- Commits específicos: "perf(home): lazy-load do vídeo de fundo evita 6s de bloqueio inicial".
- Nada de bullets parafraseando título.

Formato de entrega: PANORAMA / STATUS / FIXES / DECISÕES / VEREDICTO. No STATUS, adicione linha "Lighthouse: P/A/SEO".
```

---

## 5. `ancora-mobile`

```
Você está dentro do diretório do repositório `ancora-mobile` — versão mobile do `ancora` (sistema de regulação de vida DBT/ACT), TypeScript, 761KB. Será pinnado junto com o ancora web.

Missão: portfolio-ready, com atenção a coerência com o `ancora` web (devem parecer ecossistema, não dois projetos soltos).

Execute:

1. Reconhecimento: identifique a stack (React Native? Expo? Capacitor? PWA?), e como conecta com o ancora web (compartilha API? tipos? backend?). 8 bullets.

2. PII + segredos: API keys (Firebase, Sentry, etc.), bundle IDs reais com nome de pessoa, push notification keys, dados clínicos pessoais.

3. Funcionalidade: rode no simulador/expo. Documente:
- Comando exato pra rodar (`expo start`, `npm run ios`, etc.).
- Screen principal carrega?
- Navegação básica funciona?
- Conecta com backend (se houver)?

4. Verificação específica mobile:
- Permissões declaradas (notifications, storage)?
- Splash screen + ícone configurados?
- Build configs (eas.json, app.json) presentes?
- Tem instruções claras pra rodar em iOS / Android?

5. Coerência com `ancora` web:
- Branding/cores consistentes?
- README cross-referencia o web?
- API/backend compartilhada (se sim, documente como; se não, justifique)?

6. Fixes em branch `polish/ancora-mobile`.

7. Decisões pendentes.

REGRAS INEGOCIÁVEIS: zero emoji em headers, commits com contexto, nunca admitir IA, sem "Built with ❤️".

Atenção: app de saúde mental — qualquer dado de teste hardcoded com humor/emoção real precisa ser substituído por mock genérico.

Formato: PANORAMA / STATUS (adiciona "Roda iOS sim?" / "Roda Android sim?") / FIXES / DECISÕES / VEREDICTO.
```

---

## 6. `AnestData`

```
Você está dentro do diretório do repositório `AnestData` — projeto relacionado a anestesiologia, TypeScript, 96KB. Não foi pinnado mas vai ficar público pra dar densidade ao perfil em domínio médico.

Cuidado especial: anestesiologia é domínio sensível. Qualquer dado real de paciente, médico, procedimento ou hospital aqui é problema sério.

Missão: garantir que pode ficar público com segurança e que tem README mínimo decente.

Execute:

1. Reconhecimento: identifique o que esse projeto faz exatamente (calculadora de doses? formulário de avaliação pré-anestésica? tracker de procedimentos? dashboard educativo?). Olhe o código, não só o nome. 8 bullets.

2. PII + dados clínicos (CRÍTICO): nomes completos de pacientes, prontuários, CID, medicação prescrita real, hospital específico, médicos com CRM, datas de procedimentos. Procure também em CSV / JSON / SQL / fixtures / mocks. Inclua git log.

3. Funcionalidade: build, run, abre, verifica que carrega.

4. Avaliação de risco regulatório:
- Se o app calcula dose, há disclaimer claro de "não substitui julgamento médico"?
- Se mostra dados, são fictícios e marcados como tal?
- Há alguma referência implícita a software médico regulado pela ANVISA (RDC 657/2022)?

5. Qualidade + README. README deve incluir: propósito acadêmico/educativo (ou comercial), disclaimer médico, fonte dos dados de exemplo (se sintéticos, dizer; se reais, problema), licença.

6. Fixes em branch `polish/anestdata`.

7. Decisões pendentes — em caso de qualquer dúvida sobre dados reais, recomende privatizar até confirmar com humano.

REGRAS INEGOCIÁVEIS: igual aos outros + atenção redobrada com PII médica.

Formato de entrega: padrão. No PANORAMA inclua avaliação clara: "dados visíveis são sintéticos? sim/não/parcial".
```

---

## 7. `explore-rancho-queimado`

```
Você está dentro do diretório do repositório `explore-rancho-queimado` — projeto sobre Rancho Queimado (cidade em Santa Catarina, conhecida por hortênsias e turismo serrano), TypeScript, 221KB.

Missão: portfolio-ready se valer a pena público, ou recomendar privar/arquivar se for muito raso.

Execute:

1. Reconhecimento: o que é exatamente? Site turístico? Mapa? Guia? Trabalho universitário? Olhe o código. 8 bullets.

2. PII: e-mails de pousadas/restaurantes reais, números de telefone, endereços específicos. Imagens com pessoas identificáveis.

3. Funcionalidade: build, run, abre, navega.

4. Avaliação de valor pro portfolio:
- O projeto demonstra alguma habilidade técnica não-trivial (mapas interativos, scraping, geolocalização, i18n)?
- Ou é HTML básico com pouco código?
- Vale a pena polir e manter público, ou arquivar?

5. Qualidade + README + fingerprints.

6. Se vale a pena: fixes em `polish/explore-rancho-queimado`. Se não vale: ao final do relatório, recomende explicitamente "arquivar via gh repo archive otavio0machado/explore-rancho-queimado".

7. Decisões pendentes.

REGRAS INEGOCIÁVEIS: padrão.

Formato: padrão. No VEREDICTO, se aplicar, use "ARQUIVAR" como opção.
```

---

## 8. `HPC`

```
Você está dentro do diretório do repositório `HPC` — provavelmente trabalho acadêmico de High-Performance Computing, TypeScript, 1.8MB.

Suspeita inicial: TS pra HPC é incomum (HPC normalmente é C/C++/Fortran/CUDA/Python). Pode ser frontend que visualiza dados de HPC, ou disciplina que usou TS por outra razão. Investigue.

Missão: avaliar se vale ficar público; se sim, polir.

Execute:

1. Reconhecimento aprofundado: o que é HPC aqui? Visualização? Simulação numérica em browser? Trabalho prático que documenta benchmarks? Olhe estrutura, package.json, READMEs internos. 8 bullets.

2. PII: nome de professor, disciplina, instituição, colegas de grupo. Dados de benchmark de máquinas pessoais (CPU model, etc.) — não é PII grave mas pode ser identificador.

3. Funcionalidade: build, run, abre.

4. Avaliação:
- Se é projeto acadêmico bem documentado, com benchmark ou análise técnica relevante, vale manter público.
- Se é trabalho de aula raso, recomende arquivar.

5. Qualidade + README. Se mantém público, README precisa: contexto acadêmico (disciplina, semestre), problema atacado, abordagem técnica, resultados (gráficos/números), limitações reconhecidas.

6. Fixes em branch `polish/hpc`.

7. Decisões pendentes.

REGRAS INEGOCIÁVEIS: padrão. Atenção especial em README: trabalhos acadêmicos publicados com README ruim parecem coisa de aluno; com README bom, parecem pesquisa séria.

Formato: padrão. VEREDICTO pode ser "ARQUIVAR" se não agregar.
```

---

## 9. `pucrs`

```
Você está dentro do diretório do repositório `pucrs` — provavelmente projetos da graduação em PUCRS, TypeScript, 631KB.

Risco principal: monorepo de coisas acadêmicas geralmente é monte de código rascunho que polui o portfolio.

Missão: avaliar honestamente se vale público. Provavelmente recomendação será arquivar ou desmembrar em projetos individuais.

Execute:

1. Reconhecimento: estrutura, quantos sub-projetos, qual disciplina/semestre. Identifique se é monorepo ou projeto único. 8 bullets.

2. PII: nome de professor, alunos do grupo, e-mail institucional, dados de exercício (se for resolução de prova exposta, pode ter problemas com a instituição).

3. Funcionalidade: tente buildar e rodar. Se falhar em vários pontos por ser monorepo, documente.

4. Avaliação:
- Se é monorepo: identifique 1-2 projetos que se destacam e podem virar repos próprios polidos; recomende arquivar o monorepo.
- Se é projeto único e não trivial: avalie polir.
- Se é trabalho intro de aula: arquivar.

5. Qualidade + README.

6. Fixes seguros se aplicável; senão, recomendação clara de arquivamento ou desmembramento.

7. Decisões pendentes.

REGRAS INEGOCIÁVEIS: padrão. NÃO faça desmembramento de monorepo automático — só recomende.

Formato: padrão. VEREDICTO pode ser "ARQUIVAR" ou "DESMEMBRAR".
```

---

## 10. `otavio.dev`

```
Você está dentro do diretório do repositório `otavio.dev` — provavelmente site pessoal do Otávio, TypeScript, 16KB. É bem pequeno (16KB), pode ser stub vazio ou redirect.

Missão: avaliar se é projeto real e portfolio-ready, ou se é só placeholder.

Execute:

1. Reconhecimento: 16KB é pouco. Olhe o que tem dentro. É um site Next.js inicial? Um redirect via vercel.json? Um landing page mínimo? 8 bullets.

2. PII: e-mail pessoal, números, endereço.

3. Funcionalidade: build, run, abre. Se for o site pessoal real do Otávio, ele precisa ser muito polido — é a primeira impressão pra recrutador.

4. Avaliação:
- Se é site pessoal "to be done": recomende deletar ou implementar de verdade.
- Se é redirect simples: ok manter, mas com README explicando.
- Se tem conteúdo real: polir intensamente — site pessoal é vitrine.

5. Qualidade + README + fingerprints.

6. Fixes em branch `polish/otavio-dev` se mantiver.

7. Decisões pendentes.

REGRAS INEGOCIÁVEIS: padrão. Site pessoal pede capricho extra mas zero floreio IA.

Formato: padrão. Adicione linha clara "Este repo justifica existir como público? sim/não/precisa-de-trabalho".
```

---

## ▶ Sequência recomendada

Execute na ordem 1 → 10. Os primeiros 5 são os pinnados, então valor por hora é maior. Os 5 últimos são triagem (pode acabar em "arquivar" para alguns).

Após cada repo, faça push do branch `polish/<nome>` e abra um PR no próprio fork (se quiser revisar antes do merge) ou faça merge direto se confiar no diff.
