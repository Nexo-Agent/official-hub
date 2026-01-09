# Nexo Official Hub 🚀

This is the official repository containing resources (Agents, MCP Servers, and Prompt Templates) for the Nexo application. The Nexo app utilizes this repository as a "Resource Store," allowing users to discover and install enhancements with a single click.

## 📥 Integration with Nexo App

To integrate this repository into the application, developers should follow these technical implementation steps:

### 1. Fetching the Registry

The application should fetch the `index.json` file from the main branch using the GitHub Raw URL:
`https://raw.githubusercontent.com/Nexo-Agent/official-hub/main/index.json`

### 2. Installing Agents (Git Install)

When a user clicks "Install" on an Agent, the application performs the following workflow based on the `git_install` metadata:

1. **Clone**: Use `repository_url` and `revision` to clone the repository to a local directory.
2. **Subpath**: Only extract/use data from the specified `subpath`.
3. **Environment Setup**:
   - Locate the `entry_point` (typically `tools/main.py`).
   - If it's a Python agent, automatically create a virtual environment (`venv`) and run `pip install -r tools/requirements.txt`.
4. **Registration**: Add an entry to the user's MCP configuration using the absolute path to the `venv` Python executable and the entry point script.

### 3. Configuring MCP Servers

MCP Servers support two primary connection types:

- **`type: "stdio"`**: Local execution via command line.
  - Uses `config.command` and `config.args`.
  - If variables exist in `env` or `args` (formatted as `{variable_name}`), the app must render a form from the `variables` array to collect user input before saving.
- **`type: "sse"`**: Remote connection via URL.
  - Uses `config.url` and `config.headers`.
  - Similarly, parse placeholders in `headers` to prompt the user for required information (e.g., API Keys).

### 4. Utilizing Prompt Templates

1. Download the JSON template from the specified `path`.
2. Parse the `template` field to identify `{variable}` placeholders.
3. Use the `variables` array (label, description, default) to generate an interactive UI form.
4. After user input, inject the values into the template and send the final prompt to the LLM.

## 🏗️ Repository Structure

```text
.
├── index.json            # Central registry (Resource Map)
├── assets/               # Icons and visual assets
├── agents/               # Source code for AI Agents (Each folder is an MCP Server)
│   ├── nexo-qa/          # E2E Testing Agent
│   ├── nexo-builder/     # Full-stack Development Agent
│   └── nexo-ops/         # DevOps & Operations Agent
├── prompts/              # Curated prompt templates in JSON format
└── README.md             # This documentation
```

## 🛠️ Contribution Guidelines

We welcome community contributions via Pull Requests:

1. Add your resource folder to `agents/` or `prompts/`.
2. Upload the corresponding icon to `assets/icons/`.
3. Register the new resource in `index.json`.

---

**Nexo Team** - _Empowering AI Agents._
