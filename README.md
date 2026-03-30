<h1 align="center">
  AION Plugin Hub
</h1>

<p align="center">
  Optional plugins for <a href="https://github.com/xynstr/aion">AION</a> — install on demand, no restart needed.
</p>

<p align="center">
  <a href="https://github.com/xynstr/aion-hub-plugins/releases"><img src="https://img.shields.io/github/v/release/xynstr/aion-hub-plugins?label=latest&color=4a9eff" alt="Latest Release"></a>
  <img src="https://img.shields.io/badge/plugins-24-brightgreen" alt="24 plugins">
  <img src="https://img.shields.io/badge/hot--reload-yes-blue" alt="Hot reload">
  <a href="https://github.com/xynstr/aion"><img src="https://img.shields.io/badge/requires-AION%20v1.4%2B-orange" alt="Requires AION v1.4+"></a>
</p>

---

## How to install

Inside AION, just ask — or use the **Plugin Hub** tab in the Web UI:

```
hub_list                          → browse all available plugins
hub_install telegram_bot          → install (SHA256-verified, hot-reload, no restart)
hub_update                        → check for updates
hub_remove telegram_bot           → uninstall
```

Every install is SHA256-verified and hot-reloaded into the running AION instance — no restart needed.

---

## Plugins

### 💬 Messaging & Bots

| Plugin | Name | Description | Deps |
|--------|------|-------------|------|
| `telegram_bot` | Telegram Bot | Send and receive messages via Telegram. AION can notify you and respond to commands from any device. | `httpx` |
| `discord_bot` | Discord Bot | Connect AION to a Discord server as a bot. Supports slash commands and channel messaging. | `discord.py` |
| `slack_bot` | Slack Bot | Integrate AION into Slack workspaces. Responds to mentions and direct messages. | `slack-bolt` |
| `alexa_plugin` | Alexa Plugin | Connect AION to Amazon Alexa for voice-triggered commands and smart home integration. | — |

---

### 🤖 AI Providers

Swap AION's AI backend with a single install. Each provider registers its models in the model selector.

| Plugin | Name | Description | Deps |
|--------|------|-------------|------|
| `anthropic_provider` | Anthropic Provider | Use Claude models (claude-3-5-sonnet, claude-opus, etc.) as AION's AI backend. | `openai` |
| `gemini_provider` | Gemini Provider | Use Google Gemini models (gemini-2.0-flash, gemini-2.5-pro, etc.) as AION's AI backend. | `google-genai` |
| `deepseek_provider` | DeepSeek Provider | Use DeepSeek models (deepseek-chat, deepseek-reasoner) as AION's AI backend. | `openai`, `httpx` |
| `grok_provider` | Grok Provider | Use xAI Grok models as AION's AI backend. | `openai`, `httpx` |
| `ollama_provider` | Ollama Provider | Use local Ollama models (llama3, mistral, etc.) — fully offline, no API key required. | `openai` |
| `claude_cli_provider` | Claude CLI Provider | Route AION requests through the Claude CLI for enhanced context and tool use. | — |

---

### 🖥️ Automation & Control

| Plugin | Name | Description | Deps |
|--------|------|-------------|------|
| `desktop` | Desktop Automation | Full desktop automation — mouse control, keyboard input, screenshots, and clipboard access via pyautogui. | `pyautogui`, `Pillow`, `pyperclip` |
| `playwright_browser` | Playwright Browser | Headless browser automation. Browse, click, fill forms, and extract web content. | `playwright` |
| `multi_agent` | Multi-Agent Router | Spin up parallel sub-agents for complex multi-step tasks. AION delegates and aggregates results. | — |

---

### 🎙️ Audio & Voice

| Plugin | Name | Description | Deps |
|--------|------|-------------|------|
| `audio_pipeline` | Audio Pipeline | Full voice pipeline: text-to-speech output + speech-to-text input via local Whisper. | `faster-whisper`, `pyttsx3`, `edge-tts` |
| `audio_transcriber` | Audio Transcriber | Transcribe audio files to text using faster-whisper (local, no API key required). | `faster-whisper` |

---

### 📄 Productivity & Tools

| Plugin | Name | Description | Deps |
|--------|------|-------------|------|
| `docx_tool` | DOCX Creator | Create and edit Word documents (.docx) from AION responses or structured content. | `python-docx` |
| `image_search` | Image Search | Search the web for images and return URLs. Supports Google and Bing image search. | — |
| `mcp_client` | MCP Client | Connect to external MCP (Model Context Protocol) servers to extend AION with third-party tools. | — |
| `moltbook` | Moltbook | Integration with the Moltbook platform for content creation and social media automation. | `requests` |

---

### 🧠 AION Personality & System

| Plugin | Name | Description | Deps |
|--------|------|-------------|------|
| `mood_engine` | Mood Engine | Dynamic mood system — AION's personality adapts based on context, time, and recent interactions. | — |
| `proactive` | Proactive Memory | Proactive memory analysis — AION surfaces relevant past context without being asked. | — |
| `character_manager` | Character Manager | Manage AION's character profile — name, personality traits, and behavioral guidelines. | — |
| `focus_manager` | Focus Manager | Persistent task focus — injects the current focus goal into every AION turn for sustained attention. | — |
| `heartbeat` | Heartbeat | Periodic self-check — AION logs a heartbeat at configurable intervals to confirm it is running. | — |

---

## How it works

```
AION                         aion-hub-plugins
 │                                │
 ├─ hub_list        ──────────▶  manifest.json  (fetched on demand)
 │
 ├─ hub_install telegram_bot
 │    │
 │    ├─ 1. fetch  manifest.json  →  get download_url + expected sha256
 │    ├─ 2. download  telegram_bot-v1.0.0.zip  (from GitHub Release)
 │    ├─ 3. verify  SHA256
 │    ├─ 4. extract  →  plugins/telegram_bot/
 │    ├─ 5. pip install  httpx
 │    └─ 6. hot-reload  →  tools live immediately, no restart
 │
 └─ hub_update      ──────────▶  compare version.txt vs manifest version
```

Each installed plugin gets a `version.txt` so AION can detect future updates:

```
plugins/
└── telegram_bot/
    ├── telegram_bot.py    ← plugin code
    ├── plugin.json        ← metadata (name, description, dependencies)
    ├── README.md          ← shown in AION's system prompt
    └── version.txt        ← "1.0.0" — written by hub_install
```

---

## Contributing a plugin

1. Fork this repo and create `your_plugin/` containing:
   - `your_plugin.py` — plugin code with a `register(api)` function
   - `plugin.json` — `{ "name": "...", "description": "...", "dependencies": [...] }`
   - `README.md` — tool documentation, shown in AION's system prompt
2. Open a pull request — CI validates the plugin structure automatically.
3. On merge + tag, GitHub Actions builds the ZIPs, computes SHA256 hashes, and updates `manifest.json`.

### `plugin.json` format

```json
{
  "name": "My Plugin",
  "description": "One sentence. What AION can do with this plugin.",
  "dependencies": ["requests", "httpx"]
}
```

### `register(api)` skeleton

```python
def register(api):
    api.register_tool(
        "my_tool",
        "Description of what this tool does.",
        my_tool_handler,
        {
            "type": "object",
            "properties": {
                "param": {"type": "string", "description": "..."}
            },
            "required": ["param"]
        }
    )
```

---

<p align="center">
  Part of the <a href="https://github.com/xynstr/aion">AION</a> ecosystem &nbsp;·&nbsp;
  <a href="https://github.com/xynstr/aion/releases">AION Releases</a>
</p>
