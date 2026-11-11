# Aigine Redact Plugin (Claude, Cursor, Google Antigravity & Autonomous Agents)

Sanitize and mask personal data (**GDPR**) and confidential information (**trade secrets & statutory secrecy / OSL 2009:400**) in documents (PDF, Word `.docx`, PowerPoint `.pptx`, and images up to 50 MB) using specialized descriptive encoder AI in the EU — **with zero installation, no API keys required, and strict zero retention**.

- **Verified Benchmark vs. Human-Annotated Ground Truth (GDPR + OSL):**
  - **Precision:** `98.30%`
  - **Recall:** `93.17%`
  - **F1-Score:** `95.67%`
- **GPU Performance:** `10.8s` median latency (P50) · `85.2%` of documents completed in `≤ 30s` · `~0.89s/page` on 100+ page statutes
- **Remote MCP Endpoint (Streamable HTTP):** `https://redact.aigine.se/mcp`
- **Skill Discovery URL:** `https://redact.aigine.se/SKILL.md`
- **Privacy Policy & Cloud Terms:** `https://aigine.se/cloud-terms`

---

## Configuration

### 1. Claude.ai & Claude Desktop
1. Open **Settings → Connectors → Add Custom Connector**.
2. Set **Name** to `Aigine Redact` and **Remote MCP Server URL** to `https://redact.aigine.se/mcp`.

### 2. Claude Code Plugin
This repository includes `.claude-plugin/plugin.json`, `.mcp.json`, and `skills/aigine-redact/SKILL.md` for direct use with Claude Code.

### 3. Cursor Plugin
This repository includes `.cursor-plugin/plugin.json`, `mcp.json`, and `skills/aigine-redact/SKILL.md` for use as a Cursor plugin. To try it locally, clone the repository into `~/.cursor/plugins/local/aigine-redact` and run **Developer: Reload Window**. The MCP server and the skill load automatically.

Usage: ask the agent to redact or mask a document, for example *"Redact personal data in contract.pdf before we send it to the model"*. The agent uploads the file to the EU server, calls `propose_masking`, and saves the masked PDF to your project. No configuration or API key is needed for the free tier (watermarked PDF, 100 documents/day).

### 4. Standard MCP Configuration (`mcp.json` / `.mcp.json` / `mcp_config.json`)
For Cursor, Windsurf, VS Code, Google Antigravity, or custom MCP clients:
```json
{
  "mcpServers": {
    "aigine-redact": {
      "url": "https://redact.aigine.se/mcp"
    }
  }
}
```
