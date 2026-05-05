# Antigravity Task: OpenClaw Bootstrap — Destaca AI

> **Objetivo:** Instalar e configurar OpenClaw em uma VPS nova em ~5 minutos.
>
> **Formato:** Dê este arquivo como prompt para o Antigravity. Ele vai executar cada passo no terminal.

---

## 📋 Pré-requisitos (antes de começar)

Colete estas 2 keys obrigatórias + as opcionais:

| Key | Obrigatória? | Onde obter |
|---|---|---|
| `TELEGRAM_BOT_TOKEN` | ✅ Sim | @BotFather no Telegram |
| `DEEPSEEK_API_KEY` | ✅ Sim | platform.deepseek.com → API Keys |
| `GROQ_API_KEY` | Opcional | console.groq.com → API Keys |
| `OPENAI_API_KEY` | Opcional | platform.openai.com → API Keys |
| `FIRECRAWL_API_KEY` | Opcional | firecrawl.dev → API Keys |
| `OPENROUTER_API_KEY` | Opcional | openrouter.ai → API Keys |
| `ZAI_API_KEY` | Opcional | z.ai → API Keys |

---

## 🚀 Execução (passo a passo)

### Passo 1 — Conectar na VPS

```bash
ssh root@<IP_DA_VPS>
```

### Passo 2 — Baixar o script de bootstrap

```bash
curl -sLO https://raw.githubusercontent.com/destaca-ai/openclaw-bootstrap/main/openclaw-bootstrap.sh
chmod +x openclaw-bootstrap.sh
```

> ⚠️ Se o repositório ainda não existir, copie o script manualmente ou hospede-o em qualquer URL acessível.

### Passo 3 — Executar

```bash
./openclaw-bootstrap.sh \
  --telegram-token "SEU_TOKEN_TELEGRAM" \
  --deepseek-key "sk-..." \
  --openai-key "sk-..." \
  --groq-key "gsk_..." \
  --firecrawl-key "fc-..." \
  --project "nome-do-projeto"
```

Keys opcionais podem ser omitidas. O script instala, configura e já deixa rodando.

### Passo 4 — Verificar

```bash
systemctl status openclaw-gateway
openclaw status
```

### Passo 5 — Vincular Telegram

No terminal da VPS:

```bash
openclaw channels login --channel telegram
```

Opcional: configurar webhook do Telegram se não usar polling:

```bash
openclaw channels setup --channel telegram
```

### Passo 6 — Ajustar workspace

Personalize `SOUL.md` e `IDENTITY.md` para o projeto específico:

```bash
nano /root/.openclaw/workspace/IDENTITY.md
nano /root/.openclaw/workspace/SOUL.md
systemctl restart openclaw-gateway
```

---

## 🔧 Configuração que o script aplica

| Componente | Config |
|---|---|
| **Modelo primário** | DeepSeek V4 Pro |
| **Modelo imagem** | OpenAI GPT-5.4 |
| **Canal** | Telegram (mention-gated em grupos) |
| **Gateway** | Porta 18789, bind LAN, auth token |
| **Session** | per-channel-peer DM scope |
| **Tools** | Perfil coding, web search Firecrawl |
| **Plugins** | OpenAI, OpenRouter, DeepSeek, Groq, Z.AI, Firecrawl |
| **Systemd** | Auto-start, restart on failure, env vars isoladas |
| **Workspace** | SOUL.md, IDENTITY.md, AGENTS.md, HEARTBEAT.md, time de agentes |

---

## 📦 Estrutura final da VPS

```
/root/.openclaw/
├── openclaw.json          # Config principal
├── gateway.systemd.env    # API keys (isoladas)
├── credentials/           # Secrets gerenciados pelo OpenClaw
└── workspace/
    ├── SOUL.md
    ├── IDENTITY.md
    ├── AGENTS.md
    ├── HEARTBEAT.md
    ├── TOOLS.md
    ├── USER.md
    ├── memory/            # Memória diária do agente
    ├── team_agents/       # Fichas do time (Heitor, Clara, Nina, Max)
    ├── skills/            # Skills customizadas
    └── scripts/           # Scripts utilitários
```

---

## 🐛 Troubleshooting rápido

| Sintoma | Comando |
|---|---|
| Gateway não sobe | `journalctl -u openclaw-gateway -n 50` |
| Telegram não responde | `openclaw channels status telegram` |
| Modelo não funciona | Verificar `DEEPSEEK_API_KEY` no `gateway.systemd.env` |
| Resetar tudo | `systemctl stop openclaw-gateway && rm -rf /root/.openclaw && reboot` |
