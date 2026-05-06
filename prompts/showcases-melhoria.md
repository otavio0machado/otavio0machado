# Prompt — Melhorar showcases/study-cases (de "IA-óbvio" para "produto plausível")

> **Como usar:** abra Claude Code dentro da pasta `Showcases/` e cole este prompt inteiro. Liste os repos abaixo na ordem que quer que sejam processados. O agente vai trabalhar **um por vez**, reportar e seguir para o próximo.

---

## Contexto

Você está dentro de `~/Showcases/`. Cada subpasta é um study case / landing page que o Otavio criou (em parte ou no todo com auxílio de IA) e agora precisa ser elevado a um padrão "parece produto real, feito por humano com cuidado". O destino final é portfolio público para captação de freelance.

**Não é sanitização de produção real.** É o oposto: tirar a cara de IA e dar **identidade comercial plausível** — copy específico, layout com personalidade, trust signals que parecem verídicos (mesmo sendo fictícios).

---

## Repositórios a processar (preencha)

Substitua a lista abaixo pelos nomes das pastas em `Showcases/` na ordem que quer trabalhar. Comece pelos que mais te incomodam ou que têm mais valor pro portfolio.

```
1. <pasta-1>     Nicho: <ex: lavagem automotiva>           Persona: <ex: dono de carro premium em SP>
2. <pasta-2>     Nicho: ...                                Persona: ...
3. <pasta-3>     Nicho: ...                                Persona: ...
```

Antes de começar cada repo, confirme comigo o nicho e a persona. Se eu não preencher, **pare e pergunte** — sem essa âncora, o agente cai no genérico-IA.

---

## Princípio orientador (não negociável)

Você é o engenheiro que está revisando este study case antes de mostrar a um cliente exigente. **Não é assistente de IA. Não admite IA. Não usa frases que delatam IA. O resultado tem que parecer trabalho de gente que pensou no negócio antes de codar.**

Tudo que escrever passa por estes filtros:

### O que NUNCA fazer

- Hero copy genérico: "Transforme seu negócio com X", "Soluções inovadoras", "Eleve ao próximo nível", "Excelência em Y"
- Bullets simétricos demais: "Simples · Rápido · Seguro" / "Fácil · Eficiente · Moderno"
- Stats redondos suspeitos: "+1000 clientes", "98% satisfação", "10 anos no mercado" sem fonte
- Depoimentos com nome plástico: "João da Silva, CEO da Empresa X"
- Estrutura padrão de IA: hero → 3 features → about → 3 testimonials → pricing → CTA → footer
- Ícones aleatórios em cada feature
- Headers de README com emoji em cada seção
- "Built with ❤️", "Made with passion", "Let's dive into"
- Mensagens de commit "feat: add hero section"
- Bullets que parafraseiam o título da seção acima
- Lorem ipsum esquecido
- Classes Tailwind ordenadas alfabeticamente em sequência longa (sinal de gerador automático)

### O que SEMPRE fazer

- Copy específico do nicho. Pesquisa rápida em sites reais do segmento, absorva o jargão, use.
- Quebra de simetria: número ímpar de features, ordem inesperada de seções, micro-detalhes só de 1 lado.
- Trust signals plausíveis: nome de bairro real, CNPJ fictício mas no formato certo, telefone com DDD coerente, endereço que existe (rua de bairro popular do nicho).
- Microcopy que tem voz: botão diz "Ver agenda da semana" não "Saiba mais".
- Microcopy de empty state, error, loading com personalidade do produto.
- Mensagens de commit no padrão Conventional Commits **com contexto do domínio**: `content(hero): troca copy genérica por linguagem do mercado de detalhamento automotivo`.
- Comentários só onde explicam **por quê** (decisão de design, restrição de plataforma) — nunca **o quê**.
- Variáveis com nomes que sobrevivem code review: `bookListing`, `appointmentSlot`, não `data`/`item`/`temp`.
- README declarando textualmente que é study case (vem na Fase 6).

---

## Modo de operação: 1 repo por vez, sequencial

Para cada repo na lista, execute as fases abaixo. Só passe para o próximo depois de fechar o anterior com **VEREDICTO** explícito.

---

## Fase 1 — Diagnóstico "cara de IA" (5 min)

Liste os marcadores presentes no repo, em formato de checklist, com arquivo:linha:

```
[ ] hero copy genérico: <quote do hero atual>
[ ] features simétricas: <X features de 1 frase parafraseada>
[ ] testimonials genéricos: <quote>
[ ] stats sem fonte: <quote>
[ ] estrutura padrão IA: hero→3feat→about→3test→pricing→CTA→footer? Y/N
[ ] ícones decorativos sem propósito: <quantos>
[ ] README vazio ou com bootstrap default: Y/N
[ ] commits genéricos ("feat: hero", "chore: initial"): <quantos>
[ ] lorem ipsum residual: <onde>
[ ] classes Tailwind suspeitas em sequência alfabética longa: <linha>
[ ] frases delatoras ("Let's dive", "Built with ❤️"): <onde>
```

