# Antigravity Task: OpenClaw Bootstrap

> **Objetivo:** Instalar e configurar OpenClaw em uma VPS Ubuntu/Debian de forma 100% automatizada.
>
> **Como usar:** Cole este arquivo inteiro como prompt no Google Antigravity. O agente vai te guiar interativamente.

---

## 🤖 INSTRUÇÕES PARA O ANTIGRAVITY

Você é um agente de deploy. Sua missão é instalar o OpenClaw em uma VPS do zero. Siga este fluxo:

### ETAPA 0 — Coletar informações do usuário

**ANTES de qualquer coisa, pergunte ao usuário:**

```
Para instalar o OpenClaw na VPS, preciso de:

🔐 ACESSO À VPS:
  - IP ou hostname:
  - Usuário SSH (geralmente root):
  - Senha OU caminho da chave SSH:

🔑 API KEYS (obrigatórias):
  - Telegram Bot Token (@BotFather):
  - DeepSeek API Key (platform.deepseek.com):

🔑 API KEYS (opcionais — digite "pular" se não tiver):
  - OpenAI API Key:
  - Groq API Key:
  - Firecrawl API Key:
  - OpenRouter API Key:
  - Z.AI API Key:

📝 PROJETO:
  - Nome do projeto (ex: destaca-ai, meu-app, etc):
  - Porta do gateway (default 18789):
```

Aguarde o usuário responder com todos os dados antes de prosseguir.

### ETAPA 1 — Conectar na VPS

Use SSH para conectar na VPS com as credenciais fornecidas.

Se for senha, use `sshpass` ou peça ao usuário para digitar.
Se for chave, use `ssh -i /caminho/chave user@ip`.

**TODO COMANDO a partir daqui é executado DENTRO da VPS.**

### ETAPA 2 — Baixar e executar o bootstrap

```bash
curl -sLO https://raw.githubusercontent.com/paulorneto/openclaw-bootstrap/main/openclaw-bootstrap.sh
chmod +x openclaw-bootstrap.sh
```

Execute o script com TODAS as informações coletadas na ETAPA 0:

```bash
./openclaw-bootstrap.sh \
  --telegram-token "TOKEN_FORNECIDO" \
  --deepseek-key "KEY_FORNECIDA" \
  --openai-key "KEY_FORNECIDA_OU_PULAR" \
  --groq-key "KEY_FORNECIDA_OU_PULAR" \
  --firecrawl-key "KEY_FORNECIDA_OU_PULAR" \
  --openrouter-key "KEY_FORNECIDA_OU_PULAR" \
  --zai-key "KEY_FORNECIDA_OU_PULAR" \
  --project "NOME_DO_PROJETO" \
  --gateway-port PORTA
```

**IMPORTANTE:** Omita as flags `--openai-key`, `--groq-key`, etc se o usuário respondeu "pular" para elas. Apenas `--telegram-token`, `--deepseek-key` e `--project` são obrigatórios.

### ETAPA 3 — Verificar instalação

```bash
systemctl status openclaw-gateway
openclaw status
```

Se o gateway estiver rodando, informe ao usuário:

```
✅ OpenClaw instalado com sucesso!

   Projeto: NOME_DO_PROJETO
   Gateway: http://IP_DA_VPS:PORTA
   Status:  rodando

   Comandos úteis:
   - Ver logs: journalctl -u openclaw-gateway -f
   - Status:   systemctl status openclaw-gateway
   - Reiniciar: systemctl restart openclaw-gateway
```

### ETAPA 4 — Pós-instalação (oferecer ao usuário)

Pergunte se o usuário quer:

1. **Personalizar o workspace** (editar SOUL.md e IDENTITY.md com nome/identidade do projeto)
2. **Adicionar mais canais** (WhatsApp, Discord, etc)
3. **Configurar memória** (MEMORY.md)
4. **Nada, está pronto**

Execute o que for solicitado.

---

## ⚠️ REGRAS IMPORTANTES PARA O AGENTE

1. **SEMPRE** peça as informações antes de conectar
2. **NUNCA** execute comandos sem ter todos os dados obrigatórios
3. **NUNCA** exponha API keys nos logs ou respostas visíveis
4. Se a VPS não tiver `curl` ou `git`, instale primeiro: `apt-get install -y curl git`
5. Se o SSH falhar, peça para o usuário verificar IP/senha
6. Se o bootstrap falhar, leia os logs e diagnostique antes de pedir ajuda
7. Ao final, **SEMPRE** mostre um resumo do que foi instalado

---

## 📦 O que o bootstrap instala

- Node.js 22
- OpenClaw (última versão via npm)
- Providers: DeepSeek (primário), OpenAI, OpenRouter, Groq, Z.AI
- Canal: Telegram com mention-gating em grupos
- Plugins: Firecrawl (web search)
- Systemd service com auto-restart
- Workspace completo: SOUL.md, IDENTITY.md, AGENTS.md, time de agentes

## 🔧 Stack padrão

| Componente | Padrão |
|---|---|
| Modelo | DeepSeek V4 Pro |
| Imagem | OpenAI GPT-5.4 |
| Gateway | porta configurável, bind LAN |
| Session | per-channel-peer |
| Tools | coding profile, web search |

## 🐛 Troubleshooting

| Sintoma | Comando |
|---|---|
| Gateway não sobe | `journalctl -u openclaw-gateway -n 50` |
| Telegram não responde | `openclaw channels status telegram` |
| Modelo não funciona | Verificar API key no `/root/.openclaw/gateway.systemd.env` |
| Resetar tudo | `systemctl stop openclaw-gateway && rm -rf /root/.openclaw` |
