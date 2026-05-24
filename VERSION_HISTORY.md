# 💎 Holocron — Histórico Completo de Versões

> Da primeira versão experimental ("Blip Builder Diff") até a atual **v1.5.0 "Polish & Safety"**

**Última atualização:** 24 de maio de 2026

---

## 📊 Timeline

```
2026
├── v1.5.0 ─────────────────── 24/05  🎯 Polish & Safety (atual)
├── v1.4.9 ─────────────────── 23/05  📑 Reorder zones + Fix scroll
├── v1.4.8 ─────────────────── 23/05  🖱️ Menu visual de alinhamento
├── v1.4.7 ─────────────────── 23/05  📐 Block Alignment API
├── v1.4.6 ─────────────────── 23/05  🛡️ Block Healer + Snap to Grid
├── v1.4.5 ─────────────────── 23/05  ✅ THE FIX (debouncedSave)
├── v1.4.4 ─────────────────── 23/05  🔧 jsPlumb revalidate
├── v1.4.3 ─────────────────── 23/05  🔬 reference-replace + HOLOCRON debug
├── v1.4.2 ─────────────────── 23/05  💣 shotgun + diagnostic dump
├── v1.4.1 ─────────────────── 23/05  🔨 Harden zone drag
├── v1.4.0 ─────────────────── 23/05  🤏 Zone Grab (primeira)
├── v1.3.0 ─────────────────── 23/05  💎 Rebrand → Holocron
│
└── era "Blip Builder Diff" (v1.0 → v1.2.x)
    ├── v1.2.x ─────────────── pré-rebrand: Tools menu + AI Agents + Git
    ├── v1.1.x ─────────────── Zonas + Import cross-tenant + Multi-idioma
    └── v1.0.0 ─────────────── Diff antes de publicar (MVP)
```

---

# 🚀 v1.5.0 — "Polish & Safety"

**Data:** 24/05/2026 · **Status:** Atual · **Bundle:** 304 KB · **Zip:** 81 KB

**Tema:** refinamento das features existentes + 7 proteções automáticas. Foco em integrar com APIs nativas do Blip Builder pra reduzir custom code e aumentar confiabilidade.

## ✨ Novidades

### 🆚 Diff visual antes de publicar *(polido)*
Compara o flow atual com a última versão publicada lado-a-lado, estilo `git diff`:
- Mensagens adicionadas em **verde**
- Removidas em **vermelho**
- Modificadas mostradas como antes/depois
- Detecta corretamente inserções no meio de listas (algoritmo LCS — Longest Common Subsequence, mesmo do `git diff`)

Antes de clicar "Publicar", você vê exatamente o que mudou — adeus surpresas em produção.

### 🔍 Lint do flow com 10 regras *(R8 e R9 novas)*
Análise estática que detecta bugs **antes** do publish:

| Regra | O que detecta | Severidade |
|-------|---------------|------------|
| R1 | Bloco sem nenhuma saída | warn |
| R2 | Bloco órfão (não alcançável) | warn |
| R3 | Variável usada mas não atribuída | warn |
| R4 | Variável definida nunca usada | info |
| R5 | Mensagem vazia | warn |
| R6 | Self-loop infinito | error |
| R7 | URL HTTP não-HTTPS em request action | warn |
| **R8** | **`$invalid*` nativo do Blip** — pipeia os flags que o próprio Blip já calcula | **error** |
| **R9** | **BIG_DISPATCHER** — bloco com >10 saídas (candidato a subflow) | **info** |
| R10-12 | Validações específicas pra AI Agents (handoffs, etc) | varia |

**Por que R8 é importante:** zero falso positivo, porque é a mesma validação que o Blip Builder usa internamente.

### ⚡ Tab Duplication Detection *(NOVO)*
Quando você abre o mesmo bot em 2+ abas simultaneamente, o Holocron detecta via **BroadcastChannel** API e mostra um toast de aviso:

