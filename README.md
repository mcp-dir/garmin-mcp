# Garmin Connect (treinos)

### Garmin Connect (treinos) para Claude, Cursor e agentes de IA

Listar, obter, criar, editar (substituir), apagar e agendar treinos estruturados no Garmin Connect (corrida, ciclismo, natação, multi-sport / triatlo). Usa a API não oficial via sessão Garmin (email + senha); tokens OAuth são guardados encriptados para reduzir logins.

- 📊 **0 ferramentas**
- 🔒 **Somente leitura**
- 💬 **Funciona com qualquer cliente MCP**: Claude Desktop, Cursor, VS Code, Cline, Continue
- 🔑 **Login via magic-link (sem senha)**

[English version](README.en.md) · [Documentação completa](docs/) · [Skill pra agentes](skills/)

---

## Instalar em 1 clique

### Claude (Web e Desktop)

A Anthropic unificou a instalação de MCPs em `claude.ai/customize/connectors`. **O mesmo link serve pra Claude Web e Claude Desktop** (basta estar logado):

[➕ Abrir no Claude e conectar](https://claude.ai/customize/connectors?modal=add-custom-connector&mcpName=Garmin%20Connect%20(treinos)&mcpServerUrl=https%3A%2F%2Fapi.mcp.ai%2Fp_garmin)

**Manual** (se o deeplink não abrir): [claude.ai/customize/connectors](https://claude.ai/customize/connectors) → **+** → **Adicionar conector personalizado** → cole **Nome** `Garmin Connect (treinos)` e **URL** `https://api.mcp.ai/p_garmin`.

### Cursor

[➕ Instalar Garmin Connect (treinos) no Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=garmin&config=eyJ1cmwiOiJodHRwczovL2FwaS5tY3AuYWkvcF9nYXJtaW4ifQ==)

### VS Code (Copilot Chat)

[➕ Instalar Garmin Connect (treinos) no VS Code](vscode:mcp/install?name=garmin&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fapi.mcp.ai%2Fp_garmin%22%7D)

### ChatGPT, Manus, OpenClaw e mais 40+ clientes

Funciona em qualquer cliente MCP que suporte **MCP over HTTP**. A URL do servidor é sempre:

```
https://api.mcp.ai/p_garmin
```

Detalhes por cliente: [INSTALL.md](INSTALL.md).

---

## Exemplos de uso

```
Crie um treino de corrida intervalado e agende pra amanhã
Liste meus treinos da próxima semana
Edite o treino de bike de quinta pra um Z2 longo
```

---

## 0 ferramentas disponíveis

| Tool | Descrição |
|---|---|


Detalhe de cada tool: [docs/ferramentas.md](docs/ferramentas.md)

---

## Preços

Grátis.

---

## Privacidade & LGPD

- **Somente leitura**, nenhuma ferramenta altera dados na origem.
- **Sub-processadores**: o LLM host que você escolher (Claude, ChatGPT, Cursor, agente próprio). Lista completa em [docs/privacidade-lgpd.md](docs/privacidade-lgpd.md).
- Os dados retornados pelas tools são enviados ao **LLM host que você escolher**, sub-processador fora do nosso controle. Recomendamos planos com opt-out de treinamento.

---

## Perguntas frequentes

**O servidor é open source?**
O servidor é proprietário (hosted). Este repositório é o wrapper público com manifestos, docs e skills — tudo MIT.

**Posso usar com agente próprio (não Claude/Cursor)?**
Sim — qualquer cliente que suporte MCP over HTTP. URL: `https://api.mcp.ai/p_garmin`.


---

## Suporte

- 📧 [garmin@mcp.ai](mailto:garmin@mcp.ai)
- 🐛 [GitHub Issues](https://github.com/mcp-dir/garmin-mcp/issues)
- 📄 [docs/](docs/)

---

## Licença

MIT — veja [LICENSE](LICENSE). O servidor MCP em `api.mcp.ai/p_garmin` é proprietário (hosted); este repositório (manifestos, docs, skills) é MIT.