Esse diagnóstico vai virar a lista de coisas a corrigir.

---

## Fase 2 — Definir identidade do produto fictício (10 min)

Antes de tocar em código, defina (e me apresente):

```
NOME DO NEGÓCIO:        <pode ser o do repo ou um melhor>
TAGLINE:                <1 frase concreta, não "soluções inovadoras">
PERSONA PRIMÁRIA:       <quem usa, idade, contexto, dor real>
ZONA GEOGRÁFICA:        <cidade/bairro fictício mas plausível>
DIFERENCIAL VS GENÉRICO: <1-2 coisas que o tornam diferente do site default do nicho>
TOM DE VOZ:             <ex: direto e técnico / próximo e brincalhão / formal e tradicional>
PALETA REAL:            <2-3 cores primárias coerentes com o nicho — não "cores que IA gosta">
```

Se você não consegue preencher esses 7 campos com convicção, o site não vai parecer real. Pesquise sites reais do nicho antes de continuar.

**Para study cases comuns do Otavio:**

- **Lavagem automotiva (cleancar):** referências reais — Detail Garage, Speed Wash, Mr. Wash. Tom: direto, foco em conveniência, comparação de preço, fotos de "antes/depois".
- **Investimentos / personal financeiro (felipe.inv):** referências — XP Investimentos, Empiricus, Nord. Tom: autoridade, números, transparência sobre track record + disclaimer regulatório.
- **Profissional autônomo (helena-martins):** referências — sites de psicólogos, advogados, terapeutas. Tom: humano, autoridade calma, foco em "como funciona uma sessão".
- **Produto fictício de IA (fluxo.ai):** referências — Zapier, Make, n8n. Tom: técnico mas acessível, exemplos concretos de automação, screenshots de fluxos reais.
- **Bootcamp portfolio (semana-fullstack-portifolio):** referências — sites pessoais de devs juniores no LinkedIn. Tom: aprendiz mas competente, projetos reais, jornada documentada.
- **Turismo (explore-rancho-queimado):** referências — Visit California, Visit Brasil, sites de pousadas reais. Tom: convite, sensorial, calendário de eventos.

---

## Fase 3 — Reescrever copy (a parte mais importante)

Para cada bloco de texto visível no site, substitua. Prioridade:

### Hero
- Antes: "Transforme seu negócio com X"
- Depois: copy aterrado no benefício específico. Ex (lavagem): "Lavagem detalhada na sua garagem. Atendemos Vila Madalena e Pinheiros, sábado e domingo."

### Features (corte excesso, ganhe profundidade)
- Antes: 6 features de 1 frase cada, todas começando com verbo.
- Depois: 3 features que importam, cada uma com 2-3 linhas de explicação concreta + número/exemplo.

### Trust signals
- Substitua testimonials genéricos por 1-2 com nome de pessoa de bairro real ("Roberta, Vila Madalena"), idade ("38 anos"), e quote específico ("Marquei pela manhã, no fim da tarde o carro estava lavado e enxuto na garagem").
- Stats: troque "+1000 clientes" por "243 atendimentos no último semestre" ou apague.

### CTA
- Antes: "Comece agora", "Saiba mais"
- Depois: ação concreta. "Ver agenda do sábado", "Pegar orçamento por WhatsApp", "Agendar avaliação grátis na primeira sessão".

### Footer
- Inclua: CNPJ fictício no formato correto (`XX.XXX.XXX/0001-XX`), endereço plausível, telefone com DDD coerente com a zona, e-mail que combina com o domínio fictício.
- Disclaimer "study case" curto no rodapé linkando pro README.

---

## Fase 4 — Quebrar a simetria visual

Se o layout segue **hero → 3 features → about → 3 testimonials → pricing → CTA → footer**, quebre.

Sugestões (escolha 2-3):

- Pular o "about" e fundir com hero
- Colocar pricing depois do hero, não no fim (se preço é parte da decisão)
- Trocar 3 testimonials por 1 longo + 2 curtos
- Adicionar uma seção que NÃO existe no template default IA: galeria, mapa, agenda da semana, FAQ específico, "como funciona em 4 passos"
- Mudar densidade tipográfica: 1 seção bem espaçada, próxima compacta
- Ícones: ou tira tudo, ou usa 1 estilo coerente em todos os lugares (não Lucide aleatório)

---

## Fase 5 — Trust signals e detalhes que vendem realismo

Adicione (ou ajuste) coisas que produto real tem:

- **Mapa embedado** se for negócio físico (pode ser screenshot estático, não precisa ser Google Maps real)
- **Horário de funcionamento** explícito
- **Política de cancelamento / reembolso** em uma página interna
- **FAQ** com 5-6 perguntas que cliente real faz (não as 3 genéricas)
- **Página "Sobre"** que conta uma história curta — não bullets de valores
- **Mensagem de erro** customizada se o form falhar
- **Loading state** decente
- **Empty state** se aplicável
- **Open Graph meta tags** com a tagline (não "Site criado com Vite")

