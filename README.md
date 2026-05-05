# 🦞 OpenClaw Bootstrap — Destaca AI

Instala e configura OpenClaw em qualquer VPS Ubuntu/Debian em ~5 minutos.

## Stack

- **Modelo primário:** DeepSeek V4 Pro
- **Canal:** Telegram
- **Workspace:** SOUL.md + IDENTITY.md + AGENTS.md + time de agentes
- **Gateway:** systemd auto-start

## Uso rápido

```bash
curl -sLO https://raw.githubusercontent.com/destaca-ai/openclaw-bootstrap/main/openclaw-bootstrap.sh
chmod +x openclaw-bootstrap.sh
./openclaw-bootstrap.sh \
  --telegram-token "SEU_TOKEN" \
  --deepseek-key "sk-..."
```

## Com Antigravity

Abra `ANTIGRAVITY_TASK.md` como prompt no Google Antigravity e siga os passos.

## O que instala

- Node.js 22 + dependências
- OpenClaw (latest via npm)
- Providers: DeepSeek, OpenAI, OpenRouter, Groq, Z.AI
- Canal: Telegram com mention-gating em grupos
- Plugins: Firecrawl para web search
- Systemd service com auto-restart
- Workspace completo: SOUL.md, IDENTITY.md, AGENTS.md, time de agentes

## Requisitos

- Ubuntu 22.04+ ou Debian 12+
- Token do bot Telegram (@BotFather)
- API Key DeepSeek (platform.deepseek.com)
- (Opcional) API Keys OpenAI, Groq, Firecrawl, OpenRouter, Z.AI