```
⚠️ Bot aberto em 2 abas simultaneamente. Mexer em mais de uma pode
causar conflito de save — a última edição sobrescreve as outras.
```

**Tecnologia:** heartbeat de 30s entre abas, com cleanup automático de abas que ficam silenciosas por >90s. Debug via `HOLOCRON_TAB_DEBUG()` no console.

### 🍞 Toasts nativos via BlipToastService *(NOVO)*
Notificações da extensão agora usam o sistema visual nativo do Blip Builder. Wrapper `showHolocronToast()` tem fallback em 3 camadas:

1. **BlipToastService** (visual integrado com o resto do Builder)
2. **ngToast** se BlipToast não existir
3. **Toast próprio Holocron** (dark + cyan) se nenhum dos dois disponível

Tipos: `success` · `info` · `warn` · `error`. Duração configurável.

### ⌨️ Atalhos globais completos *(NOVO)*

| Atalho | Ação |
|--------|------|
| `Ctrl+Shift+L` | 🔍 Lint do flow |
| `Ctrl+Shift+F` | 🔎 Find & Replace |
| `Ctrl+Shift+H` ou `J` | 📜 Histórico de versões |
| `Ctrl+Shift+P` ou `M` | 🛠️ Tools menu (Palette) |
| `Ctrl+Shift+G` | 🎯 Goto bloco |
| **`Ctrl+Shift+?`** | **❓ NOVO** — overlay com todos os atalhos |
| **`Esc`** | **❌ NOVO** — fecha qualquer modal/menu do Holocron |

Todos bloqueados automaticamente quando você está digitando em input/textarea (evita conflito).

### 📊 Status bar enriquecida *(NOVO)*
Footer persistente mostra ao vivo:

```
💎 Holocron · 📦 30 blocos · ⚠️ 0 inválidos · 📍 6 zonas · 🤖 3 agents
              ⏱ 5min · 🩹 0 broken · ✅ Saved · ?ajuda · ×
```

Click em cada métrica abre a tool relacionada. **NOVO**: indicador 🩹 broken (healthcheck — click pra curar) e ⏳/✅ Saving/Saved em tempo real via `$rootScope.saving`.

### 🏷️ Rebrand técnico
- `window.__BBD_BUILD` (legacy) → `window.__HOLOCRON_VERSION` (canônico)
- Alias `__BBD_BUILD` mantido pra retrocompatibilidade

## 🛡️ Garantias

- Todos os atalhos são bloqueados quando user está digitando em input
- Tab detection só fala em browser com BroadcastChannel (todos modernos)
- Toasts têm fallback em 3 camadas
- Lint R8 não causa duplicação com R1-R7
- Zero permissões novas no manifest

---

# 🎯 v1.4.9 — Reordenar zonas + Fix scroll jump

**Data:** 23/05/2026 · **Status:** Shipped

**Tema:** dois fixes de UX que vieram direto de feedback do usuário.

## ✨ Drag-and-drop pra reordenar zonas *(handle ≡ esquerda)*

Cada row do painel de zonas ganhou um **handle `≡`** no início:

```
┌──┬──┬─────────────┬───┬───┐
│ ≡│ 🟣│ Inicio      │ ⋯ │ × │  ← arrasta o ≡ pra reordenar
└──┴──┴─────────────┴───┴───┘
   5 blocos contidos
```

Click + segura no `≡`, arrasta verticalmente, **linha cyan brilhante** indica onde vai cair (topo ou base da zona alvo). Ordem persiste automaticamente.

**Proteção:** drag em INPUT/TEXTAREA/SELECT é bloqueado — não interfere com edição dos campos.

## 🐛 Fix scroll subir ao clicar em checkbox/botão do painel

**Causa raiz:** `refreshZonesPanel()` fazia `p.innerHTML = h` (re-render completo), o que recria todos elementos DOM e o `scrollTop` volta a 0.

**Solução em 2 partes:**
1. **Preservar `scrollTop`:** salva antes do `innerHTML`, restaura após
2. **Preservar foco:** captura `document.activeElement` e refocaliza o equivalente após re-render

