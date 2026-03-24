# 🔁 Automação Bitrix24 → Node-RED → Microsoft Teams

![Node-RED](https://img.shields.io/badge/Node--RED-8F0000?style=for-the-badge&logo=nodered&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Bitrix24](https://img.shields.io/badge/Bitrix24-FF4B4B?style=for-the-badge&logo=bitrix24&logoColor=white)
![Microsoft Teams](https://img.shields.io/badge/Microsoft_Teams-6264A7?style=for-the-badge&logo=microsoft-teams&logoColor=white)

> Automação empresarial que monitora pipelines do Bitrix24 em tempo real e dispara alertas automáticos no Microsoft Teams — incluindo relatório diário de cards atrasados.

---

## 💡 Sobre o Projeto

Este projeto resolve dois problemas comuns em equipes de vendas e CRM:

1. **Atualização em tempo real** — sempre que um card é atualizado na pipeline do Bitrix24, a equipe recebe uma notificação instantânea no Teams.
2. **Relatório de atrasos** — de segunda a sexta às 16h (horário de Brasília), um gatilho automático varre a pipeline e envia uma lista com todos os cards atrasados, baseado no campo de hora do card.

Tudo isso sem precisar de um servidor dedicado — roda diretamente no Node-RED com fluxos importáveis.

---

## 🛠️ Stack de Tecnologias

| Ferramenta | Função |
|---|---|
| **Node-RED** | Orquestração dos fluxos de automação |
| **JavaScript** | Lógica de transformação dos dados |
| **Bitrix24 Webhook** | Trigger de eventos e consulta à API CRM |
| **Microsoft Teams Webhook** | Envio das notificações |
| **Visual Studio Code** | Edição e customização dos fluxos |

---

## 📐 Arquitetura da Solução

```
┌─────────────────────────────────────────────────────────┐
│                     FLUXO 1 — Tempo Real                │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  [Bitrix24]                                             │
│  Card atualizado na pipeline                            │
│       │                                                 │
│       ▼  Webhook de entrada (permissão CRM)             │
│  [Node-RED]                                             │
│  Recebe evento → extrai campos → formata payload        │
│       │                                                 │
│       ▼  HTTP POST                                      │
│  [Microsoft Teams]                                      │
│  Notificação entregue ao canal ✅                       │
│                                                         │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│              FLUXO 2 — Relatório Diário de Atrasos      │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  [Node-RED — Trigger Agendado]                          │
│  Seg–Sex às 16h (horário de Brasília)                   │
│       │                                                 │
│       ▼  Consulta via Webhook Bitrix24                  │
│  Lista todos os cards da pipeline                       │
│       │                                                 │
│       ▼  Filtro por campo de hora                       │
│  Seleciona apenas os cards atrasados                    │
│       │                                                 │
│       ▼  HTTP POST                                      │
│  [Microsoft Teams]                                      │
│  Relatório de atrasos enviado ao canal ✅               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## ⚠️ Importante: Campos Dinâmicos por Empresa

> Cada empresa tem campos personalizados no Bitrix24 com identificadores únicos no formato `UF_CRM_*` ou `C6: UC_*`. **Esses campos variam de empresa para empresa.**

Para adaptar o fluxo à sua empresa:

1. Importe o `flows.json` no Node-RED
2. Ative o **nó de debug** no fluxo
3. Acione um evento no Bitrix24 e observe o payload completo no painel de debug
4. Identifique os campos necessários (ex: campo de data, responsável, link do card)
5. Substitua nos nós de `function` do fluxo

O campo `payload.result` contém os links dos cards — adapte conforme a necessidade da sua empresa.

---

## 🚀 Como Usar

### Pré-requisitos

- [Node-RED](https://nodered.org/) instalado localmente ou em servidor
- Conta no [Bitrix24](https://www.bitrix24.com.br/) com acesso de administrador
- Webhook de entrada configurado no Microsoft Teams

### Passo 1 — Configure o Webhook no Bitrix24

1. Acesse **Bitrix24 → Desenvolvedor → Webhooks de entrada**
2. Crie um novo webhook com permissão de **CRM** (permite listar todos os negócios)
3. Copie a URL gerada

### Passo 2 — Configure o Webhook no Microsoft Teams

1. No Teams, acesse o canal desejado → **"..." → Conectores**
2. Adicione um **Incoming Webhook** e copie a URL

### Passo 3 — Importe o fluxo no Node-RED

```bash
# Opção 1: Via interface do Node-RED
Menu (☰) → Import → Selecione o arquivo flows.json

# Opção 2: Via VS Code (recomendado para edições)
code flows.json
```

### Passo 4 — Configure as variáveis no fluxo

Nos nós de `function`, substitua:

```javascript
// URL do webhook do Teams
const teamsWebhookUrl = "SUA_URL_TEAMS_AQUI";

// URL do Bitrix24
const bitrixWebhookUrl = "SUA_URL_BITRIX_AQUI";
```

### Passo 5 — Deploy e teste

1. Clique em **Deploy** no Node-RED
2. Atualize um card no Bitrix24
3. Verifique se a notificação chegou no Teams ✅

---

## 📂 Estrutura do Repositório

```
Node-Red-Automacao-Bitrix24/
├── flows.json      # Fluxo completo exportado do Node-RED (importável)
└── README.md       # Este arquivo
```

---

## 💡 Dicas de Customização

- **Filtrar por etapa específica da pipeline** — use um nó `switch` para verificar o campo `STAGE_ID`
- **Mudar o horário do relatório** — no nó `inject` agendado, ajuste a expressão cron
- **Formatar a mensagem do Teams** — o Teams aceita [Adaptive Cards](https://adaptivecards.io/) para mensagens mais ricas
- **Adicionar múltiplos canais** — duplique o nó de HTTP Request e aponte para outro webhook

---

## 🤝 Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para abrir uma _issue_ com sugestões ou um _pull request_ com melhorias.

---

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

---

## 👤 Autor

**João Lucas Souza**
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/jlucas-souza/)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=flat&logo=github&logoColor=white)](https://github.com/joaosouzas)
