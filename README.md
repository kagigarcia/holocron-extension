<div align="center">

# 💎 Holocron

### *Toolkit de produtividade não-oficial pra devs do Blip Builder*

[![Chrome Web Store](https://img.shields.io/badge/Chrome%20Web%20Store-Instalar-22d3ee?logo=googlechrome&logoColor=white)](https://chromewebstore.google.com/detail/kkpdgdkicklljbffacpdhgcbcijlbjdf)
[![Version](https://img.shields.io/badge/version-1.5.0-blueviolet)](./CHANGELOG.md)
[![License](https://img.shields.io/badge/license-Proprietary-lightgrey)](#-licença)
[![Privacy](https://img.shields.io/badge/privacy-no_PII-success)](./PRIVACY.md)

</div>

---

## 📖 O que é?

**Holocron** é uma extensão Chrome **não-oficial** que adiciona ferramentas de produtividade ao **Blip Builder** — a plataforma low-code da Blip pra construir chatbots.

Pensada por quem usa o Builder no dia-a-dia: traz diff visual antes de publicar, lint do flow, organização por zonas coloridas, alinhamento automático, histórico de versões local, atalhos de teclado, e muito mais.

> *Nome inspirado nos "holocrons" — cristais Jedi que armazenam conhecimento. A ideia é deixar o Builder mais inteligente sem mexer no que já funciona.*

---

## ✨ Principais features

### 🆚 Diff visual antes de publicar
Compare o flow atual com a última versão publicada lado-a-lado, no estilo `git diff`. Veja exatamente o que mudou em mensagens, ações, condições e variáveis **antes** de clicar "Publicar".

### 🔍 Lint do flow com 10 regras
Análise estática que detecta bugs antes do publish: variáveis quebradas, blocos órfãos, loops infinitos, mensagens vazias, URLs HTTP, e mais. Inclui regras que usam os próprios flags `$invalid` que o Blip já calcula (zero falso positivo).

### 🎯 Zonas no canvas
Crie áreas coloridas pra agrupar blocos por contexto (Onboarding, FAQ, Checkout, etc).
- 8 categorias prontas + crie suas próprias
- Drag pelo nome da zona = move zona + blocos juntos
- Right-click no nome = menu visual de alinhamento
- Reordene as zonas no painel via drag-and-drop

### 📐 Alinhamento + Snap to Grid
Alinhe blocos por topo, base, laterais, ou distribua igualmente. Snap automático em grid de 20px (configurável) garante linhas sempre retas.

### 🛡️ Block Healer
Detecta e cura automaticamente blocos com posição corrompida, em 4 gatilhos preventivos. Resolve definitivamente o problema do "bloco travado fora de posição".

### ⌨️ Atalhos globais
| Atalho | Ação |
|--------|------|
| `Ctrl+Shift+L` | Lint |
| `Ctrl+Shift+F` | Find & Replace global |
| `Ctrl+Shift+H` | Histórico de versões |
| `Ctrl+Shift+P` | Tools menu (Palette) |
| `Ctrl+Shift+G` | Goto bloco |
| `Ctrl+Shift+?` | Ajuda dos atalhos |
| `Esc` | Fecha qualquer modal |

### ⚡ Tab Duplication Detection
Avisa automaticamente se o mesmo bot está aberto em 2+ abas, evitando conflito de save.

### 📜 Histórico de versões local
Snapshots automáticos antes de cada publish. Restaure qualquer versão anterior com 1 click. Mini-git interno, sem servidor.

### 🔎 Find & Replace global
Busca e substitui em mensagens, conditions, variáveis e JS actions — em todos os blocos do flow.

### 📋 Snippets reutilizáveis
Salve blocos como snippets pra reutilizar em qualquer bot.

### 🔗 Integração Git multi-provider
Export do flow como Markdown/Mermaid commit-ready pra GitHub, GitLab, Bitbucket, Azure DevOps.

### 🤖 AI Agents viewer
Visualize os agents do bot atual (handoffs, instructions, model, temperature).

### 📥 Import de fluxo entre bots
Copie a estrutura de um bot pra outro (cross-tenant suportado).

### 🌐 Multi-idioma
🇧🇷 Português (Brasil) · 🇵🇹 Português (Portugal) · 🇺🇸 English · 🇪🇸 Español

### 📊 Status bar persistente
Footer mostra ao vivo: blocos, inválidos, zonas, agents, idade do snapshot, blocos quebrados, status de save, e dica de atalhos.

---

## 🚀 Instalação

### Recomendado — Chrome Web Store

🔗 **[Instalar Holocron](https://chromewebstore.google.com/detail/kkpdgdkicklljbffacpdhgcbcijlbjdf)**

1. Click "Adicionar ao Chrome"
2. Visite qualquer URL do Blip Builder (ex: `https://*.blip.ai/.../templates/builder/`)
3. Holocron aparece automaticamente — ícones na barra superior + status bar no rodapé

---

## 📚 Documentação

| Documento | Descrição |
|-----------|-----------|
| 📜 **[PRIVACY.md](./PRIVACY.md)** | Política de privacidade completa (também hospedada em [Gist público](https://gist.github.com/kagigarcia/2378d8f5c0980753e8ea6a4c58a15e4d)) |
| 📖 **[VERSION_HISTORY.md](./VERSION_HISTORY.md)** | Narrativa visual da evolução da extensão (v1.0 → v1.5.0) |

---

## 🛡️ Privacidade — em resumo

- ✅ **Não coleta** dados pessoais (PII)
- ✅ **Não envia** telemetria pra servidores próprios — não há servidor
- ✅ **Não compartilha** dados com terceiros (sem Google Analytics, Mixpanel, Sentry, OpenAI, etc.)
- ✅ Funciona **apenas dentro de `*.blip.ai`** — não enxerga outros sites
- ✅ Tudo armazenado no **`localStorage`** do navegador (apagável a qualquer momento)
- ℹ️ Único evento de auditoria interna: registro de exclusão de fluxo via API nativa do Blip
- ✅ Código **minificado** mas **NÃO obfuscado** (em conformidade com [Chrome Web Store policy](https://developer.chrome.com/docs/webstore/program-policies/code-readability))

📜 Leia a política completa em **[PRIVACY.md](./PRIVACY.md)**.

---

## 🆕 Updates recentes

### v1.5.0 — "Polish & Safety" *(atual)*
- ⚡ Tab Duplication Detection (avisa se mesmo bot em 2 abas)
- 🍞 Toasts nativos integrados com Blip
- 🔍 Lint R8: usa flags `$invalid` do próprio Blip (zero falso positivo)
- 🔍 Lint R9: detecta blocos com >10 saídas (candidatos a subflow)
- ⌨️ Atalhos globais completos + `Esc` global + `?` ajuda
- 📊 Status bar com indicador de "Saving..." / "Saved" ao vivo

### v1.4.9 — Reordenar zonas + Fix scroll
- Drag-and-drop com handle `≡` pra reordenar zonas
- Scroll do painel não sobe mais ao clicar em checkbox/botão

### v1.4.8 — Menu visual de alinhamento
- Right-click no nome da zona abre menu com 11 opções
- Drag no corpo da zona não move mais blocos (só pelo nome)

### v1.4.6 — Block Healer + Snap to Grid
- Auto-cura blocos com posição corrompida em 4 gatilhos
- Snap to grid 20px garante linhas sempre retas

### v1.4.5 — Drag de zona funcional (THE FIX)
- Arraste o NOME da zona pra mover zona + blocos juntos
- Persiste após F5 via `debouncedSave` nativo do Blip

📖 **Histórico:** [VERSION_HISTORY.md](./VERSION_HISTORY.md)

---

## 🤝 Contato & Suporte

| Canal | Para quê |
|-------|----------|
| 🐛 **[Reportar bug ou sugerir feature](https://forms.gle/gALjtjQWAMSxbDev6)** | Bugs, ideias, melhorias |
| ✉️ **`holocrongiindev@duck.com`** | Dúvidas sobre privacidade, segurança, GDPR |
| 👤 **Autor:** Kagi Adrian Garcia | Desenvolvedor responsável |

---

## ⚠️ Disclaimer

Holocron é uma extensão **não-oficial**, criada de forma independente e não afiliada nem endossada pela **Take Blip**. "Blip" e "Blip Builder" são marcas registradas da Take Blip — usadas aqui apenas em referência ao produto que a extensão complementa.

A extensão **não modifica os dados do bot no backend** — apenas adiciona ferramentas visuais e organizacionais que rodam no seu navegador. Todas as operações de publicação seguem fluxo nativo do Blip Builder.

---

## 📄 Licença

Software **proprietário**. Todos os direitos reservados © 2026 Kagi Adrian Garcia.

O código-fonte da extensão é **privado** por motivos de propriedade intelectual. Apenas a documentação pública (este README, Privacy Policy e Changelog) é compartilhada neste repositório.

Você é livre pra:
- ✅ Instalar e usar a extensão (gratuitamente)
- ✅ Reportar bugs e sugerir features
- ✅ Compartilhar o link da extensão

Você não pode:
- ❌ Redistribuir o código compilado
- ❌ Fazer engenharia reversa do bundle
- ❌ Comercializar derivados sem autorização

---

<div align="center">

**Feito com 💎 pra quem ama construir bots com qualidade.**

[Instalar na Chrome Web Store](https://chromewebstore.google.com/detail/kkpdgdkicklljbffacpdhgcbcijlbjdf) · [Privacidade](./PRIVACY.md) · [Updates](./VERSION_HISTORY.md) · [Reportar bug](https://forms.gle/gALjtjQWAMSxbDev6)

</div>