**Aplicado em ~10 lugares** que chamam `refreshZonesPanel()`:
- Checkboxes (Travada, AutoFit)
- Selects (Categoria)
- Botões (Ajustar aos blocos, Re-vincular, Toggle expand, Delete)
- Inputs (Nome, Descrição, URL, Owner, Cor)

Resultado: você clica em qualquer botão do painel e **permanece exatamente onde estava olhando**.

---

# 🖱️ v1.4.8 — Menu visual de alinhamento

**Data:** 23/05/2026 · **Status:** Shipped

**Tema:** trazer as features de alignment (que estavam só no console) pra uma UI visual + corrigir comportamento de drag confuso.

## ✨ Botão direito no nome da zona = menu colorido

Em vez de comandos no console, agora um menu estilizado aparece na posição do cursor:

```
┌─────────────────────────────────┐
│ ENTRADA · 6 blocos              │
├─ 📐 Alinhar pelos extremos ─────┤
│ ↑ Pelo topo                     │
│ ↓ Pela base                     │
│ ← Pela esquerda                 │
│ → Pela direita                  │
├─ 📐 Centralizar ────────────────┤
│ ═ Mesma altura (Y)              │
│ ║ Mesma coluna (X)              │
├─ 📏 Distribuir (≥3 blocos) ─────┤
│ ⇔ Horizontalmente               │
│ ⇕ Verticalmente                 │
├─ 🛠️ Ações ──────────────────────┤
│ 🧲 Snap zona à grade (20px)     │
│ 🩹 Curar blocos travados        │
│ 💡 Destacar blocos da zona      │
└─────────────────────────────────┘
```

**Características:**
- Auto-ajusta se sair da tela
- Fecha com `Esc` ou click fora
- Usa a cor da própria zona no header

## 🐛 Drag no corpo da zona não move mais blocos *(só pelo nome)*

Comportamento agora é explícito por área de click:

| Click | Comportamento |
|-------|---------------|
| 🖱️ **NOME** (label) | Move zona + blocos juntos |
| 🖱️ **CORPO** da zona (área tracejada) | Move só a zona (blocos ficam) |
| 🖱️ **HANDLE** de resize (canto) | Redimensiona zona |
| 🖱️ **Click curto** no nome (sem drag) | Destaca blocos da zona |

Resolve o feedback "tentei mexer dentro da zona e os blocos também mexeram".

---

# 📐 v1.4.7 — Block Alignment API

**Data:** 23/05/2026 · **Status:** Shipped

**Tema:** sistema completo de alinhamento programático com garantias de segurança rigorosas.

## ✨ API exposta

```js
HOLOCRON.alignAll()                          // Snap todos blocos pra grid
HOLOCRON.alignInZone('Inicio', 'top')        // Alinha blocos da zona
HOLOCRON.align(['id1', 'id2'], 'distributeH')// Alinha blocos por ID
HOLOCRON.alignModes()                        // Lista modos disponíveis
```

## Modos disponíveis

| Modo | Comportamento |
|------|---------------|
| `top` | Todos com mesmo `y` (do mais alto) |
| `bottom` | Todos com mesmo `y + altura` (do mais baixo) |
| `left` | Todos com mesmo `x` (do mais à esquerda) |
| `right` | Todos com mesmo `x + largura` (do mais à direita) |
| `middleX` | Mesma altura vertical (média) |
| `middleY` | Mesma posição horizontal (média) |
| `distributeH` | Espaço horizontal igual (≥3 blocos) |
| `distributeV` | Espaço vertical igual (≥3 blocos) |

## 🛡️ Garantias de segurança

| Camada | Proteção |
|--------|----------|
| Mutação | Só `block.$position.left/top` |
| Snapshot | Pushed pro histórico antes (rotulo `pre_align_*`) — Ctrl+Z desfaz |
| Idempotência | Se posição não mudar, skip silencioso |
| Heal pós-align | Auto-detecta+cura qualquer side-effect (+100ms) |
| jsPlumb | `revalidate()` por bloco — linhas redesenham |
| Persistência | `debouncedSave()` — confirma em ~2s |

