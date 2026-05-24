# Política de Privacidade — Holocron

**Última atualização:** 23 de maio de 2026

## TL;DR

- ✅ A Extensão **não coleta dados pessoais** (PII).
- ✅ **Não envia telemetria** para servidores próprios — não há servidor.
- ✅ **Não compartilha dados com terceiros** (sem Google Analytics, Mixpanel, Sentry, OpenAI, etc.).
- ✅ Funciona **apenas dentro de `*.blip.ai`** — não enxerga outros sites que você abre.
- ✅ Todos os dados ficam no **`localStorage` do seu navegador** e podem ser apagados a qualquer momento.
- ℹ️ **Um único evento de auditoria interna** é registrado via a API nativa do Blip quando o usuário exclui um fluxo (detalhes mais abaixo).

---

## 🔍 Quais dados a extensão acessa?

O **Holocron** foi projetado para funcionar **sem coletar Dados Pessoais de Identificação (PII)**. Os dados acessados são apenas a estrutura do fluxo do bot e configurações locais necessárias para que os recursos funcionem.

### Snapshot do Fluxo (para Diff e Histórico)

A Extensão extrai um *snapshot semântico* do fluxo do bot atualmente aberto no Blip Builder — contendo títulos de blocos, mensagens, ações customizadas (JavaScript, SetVariable, HTTP), condições de saída, ações globais e variáveis do fluxo.

Esse snapshot é usado para:

- Calcular o **diff** antes da publicação
- Manter o **histórico de versões** local do fluxo
- Permitir **import de fluxo entre bots** (operação manual disparada pelo usuário)

**Nunca são lidos ou armazenados:** credenciais, tokens de autenticação, cookies, dados de contato de usuários finais do bot, ou histórico de conversas reais.

### Lint, Find/Replace e Zonas no Canvas

- **Lint** analisa o fluxo em memória para apontar problemas; nenhum dado sai do navegador.
- **Find/Replace** opera apenas sobre o conteúdo do fluxo carregado localmente.
- **Zonas no canvas** (anotações visuais que agrupam blocos) ficam no `localStorage`.

### Snippets

Snippets criados pelo usuário (trechos de código JS, mensagens reutilizáveis, etc.) ficam **somente no `localStorage`** do navegador. Não são sincronizados com servidor nenhum.

### Histórico / Integração Git

A funcionalidade de **histórico** armazena versões anteriores do fluxo localmente, no `localStorage`.

A integração com **Git** gera diffs em formato **Markdown** e **Mermaid** que o usuário pode **copiar manualmente** para o próprio repositório (GitHub, GitLab, Bitbucket, etc.). **A Extensão não se conecta a nenhum repositório Git remoto** — todo push/commit é feito manualmente pelo usuário fora da extensão.

### AI Agents Viewer

O visualizador de AI Agents lê os agentes **já existentes no Blip Builder do bot atual** (via DOM/estado do React) e exibe na UI da Extensão. **Nenhuma chamada para APIs de IA externas** (OpenAI, Anthropic, Gemini, etc.) é feita pela Extensão.

### Conteúdo do Site

Elementos da interface do Blip Builder (DOM do AngularJS, controller do builder, estado interno) são acessados localmente para:

- Interceptar o botão **Publicar**
- Injetar botões e overlays da Extensão (Diff 🆚, Lint, Snippets, etc.)
- Renderizar modais, painéis e visualizações

Esses acessos são manipulados inteiramente no navegador.

### Atividade do Usuário

Cliques nos botões da Extensão são processados localmente para acionar funcionalidades. **Nenhuma telemetria de uso é enviada para qualquer servidor** (com a única exceção descrita em "Audit interno" mais abaixo).

---

## 🔒 Permissões utilizadas

A extensão solicita **apenas duas permissões** no `manifest.json`:

| Permissão | Por que é necessária |
|-----------|----------------------|
| `storage` | Reservada para uso futuro de `chrome.storage.local`. A versão atual usa `localStorage` do navegador (escopado ao domínio `blip.ai`), que não exige essa permissão; está declarada por compatibilidade. |
| `host_permissions: https://*.blip.ai/*` | Necessária para que o content script execute **apenas** dentro do domínio da plataforma Blip. A Extensão não acessa nenhum outro site. |

Nenhuma dessas permissões é usada para coletar ou acessar dados pessoais.

---

## 💽 Armazenamento e Localização dos Dados

Tudo fica **no seu navegador**. A Extensão **não possui servidores próprios** para coleta, processamento ou armazenamento.

