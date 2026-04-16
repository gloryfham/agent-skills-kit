# Agent Skills

Claude Code **Agent Skills** for Glory wealth workflows: structured instructions and tool references for product catalog queries.

This repository is a **[Claude Code plugin](https://code.claude.com/docs/en/plugins)** (manifest under [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json), skills in [`skills/`](skills/)).

## Layout

- [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) — Claude Code plugin identity (`name`, `version`, …). Plugin id `glory-agent-kit` namespaces skills.
- [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) — catalog for team distribution via GitHub marketplace.

Each skill lives under `skills/<skill-id>/`:

- `SKILL.md` — front matter (`name`, `description`) plus the full workflow the agent must follow.
- `references/` — supporting material (MCP tool documentation).

## Skills in this repo

| Skill | Purpose |
|--------|---------|
| [glory-product-catalog](skills/glory-product-catalog/SKILL.md) | 保险产品与签证项目查询，通过 MCP Server 提供实时产品数据。 |

## Using these skills

### Claude Code (plugin)

1. **Local checkout (development)**:

   ```bash
   claude --plugin-dir /path/to/agent-skills
   ```

   Then run `/reload-plugins` if you change files.

2. **Install via marketplace (team or GitHub)**:

   ```text
   /plugin marketplace add gloryfham/agent-skills
   /plugin install glory-agent-kit@glory-agent-kit
   /reload-plugins
   ```

3. **Install from GitHub directly**:

   ```bash
   npx skills add gloryfham/agent-skills
   ```

## MCP Server

This skill requires a running MCP Server `glory-product-catalog`. The server is integrated into the `business-hk-web` Spring Boot application:

- **Endpoint**: `http://localhost:8076/sse`
- **Source**: `glory-business-hk` project, `business-hk-service/.../mcp/ProductMcpTool.java`
- **Data**: JSON files under `business-hk-web/src/main/resources/mcp/`