**Nunca tocado:** `$contentActions`, `$conditionOutputs`, `$defaultOutput`, scripts JS, tags, IDs, títulos.

---

# 🛡️ v1.4.6 — Block Healer + Snap to Grid

**Data:** 23/05/2026 · **Status:** Shipped

**Tema:** proteção permanente contra blocos corrompidos + alinhamento automático de blocos pra linhas sempre retas.

## ✨ Block Healer — cura + previne

Sistema que detecta e corrige automaticamente blocos com **4 sintomas patológicos**:

1. `$position.left` ou `top` é número (deveria ser string `"Npx"`)
2. `el.style.left/top` vazio mas `$position` tem valor
3. Falta classe `jtk-draggable` (jsPlumb perdeu registro)
4. Falta classe `jtk-droppable`

**Cura aplicada:**
- Converte `$position` pra strings `"Npx"`
- Sincroniza `el.style.left/top`
- Re-registra no jsPlumb: `manage` + `draggable` + `makeTarget` + `makeSource` + `revalidate`
- `$scope.$apply()` + `debouncedSave()` pra persistir

**4 gatilhos de defesa:**

| Quando | Por quê |
|--------|---------|
| Startup (3s após flow carregar) | Cura blocos quebrados antes mesmo do usuário interagir |
| **Antes** de cada drag de zona | Garante estado limpo |
| **Depois** de cada drag de zona (+100ms) | Pega qualquer side-effect |
| Manual via `HOLOCRON.heal()` | Quando quiser |

**Cooldown de 3s** entre execuções (sem loops infinitos).

## ✨ Snap to Grid 20px — linhas sempre retas

Toda posição final de bloco é arredondada pro múltiplo de 20px mais próximo:

```javascript
function snapToGrid(value) {
  if (!SNAP_ENABLED || SNAP_GRID_SIZE <= 1) return value;
  return Math.round(value / SNAP_GRID_SIZE) * SNAP_GRID_SIZE;
}
```

Resultado: blocos sempre alinhados → **linhas/setas naturalmente retas** (sem curvas estranhas por causa de offsets ímpares de 1-3px).

**Configurável:**
```js
HOLOCRON.snap.size = 10        // grade mais fina (10px)
HOLOCRON.snap.enabled = false  // desliga snap
HOLOCRON.snap.status()         // { enabled: true, sizePx: 20 }
```

## 🆕 API exposta

```js
HOLOCRON.heal()           // Cura todos os blocos quebrados agora
HOLOCRON.healthCheck()    // Só detecta (sem mutar), retorna lista
```

---

# ✅ v1.4.5 — THE FIX (debouncedSave descoberto live)

**Data:** 23/05/2026 · **Status:** Shipped · **Marco crítico**

**Tema:** após 5 iterações falhas, a chave foi descobrir o método de save nativo do Blip Builder.

## 🔬 A jornada (5 versões pra acertar)

| Versão | Tentativa | Resultado |
|--------|-----------|-----------|
| v1.4.0 | Drag de zona pelo nome, mutate `$position.left/top` | ❌ Blocos não movem visualmente |
| v1.4.1 | Hardening: CSS transform + fallback inline-style | ✅ Movem visual ❌ Linhas paradas + F5 reverte |
| v1.4.2 | Shotgun: mutar todo campo plausível + diag dump | ⚠️ Provoca side-effect: 3 blocos travados! |
| v1.4.3 | Reference-replace de `$position` + window.HOLOCRON | ❌ Ainda não persiste |
| v1.4.4 | jsPlumb `revalidate()` — linhas voltaram a seguir | ✅ Visual completo ❌ F5 ainda reverte |
| **v1.4.5** | **`activeCtrl.debouncedSave()`** | ✅ **TUDO FUNCIONA + persiste** |