| Tipo de dado | Onde mora | Identificável? |
|---|---|---|
| Snapshot do fluxo (diff) | `localStorage` — chave `bbd:snapshot:v2:<flowId>` | ❌ não |
| Histórico de versões | `localStorage` — prefixo `bbd:` / `holocron:` | ❌ não |
| Snippets do usuário | `localStorage` | ❌ não |
| Zonas no canvas | `localStorage` | ❌ não |
| Preferências (idioma, etc.) | `localStorage` / `chrome.storage` | ❌ não |

- Cada bot tem seu próprio snapshot, identificado pelo `flowId` do builder.
- O snapshot contém apenas a estrutura semântica do fluxo, **sem dados pessoais seus ou de usuários finais do bot**.
- A chamada de publicação efetiva é feita **pela própria plataforma Blip** (não pela extensão), usando a API original de publicação do builder.

---

## 📤 Compartilhamento de dados

A Extensão **NÃO compartilha dados com terceiros externos**.

- ❌ **Nenhuma chamada de rede própria** é feita pela Extensão para servidores fora da plataforma Blip.
- ❌ **Nenhum serviço de analytics terceiro** (Google Analytics, Mixpanel, Sentry, Amplitude, etc.) é utilizado.
- ❌ **Nenhuma API de IA externa** (OpenAI, Anthropic, Gemini, etc.) é chamada.
- ❌ **Nenhum repositório Git remoto** é acessado automaticamente — integração Git é manual (copy/paste de diff).
- ✅ A Extensão opera **estritamente dentro do domínio `*.blip.ai`**, declarado em `host_permissions`.

### Audit interno (registro de exclusão de fluxo)

Para fins de **auditoria interna na plataforma Blip**, a Extensão registra **um único evento**: quando o usuário clica no botão "Excluir fluxo" na página de configurações de um bot. O evento é enviado **através da própria API nativa de tracking de eventos de bot do Blip** (`SegmentService.createBotTrack`) — a mesma função que o próprio Blip Builder usa internamente para registrar eventos vinculados a um bot.

O payload é **mínimo**:

| Campo | Valor |
|-------|-------|
| `user_identity` | Identidade Blip do usuário (formato `email@blip.ai`) |
| `bot` | `shortName` do bot |
| `timestamp` | Data/hora ISO 8601 |

**Não são enviados:** email pessoal, nome completo, IP, user-agent, URL da página, conteúdo/estrutura do fluxo, snapshots, snippets, configurações ou qualquer outro dado.

Como o envio é feito via a API nativa do Blip, os dados ficam sujeitos à **política de privacidade da plataforma Blip** e visíveis nos canais de analytics que o Blip já mantém. **A publicação do fluxo (`Publicar`) NÃO gera evento de auditoria** — apenas a exclusão.

---

## 🌐 Internacionalização

A Extensão suporta **Português (Brasil)**, **Português (Portugal)**, **English** e **Español**. A preferência de idioma é salva localmente no navegador e nunca compartilhada.

---

## ❌ Seus direitos de privacidade (exclusão de dados)

Você pode excluir todos os dados armazenados pela Extensão a qualquer momento:

- **Desinstalar a Extensão:** os dados continuam no `localStorage` associado ao domínio `blip.ai`. Para removê-los completamente, use uma das opções abaixo.
- **DevTools:** abra DevTools em uma aba `*.blip.ai` → *Application* → *Storage* → *Clear site data*.
- **Comando manual:** no Console do navegador em uma aba do Blip:

  ```js
  Object.keys(localStorage)
    .filter(k => k.startsWith('bbd:') || k.startsWith('holocron:'))
    .forEach(k => localStorage.removeItem(k));
  ```

---

## 🔓 Transparência e código

Esta extensão foi desenvolvida com foco em **privacidade, transparência e segurança**, em conformidade com as diretrizes da Chrome Web Store e melhores práticas de desenvolvimento de extensões.

O código-fonte é entregue **minificado** (via Terser) por motivos de tamanho de bundle e proteção de propriedade intelectual. **A Extensão NÃO utiliza obfuscação de código**, em conformidade com a [Política de Legibilidade de Código](https://developer.chrome.com/docs/webstore/program-policies/code-readability) do Chrome Web Store.

Todas as operações descritas nesta política podem ser **verificadas independentemente**:

- Inspecionando o `dist/content.js` carregado pelo navegador (minificado mas legível)
- Monitorando o tráfego de rede da aba (que mostrará apenas chamadas para `*.blip.ai` originadas pelo próprio Blip Builder)
- Inspecionando o `localStorage` via DevTools

---

## 📬 Contato

Para dúvidas sobre privacidade, solicitações de exclusão de dados ou reporte de problemas de segurança:

**Adrian Garcia** — desenvolvedor responsável
✉️ holocrongiindev@duck.com

Ou pelo formulário oficial de feedback da extensão:
🔗 https://forms.gle/gALjtjQWAMSxbDev6
