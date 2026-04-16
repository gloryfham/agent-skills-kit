# Agent Skills

Claude Code **Agent Skills** for Glory global planning workflows: structured instructions and tool references for the **荣耀全球路径规划师** (Glory Global Path Planner).

This repository is a **[Claude Code plugin](https://code.claude.com/docs/en/plugins)** (manifest under [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json), skills in [`skills/`](skills/)).

## Layout

- [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) — Claude Code plugin identity (`name`, `version`, …). Plugin id `glory-agent-kit` namespaces skills.
- [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) — catalog for team distribution via GitHub marketplace.

Each skill lives under `skills/<skill-id>/`:

- `SKILL.md` — front matter (`name`, `description`) plus the full workflow the agent must follow.

## Skills in this repo

| Skill | Purpose |
|--------|---------|
| [glory-global-path-planner](skills/glory-global-path-planner/SKILL.md) | 荣耀全球路径规划师 — 联动保险与移民签证的家庭未来规划智能助手。 |

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

This skill uses the `@gloryfham/mcp-global-planner` npm package as its MCP Server:

- **npm**: https://www.npmjs.com/package/@gloryfham/mcp-global-planner
- **GitHub**: https://github.com/sergio-wen/mcp-global-planner
- **Install**: `npm install -g @gloryfham/mcp-global-planner`
- **Bin**: `gloryfham-mcp-global-planner`

### Available Tools

| Category | Tool | Purpose |
|----------|------|---------|
| Insurance | `listInsuranceProducts` | List all insurance products |
| Insurance | `searchInsuranceProducts(keyword?, productType?, region?)` | Search insurance products |
| Insurance | `getInsuranceProductDetail(productCode)` | Get product details |
| Visa | `listVisaProjectTypes` | List visa project categories |
| Visa | `listAllVisaProjects` | List all visa/immigration projects |
| Visa | `searchVisaProjects(keyword?, country?, projectType?, identityType?, minAmount?)` | Search visa projects |
| Visa | `getVisaProjectDetail(projectCode)` | Get project details |
| **Core** | **`generateFamilyPathPlan`** | **Generate family identity + insurance path recommendation** |
