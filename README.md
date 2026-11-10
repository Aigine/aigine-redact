# Aigine Redact Plugin (Claude, Google Antigravity, Cursor & Autonomous Agents)

Sanitize and mask personal data (**GDPR**) and confidential information (**trade secrets & statutory secrecy / OSL 2009:400**) in documents (PDF, Word `.docx`, PowerPoint `.pptx`, and images up to 50 MB) using specialized descriptive encoder AI in the EU — **with zero installation, no API keys required, and strict zero retention**.

- **Verified Benchmark vs. Human-Annotated Ground Truth (GDPR + OSL):**
  - **Precision:** `98.30%`
  - **Recall:** `93.17%`
  - **F1-Score:** `95.67%`
- **GPU Performance:** `10.8s` median latency (P50) · `85.2%` of documents completed in `≤ 30s` · `~0.89s/page` on 100+ page statutes
- **Remote MCP Endpoint (Streamable HTTP):** `https://redact.aigine.se/mcp`
- **Skill Discovery URL:** `https://redact.aigine.se/SKILL.md`

---

## Quick Connect

### 1. Claude.ai & Claude Desktop
1. Open **Settings → Connectors → Add Custom Connector**.
2. Set **Name** to `Aigine Redact` and **Remote MCP Server URL** to `https://redact.aigine.se/mcp`.

### 2. Claude Code (CLI)
```bash
claude mcp add --transport http aigine-redact https://redact.aigine.se/mcp
```
Or clone and load this full plugin bundle (including the `aigine-redact` Skill):
```bash
git clone https://github.com/Aigine/aigine-redact.git
claude --plugin-dir ./aigine-redact
```

### 3. Google Antigravity
Clone directly into your Antigravity plugins folder:
```bash
mkdir -p ~/.gemini/config/plugins
git clone https://github.com/Aigine/aigine-redact.git ~/.gemini/config/plugins/aigine-redact
```
Or add `"url": "https://redact.aigine.se/mcp"` to `~/.gemini/config/mcp_config.json`.

### 4. Cursor / Windsurf / VS Code
Add to `.cursor/mcp.json` or `mcp.json`:
```json
{
  "mcpServers": {
    "aigine-redact": {
      "url": "https://redact.aigine.se/mcp"
    }
  }
}
```