## ✨ Receita correta em 5 linhas

```javascript
// 1. Mutate $position com nova referência (força $watch)
b.$position = { left: nx + 'px', top: ny + 'px' };

// 2. Set inline style (Blip usa imperativo, não binding)
el.style.left = nx + 'px';
el.style.top = ny + 'px';

// 3. jsPlumb revalidate — redesenha conexões
activeCtrl.builderInstance.revalidate(el);

// 4. Angular digest direto via $scope
activeCtrl.$scope.$apply();

// 5. DEBOUNCED SAVE — debounce ~2s, completa em ~500ms
activeCtrl.debouncedSave();
```

## 🧪 Validado live via Chrome MCP

No bot `hmgonboardingnegdia`:
- Movi bloco `onboarding` de `x=783` pra `x=793` (+10px)
- `saving = true` em **+2000ms** após `debouncedSave()`
- `saving = false` em **+2500ms** (completou em 500ms)
- F5 → posição persistiu como `793px` ✓
- Rollback pra `783px` → F5 → restaurou ✓

## ❌ O que removi vs v1.4.2 (shotgun → cirúrgico)

- Mutação de `position`, `x/y`, `layout` — confirmado: só `$position` existe
- Chamadas a `SavingService.save/markChanged/etc` — não existem
- `$rootScope.$broadcast('flow:changed'/etc)` — sem listeners
- `activeCtrl.dirty = true` flags — Blip não usa
- Diag dump verbose — fix confirmado

---

# 🔧 v1.4.4 — jsPlumb revalidate

**Data:** 23/05/2026 · **Status:** Shipped

**Tema:** descobrir como redesenhar as linhas do canvas após mover blocos.

## A descoberta

Inspecionei o HTML do bloco no Builder e encontrei:

```html
<builder-node class="jtk-draggable jtk-droppable jtk-endpoint-anchor jtk-connected"
              on-update-element="$ctrl.builderInstance.revalidate($node)" ...>
```

As classes `jtk-*` são do **jsPlumb** (biblioteca de diagrams). E o atributo HTML literalmente diz o nome do método: `builderInstance.revalidate($node)`.

## ✨ Fix aplicado

```javascript
blocks.forEach(b => {
  // ... mutate position ...
  if (activeCtrl.builderInstance && activeCtrl.builderInstance.revalidate) {
    activeCtrl.builderInstance.revalidate(blockElement);
  }
});

// Backup: repaint everything (mais pesado mas garante)
if (activeCtrl.builderInstance.repaintEverything) {
  activeCtrl.builderInstance.repaintEverything();
}
```

**Resultado:** linhas voltaram a seguir os blocos durante drag de zona. Mas F5 ainda revertia — esse problema só seria resolvido na v1.4.5.

---

# 🔬 v1.4.3 — Reference-replace + window.HOLOCRON

**Data:** 23/05/2026 · **Status:** Shipped

**Tema:** descobrir por que mutar `$position.left` não disparava `$watch` do Angular + expor internals pra debug live.

## ✨ Reference-replace

AngularJS faz `$watch` por **referência** por padrão. Mutar `obj.prop` não dispara o watch — precisa substituir `obj` inteiro:

```javascript
// ❌ ANTES (não dispara $watch):
b.$position.left = nx;
b.$position.top = ny;

// ✅ DEPOIS (força $watch a disparar):
b.$position = { ...b.$position, left: nx, top: ny };
```

## ✨ `window.HOLOCRON` exposto pra DevTools

Objeto que dá acesso aos internals do Holocron via console do navegador:

```js
HOLOCRON.dump()                 // dump completo do primeiro bloco
HOLOCRON.dump('onboarding')      // dump de um bloco específico
HOLOCRON.block('id')             // referência ao objeto bloco
HOLOCRON.flow()                  // todo o flow
HOLOCRON.blockIds()              // lista de IDs
HOLOCRON.services()              // serviços do injector do Blip
HOLOCRON.injector()              // $injector raw
HOLOCRON.ctrl()                  // activeCtrl
```