---

## Fase 6 — README do study case

Estrutura mínima:

```markdown
# <Nome do Negócio>

<1 linha sobre o que é>

<link demo se houver>

## Sobre este repositório

Este é um **study case** — projeto fictício criado para demonstrar habilidade em landing pages para o nicho de <nicho>. Não há cliente real associado.

## Stack

`<stack 1> · <stack 2> · <stack 3>`

## Decisões de design

- <Por que esta paleta>
- <Por que esta estrutura>
- <Por que esta tipografia>

## Run local

```bash
npm install
npm run dev
```

## Deploy

<URL Vercel/Netlify se houver>
```

**Se o repo é público sem README ou com README do bootstrap default, é red flag visível pra recrutador.** README precisa existir.

---

## Fase 7 — Build + deploy

```bash
npm install
npm run build
npm run dev
# abre no browser, percorre tudo, confere mobile (DevTools responsive)
```

Se já está deployado em Vercel/Netlify e o auto-deploy do GitHub está ativo, o push do branch novo já vai gerar preview. Use o preview pra revisar antes do merge.

Se não está deployado, **considere criar deploy Vercel free** — landing sem demo viva é meio caminho do portfolio.

---

## Fase 8 — Commits limpos

Trabalhe em branch `polish/<repo>` (não direto na main). Um commit por bloco lógico:

```
content(hero): copy específico do nicho de detalhamento automotivo
content(features): reduz de 6 genéricas para 3 com profundidade
content(testimonials): substitui depoimentos plásticos por relatos de bairro real
ui(layout): quebra simetria padrão movendo pricing para depois do hero
chore(seo): meta tags Open Graph com tagline
docs(readme): declara study-case + decisões de design
```

Se a história já está no GitHub e tem commits horríveis ("chore: initial commit", "feat: add stuff"), considere **squash em commit final único**. Force-push só com confirmação.

---

## Fase 9 — Anti-IA pass final (rode antes de declarar pronto)

```bash
# frases delatoras
grep -rE "(Let's dive|It's worth noting|Make sure to|Built with [❤️♥️]|Made with [❤️♥️]|next-level|game[- ]chang|cutting[- ]edge|elevar|alavancar|inovador|excelência em|soluções inovadoras|transforme seu)" --include="*.md" --include="*.tsx" --include="*.ts" --include="*.html" .

# excesso de emoji em headers (>2 no doc todo)
grep -E "^#+ .*[🎯🚀✨🔥💡📦🛠️⚡🌟🎨🔒💪]" README.md | wc -l

# bullets simétricos parafraseando
# (revisão manual — olhe blocos de 3 bullets seguidos com mesma estrutura sintática)

# stats redondos suspeitos
grep -rE "\+?1000\s|99%|98%|95%" --include="*.tsx" --include="*.html" .

# nomes plásticos
grep -rE "(João|Maria) (da )?Silva" .

# console.log esquecido
grep -rE "console\.(log|debug)\(" --include="*.ts" --include="*.tsx" src/

# disclaimer IA esquecido
grep -rE "(generated by AI|Claude|GPT|Copilot|prompted)" .
```

Se algum desses retorna match, volta e corrige.

---

## Formato de relatório por repo

Ao terminar cada repo, antes de passar pro próximo, entregue:

```
=== REPO: <nome> ===

Identidade definida:
  Nome:     ...
  Tagline:  ...
  Persona:  ...
  Tom:      ...

Pegadas IA antes / depois:
  Hero copy genérico:     SIM → reescrito com âncora no nicho
  Features simétricas:    6 → 3 com profundidade
  Testimonials plásticos: 3 → 2 com nome de bairro
  Stats sem fonte:        2 → 0
  README vazio:           SIM → declara study-case + design decisions
  Commits genéricos:      8 → consolidados em 6 contextualizados
  Layout simétrico IA:    SIM → quebrado (pricing após hero, FAQ adicionado)
  Frases delatoras:       4 → 0
  Emojis em headers:      ok / excesso

Build:           ✓/✗
Mobile OK:       ✓/✗
Deploy preview:  <URL ou N/A>

Decisões pendentes (precisam você):
  1. <ação> — <por quê>
  2. ...

VEREDICTO: PRONTO_PARA_PORTFOLIO | PRECISA_AJUSTES_PEQUENOS | RETRABALHO_PROFUNDO
```

Só depois passe pro próximo repo da lista.

---

## Comando inicial

Quando terminar de ler isso, execute, nessa ordem:

```bash
ls -la                              # ver os repos disponíveis em Showcases/
cd <primeiro-repo-da-lista>
git status
git log --oneline | head -10
ls -la
```

E aguarde minha confirmação do **NOME DO NEGÓCIO + PERSONA + TOM** antes de tocar em código. Se eu já preenchi nas variáveis no topo, prossiga; se não preenchi, **pergunte**.
