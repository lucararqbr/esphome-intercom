# VoIP Stack (voip_stack) — versão modificada PT-BR

Integração customizada do Home Assistant que fornece PBX/SIP, softphone no
navegador e card Lovelace. Este repositório guarda a versão original e as
minhas modificações locais.

## Versão base

- **Integração:** `voip_stack` **v2026.11.0** (`manifest.json` → `"version": "2026.11.0"`)
- **Repositório original:** https://github.com/n-IA-hane/esphome-intercom
  - A integração fica em `custom_components/voip_stack` desse repositório.
  - Base exata: tag [`v2026.11.0`](https://github.com/n-IA-hane/esphome-intercom/tree/v2026.11.0/custom_components/voip_stack) (commit `270cbb9`).
- **Fork com as modificações:** https://github.com/lucararqbr/esphome-intercom (branch `ptbr-modificacoes`)
- **Home Assistant alvo:** 2026.10.4

## Histórico deste repositório

| Commit | Conteúdo |
| --- | --- |
| `original v2026.11.0` | Arquivos da tag oficial `v2026.11.0`, sem alterações |
| `minhas modificações` | Conteúdo de `voip_stack_ptbr_2026.11.0.zip` |

Os zips `voip_stack_ptbr_2026.11.0.zip` e `voip_stack_ptbr_2026.11.0_modificado.zip`
ficam na pasta só como backup (ignorados pelo Git). **Os dois são idênticos**
(mesmo SHA-256): ambos já contêm as modificações, por isso a base "original"
foi tirada do GitHub e não do zip.

## O que foi modificado e por quê

Objetivo geral: **completar a tradução PT-BR do card Lovelace / softphone**.
Na versão original vários textos do card apareciam em inglês mesmo com o Home
Assistant em português, porque (a) faltavam as chaves no dicionário PT-BR e
(b) alguns textos estavam fixos no código, sem passar pela função de tradução.
Nenhum arquivo Python, de configuração ou de tradução do backend foi alterado.

### 1. `frontend/voip-stack-i18n.js` — novas traduções PT-BR

Adicionadas 60 entradas ao dicionário `PT_BR`, cobrindo:

- **Estados da chamada:** `Ending` → "Encerrando", `Ringing` → "Tocando".
- **Seletor de dispositivos de mídia:** `System default` → "Padrão do sistema",
  `Browser default` → "Padrão do navegador", ` - active` → " - ativo",
  "Solicitando acesso à mídia...", "Alternando câmera...", "Câmera ativa." etc.
- **Estacionamento de chamadas (parking):** mensagens de configuração e de
  restrição a chamadas só de áudio.
- **Motivos de fim de chamada:** `Remote/Caller/Callee` × `hung up/lost/declined/error`
  (ex.: "O outro lado desligou", "O chamador recusou", "Erro do destinatário"),
  `Local decline` → "Recusa local", `Error` → "Erro".
- **Erros de conexão com o HA:** "O Home Assistant não respondeu.",
  "O Home Assistant está inacessível...", "A conexão com o Home Assistant não
  está disponível." etc.
- **Erros de navegador / mídia:** microfone, câmera, HTTPS obrigatório, Web
  Audio, WebCodecs, worker JPEG, tamanho de quadro de vídeo SIP.
- **Erros de configuração do card:** "Nenhum endpoint", "Entidade de switch
  indisponível", "A agenda telefônica VoIP exige uma entidade do tipo sensor",
  privilégios de administrador etc.

Adicionadas também regras de substituição por regex (bloco `replacements` do
PT-BR) para mensagens com parâmetros:

- `Local decline: X` → "Recusa local: X"
- `Local error (code N)` → "Erro local (código N)"; `Local error: X` → "Erro local: X"
- `(Remote|Caller|Callee) (hung up|lost|declined|error): X` → traduz o prefixo
  reaproveitando as chaves acima e mantém o detalhe X.
- `(Remote|Caller|Callee) error (code N)` → ex.: "Erro remoto (código N)".

### 2. `frontend/voip-stack-card.js` — textos fixos passam por `this._t()`

Sem `_t()`, as traduções acima nunca seriam usadas nesses pontos:

- **Barra de chamada em vídeo (`hangupState` / `hangupPeer`, ~linha 1861):** os
  estados `Ending` / `Calling` / `Answering` / `In call` e o fallback
  `Active call` agora são traduzidos.
- **Seletor de dispositivo de áudio/vídeo (~linha 3073):** `System default`,
  `Browser default` e o sufixo ` - active` agora são traduzidos.

## Instalação no Home Assistant

1. Copiar **o conteúdo deste repositório** (sem `.git`, `.gitignore`, zips e
   este `CLAUDE.md`) para `/config/custom_components/voip_stack/` no Home
   Assistant — via Samba (`Z:\custom_components\voip_stack`) ou SSH.
   O `manifest.json` deve ficar em `/config/custom_components/voip_stack/manifest.json`.
2. Reiniciar o Home Assistant.
3. Recarregar o navegador com cache limpo (Ctrl+F5) para pegar os JS novos do card.

Exemplo no Windows (PowerShell/cmd, a partir desta pasta; sem `/MIR`, para não
apagar arquivos que já existam no destino):

```powershell
robocopy . Z:\custom_components\voip_stack /E /XD .git __pycache__ /XF .gitignore *.zip CLAUDE.md
```

(ou copiar manualmente com o Explorer, sobrescrevendo os arquivos.)

## Atualizando para uma nova versão do upstream

1. Baixar a nova tag de `n-IA-hane/esphome-intercom` e substituir os arquivos
   (commit "original vX.Y.Z").
2. Reaplicar o commit "minhas modificações" (`git cherry-pick`) e resolver
   conflitos. Conferir se o upstream já incluiu alguma dessas traduções.
3. Atualizar a seção "Versão base" deste arquivo.