Foi esse `HOLOCRON.dump()` que permitiu descobrir o `debouncedSave` na v1.4.5.

---

# 💣 v1.4.2 — Shotgun position save + diagnostic dump

**Data:** 23/05/2026 · **Status:** Shipped *(causou bug colateral resolvido na v1.4.6)*

**Tema:** tentar acertar o save mexendo em todo campo plausível ao mesmo tempo.

## ✨ Estratégia shotgun

Como não sabíamos qual campo era source-of-truth, mutamos todos os candidatos:

```javascript
// $position
b.$position.left = nx;
b.$position.top = ny;

// position alternativa
if (b.position) {
  b.position.x = nx; b.position.y = ny;
  b.position.left = nx; b.position.top = ny;
}

// x/y direto
if (typeof b.x === 'number') b.x = nx;

// layout
if (b.layout) { b.layout.x = nx; b.layout.y = ny; }
```

E broadcast de eventos:
```javascript
['flow:changed', 'flow:dirty', 'block:moved', 'save'].forEach(evt =>
  $rootScope.$broadcast(evt)
);
```

## 🐛 Efeito colateral

Em alguns blocos, a mutação fez `$position` virar **número** em vez de string `"Npx"`. Isso:
1. Quebrou o Blip's positioning logic (browser ignora `el.style.left = 442` sem unidade)
2. jsPlumb perdeu o tracking → classes `jtk-draggable` sumiram
3. Blocos ficaram **travados** fora de posição, sem ser movíveis ou editáveis

3 blocos do bot `hmgonboardingnegdia` foram afetados. Corrigidos manualmente via Chrome MCP, e o sistema **Block Healer** (v1.4.6) foi criado pra prevenir/curar isso automaticamente no futuro.

## 📚 Lição aprendida

**Shotgun não é grátis** — pode quebrar coisas. Daí a importância da defesa em profundidade (Block Healer).

---

# 🔨 v1.4.1 — Harden zone drag

**Data:** 23/05/2026 · **Status:** Shipped

**Tema:** primeira tentativa de fix após v1.4.0 não funcionar.

## ✨ Hardenings

1. **Qualquer drag da zona** (label OU corpo) move blocos juntos
2. **CSS transform direto nos blocos durante o drag** — visual instantâneo, independente do Angular
3. **Fallback inline-style no commit** — se Angular binding falhar, força `el.style.left/top` direto
4. **Preserva formato `px`** vs número puro (detecta o tipo do `$position` original)
5. **Debug logs sobreviventes ao Terser** — usando `window['console']['log']` (bracket notation que não cai no `pure_funcs`)

## 🎯 Resultado

Blocos passaram a mover **visualmente** durante o drag. Mas:
- ❌ Linhas/setas ainda paradas no lugar (próximo problema, v1.4.4)
- ❌ F5 ainda revertia tudo (problema final, v1.4.5)

---

# 🤏 v1.4.0 — Zone Grab (primeira versão)

**Data:** 23/05/2026 · **Status:** Shipped

**Tema:** atender a primeira solicitação do usuário — drag de zona pelo nome move blocos junto.

## ✨ Comportamento proposto

| Click | Comportamento |
|-------|---------------|
| 🖱️ Drag pelo **nome da zona** | Move zona + blocos contidos juntos |
| 🖱️ Click curto no nome | Destaca blocos contidos por 4s |
| 🔒 Zona travada | Cursor `not-allowed`, nada move |

## ✨ Garantias de segurança

- Snapshot empurrado pro Histórico ANTES do commit (`pre_zone_drag`)
- Click vs drag detection (movimento <2px = click)
- RAF-throttled Angular digest pra performance
- Comportamento antigo 100% preservado (resize handle, etc)

## 🐛 Resultado real

Usuário reportou: **"não move, eu estou mexendo a zona e não move os blocos"**.

Isso iniciou a saga v1.4.1 → v1.4.5 pra descobrir COMO fazer o save funcionar.

