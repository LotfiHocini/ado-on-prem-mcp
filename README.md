# ⭐ Azure DevOps On-Premise MCP Server

This project provides a lightweight server to interact with on-premises Azure DevOps, enabling automation, project management, and streamlined workflows.

Forked from [web-marketing-hr](https://www.npmjs.com/package/@web-marketing-hr/azure-devops-mcp), which itself is a fork of [Microsoft's Azure DevOps MCP](https://github.com/microsoft/azure-devops-mcp)

## 📺 Overview

The Azure DevOps MCP Server brings Azure DevOps context to your agents. Try prompts like:

- "List my ADO projects"
- "List ADO Builds for 'Contoso'"
- "List ADO Repos for 'Contoso'"
- "List test plans for 'Contoso'"
- "List teams for project 'Contoso'"
- "List iterations for project 'Contoso'"
- "List my work items for project 'Contoso'"
- "List work items in current iteration for 'Contoso' project and 'Contoso Team'"
- "List all wikis in the 'Contoso' project"
- "Create a wiki page '/Architecture/Overview' with content about system design"
- "Update the wiki page '/Getting Started' with new onboarding instructions"
- "Get the content of the wiki page '/API/Authentication' from the Documentation wiki"

## 🏆 Expectations

The Azure DevOps MCP Server is built from tools that are concise, simple, focused, and easy to use—each designed for a specific scenario. We intentionally avoid complex tools that try to do too much. The goal is to provide a thin abstraction layer over the REST APIs, making data access straightforward and letting the language model handle complex reasoning.

## ⚙️ Supported Tools

See [TOOLSET.md](./docs/TOOLSET.md) for a comprehensive list.

## 🔌 Installation & Getting Started

For the best experience, use Visual Studio Code and GitHub Copilot. See the [getting started documentation](./docs/GETTINGSTARTED.md) to use our MCP Server with other tools such as Visual Studio 2022, Claude Code, and Cursor.

### Prerequisites

1. Install [VS Code](https://code.visualstudio.com/download) or [VS Code Insiders](https://code.visualstudio.com/insiders)
2. Install [Node.js](https://nodejs.org/en/download) 20+
3. Open VS Code in an empty folder

### Installation

In your project, add a `.vscode\mcp.json` file and setup environment varibales with the following content:

```json
{
  "inputs": [
    {
      "id": "ado_org",
      "type": "promptString",
      "description": "Azure DevOps organization name"
    }
  ],
  "servers": {
    "ado-on-prem-mcp": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@lotfihoc/ado-on-prem-mcp", "${input:ado_org}"],
      "env": {
        "LOG_LEVEL": "info",
        "ADO_MCP_MODE": "onprem",
        "ADO_MCP_AUTH_TYPE": "basic",
        "ADO_MCP_ORG_URL": "https://<on-prem-host>/tfs/<collection_name>",
        "ADO_MCP_API_VERSION": "6.0",
        "ADO_MCP_BATCH_API_VERSION": "6.0",
        "ADO_MCP_MARKDOWN_COMMENTS_API_VERSION": "5.0",
        "NODE_EXTRA_CA_CERTS": "<path_to_cert>",
        "ADO_MCP_AUTH_TOKEN": "<your_ado_pat>"
      }
    }
  }
}
```

#### Environment variables

- `ADO_MCP_AUTH_TOKEN`:
  - DevOps Personal Access Token (PAT)
- `ADO_MCP_MODE`:
  - Whether Azure DevOps is on-premises or in the cloud
  - `"cloud"` (default) or `"onprem"`
- `ADO_MCP_AUTH_TYPE`
  - DevOps authentication mode
  - `"bearer"` (default) or `"basic"`
- `ADO_MCP_ORG_URL`:
  - Full URL of the on-premises instance, for example:
    `https://my-server/tfs/MyCollection`
- `ADO_MCP_API_VERSION`:
  - Set Azure DevOps API version
  - default: `"6.0-preview"`
- `ADO_MCP_BATCH_API_VERSION`:
  - Set Azure DevOps Batch API version
  - default: `"6.0-preview"`
- `ADO_MCP_MARKDOWN_COMMENTS_API_VERSION`:
  - Set Azure DevOps Markdown API version
  - default: `"5.0"`
- `NODE_EXTRA_CA_CERTS`:
  - Set this variable to your certifcat path if needed
- `ADO_MCP_AUTH_TOKEN`:
  - Ado PAT

If you are using continue dev on vscode, you can use this config

```yaml
mcpServers:
  - name: ado-on-prem-mcp
    type: stdio
    command: npx
    args:
      - -y
      - "@lotfihoc/ado-on-prem-mcp"
      - "<your_devops_project_name>"
      - "--authentication"
      - "envvar"
    env:
      LOG_LEVEL: "info"
      ADO_MCP_MODE: "onprem"
      ADO_MCP_AUTH_TYPE: "basic"
      ADO_MCP_ORG_URL: "https://<on-prem-host>/tfs/<collection_name>"
      ADO_MCP_API_VERSION: "6.0"
      ADO_MCP_BATCH_API_VERSION: "6.0"
      ADO_MCP_MARKDOWN_COMMENTS_API_VERSION: "5.0"
      ADO_MCP_AUTH_TOKEN: ${{ secrets.ADO_TOKEN }}
      NODE_EXTRA_CA_CERTS: ${{ secrets.NODE_EXTRA_CA_CERTS }}
```

It's recommended to set `ADO_MCP_AUTH_TOKEN` in your terminal or command line. Windows example:

```bat
setx ADO_MCP_AUTH_TOKEN "<pat_token>"`
```

🔥 To stay up to date with the latest features, you can use our nightly builds. Simply update your `mcp.json` configuration to use `@lotfihoc/ado-on-prem-mcp@next`. Here is an updated example:

```json
{
  "inputs": [
    {
      "id": "ado_org",
      "type": "promptString",
      "description": "Azure DevOps organization name"
    }
  ],
  "servers": {
    "ado-on-prem-mcp": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@lotfihoc/ado-on-prem-mcp@next", "${input:ado_org}"],
      "env": {
        "LOG_LEVEL": "info",
        "ADO_MCP_MODE": "onprem",
        "ADO_MCP_AUTH_TYPE": "basic",
        "ADO_MCP_ORG_URL": "https://<on-prem-host>/tfs/<collection_name>",
        "ADO_MCP_API_VERSION": "6.0",
        "ADO_MCP_BATCH_API_VERSION": "6.0",
        "ADO_MCP_MARKDOWN_COMMENTS_API_VERSION": "5.0",
        "NODE_EXTRA_CA_CERTS": "<path_to_cert>",
        "ADO_MCP_AUTH_TOKEN": "<your_ado_pat>"
      }
    }
  }
}
```

Save the file, then click 'Start'.

![start mcp server](./docs/media/start-mcp-server.gif)

In chat, switch to [Agent Mode](https://code.visualstudio.com/blogs/2025/02/24/introducing-copilot-agent-mode).

Click "Select Tools" and choose the available tools.

![configure mcp server tools](./docs/media/configure-mcp-server-tools.gif)

Open GitHub Copilot Chat and try a prompt like `List ADO projects`. The first time an ADO tool is executed browser will open prompting to login with your Microsoft account. Please ensure you are using credentials matching selected Azure DevOps organization.

> 💥 We strongly recommend creating a `.github\copilot-instructions.md` in your project. This will enhance your experience using the Azure DevOps MCP Server with GitHub Copilot Chat.
> To start, just include "`This project uses Azure DevOps. Always check to see if the Azure DevOps MCP server has a tool relevant to the user's request`" in your copilot instructions file.

See the [getting started documentation](./docs/GETTINGSTARTED.md) to use our MCP Server with other tools such as Visual Studio 2022, Claude Code, and Cursor.

## 🌏 Using Domains

Azure DevOps exposes a large surface area. As a result, our Azure DevOps MCP Server includes many tools. To keep the toolset manageable, avoid confusing the model, and respect client limits on loaded tools, use Domains to load only the areas you need. Domains are named groups of related tools (for example: core, work, work-items, repositories, wiki). Add the `-d` argument and the domain names to the server args in your `mcp.json` to list the domains to enable.

For example, use `"-d", "core", "work", "work-items"` to load only Work Item related tools (see the example below).

> By default all domains are loaded

```json
{
  "inputs": [
    {
      "id": "ado_org",
      "type": "promptString",
      "description": "Azure DevOps organization name"
    }
  ],
  "servers": {
    "ado-on-prem-mcp-with-filtered-domains": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@lotfihoc/ado-on-prem-mcp", "${input:ado_org}", "-d", "core", "work", "work-items"]
      "env": {
        "LOG_LEVEL": "info",
        "ADO_MCP_MODE": "onprem",
        "ADO_MCP_AUTH_TYPE": "basic",
        "ADO_MCP_ORG_URL": "https://<on-prem-host>/tfs/<collection_name>",
        "ADO_MCP_API_VERSION": "6.0",
        "ADO_MCP_BATCH_API_VERSION": "6.0",
        "ADO_MCP_MARKDOWN_COMMENTS_API_VERSION": "5.0",
        "NODE_EXTRA_CA_CERTS": "<path_to_cert>",
        "ADO_MCP_AUTH_TOKEN": "<your_ado_pat>"
      }
    }
  }
}

```

Domains that are available are: `core`, `work`, `work-items`, `search`, `test-plans`, `repositories`, `wiki`, `pipelines`, `advanced-security`

We recommend that you always enable `core` tools so that you can fetch project level information.

If you are using continiue dev, you can use the following config

```yml
mcpServers:
  - name: ado-on-prem-mcp-domains
    type: stdio
    command: npx
    args:
      - -y
      - "@lotfihoc/ado-on-prem-mcp"
      - "<your_devops_project_name>"
      - "-d"
      - "core"
      - "work"
      - "work-items"
      - "--authentication"
      - "envvar"
    env:
      LOG_LEVEL: "info"
      ADO_MCP_MODE: "onprem"
      ADO_MCP_AUTH_TYPE: "basic"
      ADO_MCP_ORG_URL: "https://<on-prem-host>/tfs/<collection_name>"
      ADO_MCP_API_VERSION: "6.0"
      ADO_MCP_BATCH_API_VERSION: "6.0"
      ADO_MCP_MARKDOWN_COMMENTS_API_VERSION: "5.0"
      ADO_MCP_AUTH_TOKEN: ${{ secrets.ADO_TOKEN }}
      NODE_EXTRA_CA_CERTS: ${{ secrets.NODE_EXTRA_CA_CERTS }}
```