---

# 💎 v1.3.0 — Rebrand: "Blip Builder Diff" → "Holocron"

**Data:** 23/05/2026 · **Status:** Shipped (já publicada na CWS)

**Tema:** mudança de nome pra evitar trademark conflicts + nova identidade visual.

## 🎨 Mudanças de identidade

| Item | Antes | Depois |
|------|-------|--------|
| Nome | Blip Builder Diff | **Holocron** |
| Símbolo | (sem) | 💎 (cristal Jedi de conhecimento) |
| Cor primária | (sem) | Cyan `#22d3ee` |
| Cor secundária | (sem) | Deep blue `#0c1a4d` |
| Ícones | (sem) | 16/48/128 PNG (cristal cyan com glow) |

## 📝 Mudanças técnicas

- Author: `Kagi Adrian Garcia` adicionado no manifest
- Description ≤132 chars (limite CWS): `"Holocron — diff, lint, snippets, find/replace, history, AI agents and Git for Blip Builder devs."`
- Status bar: `<span class="brand">BBD</span>` → `<span class="brand">💎 Holocron</span>`
- Audit payloads: `extension: 'blip-builder-diff'` → `extension: 'holocron'`
- Exports Markdown/Mermaid: `Generated by blip-builder-diff` → `Generated by Holocron`
- Popup HTML redesenhado: header com emoji 💎, paleta cyan/deep-blue

## 🛠️ Mudanças de compliance pra CWS

- ❌ → ✅ Removido `webpack-obfuscator` (Chrome Web Store **proíbe** obfuscação)
- ❌ → ✅ Adicionados ícones 16/48/128 PNG no manifest
- ❌ → ✅ Removida chamada `api.ipify.org` não declarada
- ✅ Bundle reduziu de 226 KB → 72 KB (3x menor sem obfuscação)
- ✅ Privacy Policy atualizada e hospedada publicamente

---

# 📦 Era "Blip Builder Diff" (v1.0 → v1.2.x)

## v1.2.x — Tools menu + AI Agents + Git multi-provider

**Tema:** consolidação de ferramentas avançadas.

### Features acumuladas

- **Tools menu** centralizado: lint, find&replace, snippets, history, export, import
- **AI Agents viewer** — lista agentes IA do bot atual (handoffs, instructions, model, temperature)
- **Integração Git multi-provider**: GitHub, GitLab, Bitbucket, Azure DevOps, custom webhook
- **Export Markdown/Mermaid** do flow (commit-ready)
- **Delete-log mínimo** via API nativa do Blip (`SegmentService.createBotTrack`) — único evento de auditoria interna

### Bugfixes notáveis

- **Publish premature success**: `_bbdOriginal()` resolvia antes do publish real concluir. Fix: polling `isPublishing` true→false em vez de Promise.
- **Idioma travado em EN**: `localStorage.getItem('bbd:lang')` lia vestígio antigo. Fix: bridge `chrome.storage` retransmite múltiplas vezes.

---

## v1.1.x — Zonas + Import cross-tenant + Multi-idioma

**Tema:** organização visual do canvas e portabilidade entre bots.

### Features adicionadas

- **Zonas no canvas** — overlays coloridos pra agrupar blocos
  - 8 categorias built-in: Custom, WIP, Pronto, Bug, Crítico, Refactor, Experimento, Agente IA
  - Categorias customizáveis (cor + ícone + nome próprio)
  - Persistidas em `localStorage` + sync com Bucket do bot
- **Import de fluxo entre bots** (cross-tenant): copia flow de um bot pra outro (mesma conta ou diferente)
- **Multi-idioma**: 🇧🇷 PT-BR · 🇵🇹 PT-PT · 🇺🇸 EN · 🇪🇸 ES (preferência salva localmente)
- **Snippets reutilizáveis**: salva blocos como snippets pra reusar em qualquer bot
- **Histórico de versões local** (mini-git interno): timeline com diff entre versões + restore

---

## v1.0.x — MVP: Diff antes de publicar

**Tema:** o problema original que originou tudo.

### Feature única (mas matadora)

**🆚 Diff semântico antes de publicar**
- Intercepta o botão "Publicar" do Blip Builder
- Snapshot do flow atual vs último snapshot salvo
- Algoritmo **LCS (Longest Common Subsequence)** — mesmo do `git diff`
- Modal com cores:
  - ✅ Mensagens adicionadas (verde)
  - ❌ Mensagens removidas (vermelho)
  - ✏️ Mensagens modificadas (antes/depois)
  - = Mensagens iguais em cinza (pra contexto)
- Detalhes por bloco: ações de entrada/saída, condições, variáveis, ações globais
- Botão "Publicar mesmo assim" / "Cancelar"
- Spinner enquanto aguarda + feedback de sucesso/erro com hints

**Lint inicial (7 regras):**
- Bloco sem saída, órfão, variável quebrada, variável inutilizada, mensagem vazia, self-loop, HTTP não-HTTPS

**Find & Replace global** — buscar texto em todos os blocos (mensagens, conditions, variáveis, JS actions)

---

# 📊 Tabela-resumo: features por versão

| Feature | v1.0 | v1.1 | v1.2 | v1.3 | v1.4 | v1.5 |
|---------|:---:|:---:|:---:|:---:|:---:|:---:|
| Diff antes de publicar | ✨ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Lint do flow | ✨ 7 | ✓ | ✓ | ✓ | ✓ | ✨ 10 |
| Find & Replace global | ✨ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Snippets reutilizáveis | | ✨ | ✓ | ✓ | ✓ | ✓ |
| Histórico de versões | | ✨ | ✓ | ✓ | ✓ | ✓ |
| Zonas no canvas | | ✨ | ✓ | ✓ | ✨ drag | ✓ |
| Import cross-tenant | | ✨ | ✓ | ✓ | ✓ | ✓ |
| Multi-idioma (4) | | ✨ | ✓ | ✓ | ✓ | ✓ |
| Tools menu | | | ✨ | ✓ | ✓ | ✓ |
| Integração Git | | | ✨ | ✓ | ✓ | ✓ |
| AI Agents viewer | | | ✨ | ✓ | ✓ | ✓ |
| Export Markdown/Mermaid | | | ✨ | ✓ | ✓ | ✓ |
| Delete-log auditoria | | | ✨ | ✓ | ✓ | ✓ |
| Status bar persistente | | | | ✨ | ✓ | ✨ rica |
| Ícones 16/48/128 | | | | ✨ | ✓ | ✓ |
| Privacy Policy pública | | | | ✨ | ✓ | ✓ |
| Drag zona pelo nome | | | | | ✨ 1.4.0 | ✓ |
| jsPlumb revalidate | | | | | ✨ 1.4.4 | ✓ |
| **debouncedSave** (THE FIX) | | | | | ✨ 1.4.5 | ✓ |
| Block Healer | | | | | ✨ 1.4.6 | ✓ |
| Snap to Grid 20px | | | | | ✨ 1.4.6 | ✓ |
| Block Alignment API | | | | | ✨ 1.4.7 | ✓ |
| Menu visual right-click | | | | | ✨ 1.4.8 | ✓ |
| Reorder zonas drag-drop | | | | | ✨ 1.4.9 | ✓ |
| Scroll preservation | | | | | ✨ 1.4.9 | ✓ |
| Tab Duplication Detection | | | | | | ✨ |
| Toasts nativos (BlipToast) | | | | | | ✨ |
| Lint R8 nativo Blip | | | | | | ✨ |
| Lint R9 BIG_DISPATCHER | | | | | | ✨ |
| Atalhos globais completos | | | | | parcial | ✨ |
| Save status ao vivo | | | | | | ✨ |

Legenda: ✨ = adicionado nessa versão · ✓ = continua funcionando

---

*Documento atualizado em 24/05/2026 · Holocron v1.5.0 · "Polish & Safety"*
