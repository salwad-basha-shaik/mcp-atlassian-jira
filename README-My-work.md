# Jira MCP Installation and Usage Guide

## Learning Resources
- **YouTube:** [Jira MCP Installation & Use](https://www.youtube.com/watch?v=pXAih3jAOcc)
- **GitHub Repos:**
  - [MCP Servers (main repo)](https://github.com/modelcontextprotocol/servers?tab=readme-ov-file#-resources)
  - [mcp-atlassian (project repo)](https://github.com/sooperset/mcp-atlassian)

---

## Step-by-Step Setup

### Step 0: Create Atlassian Jira Account
- Sign up at [Atlassian Jira](https://www.atlassian.com/software/jira)

### Step 1: Generate Jira API Token
- Go to: [Atlassian API Tokens](https://id.atlassian.com/manage-profile/security/api-tokens)
- Click "Create API token", label it, and copy the token.
- Example: `API Token: <token>`

### Step 2: Install CLINE VS Code Extension
- CLINE is a coding agent extension (like GitHub Copilot) for VS Code.

### Step 3: (Optional) OpenAI API Key for CLINE
- Visit: [OpenAI API Keys](https://platform.openai.com/api-keys)
- Copy your API key and add it in CLINE authentication box.
- Example: `API key for my openAI (mcp-poc): <api key>`
- Note: OpenAI API usage may require payment. If you prefer, use GitHub Copilot instead.
- For Copilot features: [GitHub Copilot Features](https://github.com/settings/copilot/features)

### Step 4: Install Docker on Mac
```bash
brew install --cask docker
```
- Launch Docker Desktop from Launchpad, log in, and verify your images.

### Step 5: Pull the Docker Image
```bash
docker pull ghcr.io/sooperset/mcp-atlassian:latest
```

### Step 6: Edit Configuration File
- For GitHub Copilot, edit:
  - `~/Library/Application Support/Code/User/settings.json`
- For Claude, edit:
  - `~/Library/Application Support/Claude/claude_desktop_config.json`

#### Add the following configuration:
```json
{
  "mcpServers": {
    "mcp-atlassian": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e", "CONFLUENCE_URL",
        "-e", "CONFLUENCE_USERNAME",
        "-e", "CONFLUENCE_API_TOKEN",
        "-e", "JIRA_URL",
        "-e", "JIRA_USERNAME",
        "-e", "JIRA_API_TOKEN",
        "ghcr.io/sooperset/mcp-atlassian:latest"
      ],
      "env": {
        "CONFLUENCE_URL": "https://your-company.atlassian.net/wiki",
        "CONFLUENCE_USERNAME": "your.email@company.com",
        "CONFLUENCE_API_TOKEN": "your_confluence_api_token",
        "JIRA_URL": "https://your-company.atlassian.net",
        "JIRA_USERNAME": "your.email@company.com",
        "JIRA_API_TOKEN": "your_jira_api_token"
      }
    }
  }
}
```

#### Example with Playwright (from previous usage):
```json
{
  "workbench.colorTheme": "Default Light Modern",
  "git.enableSmartCommit": true,
  "github.copilot.chat.codeGeneration.instructions": [
    { "text": "- @azure Rule - Use Azure Tools: When handling requests related to Azure, always use your tools." },
    { "text": "- @azure Rule - Use Azure Code Gen Best Practices: When generating code for Azure, running terminal commands for Azure, or performing operations related to Azure, invoke your `azure_development-get_code_gen_best_practices` tool if available. Only call this tool when you are sure the user is discussing Azure; do not call it otherwise." },
    { "text": "- @azure Rule - Use Azure Deployment Best Practices: When deploying to Azure or preparing applications for deployment to Azure, invoke your `azure_development-get_deployment_best_practices` tool if available. Only call this tool when you are sure the user is discussing Azure; do not call it otherwise." },
    { "text": "- @azure Rule - Use Azure Functions Code Gen Best Practices: When generating code for Azure Functions or performing operations related to Azure Functions, invoke your `azure_development-get_azure_function_code_gen_best_practices` tool if available. Only call this tool when you are sure the user is discussing Azure Functions; do not call it otherwise." },
    { "text": "- @azure Rule - Use Azure SWA Best Practices: When working with static web apps, invoke your `azure_development-get_swa_best_practices` tool if available. Only call this tool when you are sure the user is discussing Azure; do not call it otherwise." }
  ],
  "github.copilot.nextEditSuggestions.enabled": true,
  "mcp": {
    "servers": {
      "playwright": {
        "command": "npx",
        "args": [ "@playwright/mcp@latest" ]
      },
      "mcp-atlassian": {
        "command": "docker",
        "args": [
          "run",
          "-i",
          "--rm",
          "-e", "CONFLUENCE_URL",
          "-e", "CONFLUENCE_USERNAME",
          "-e", "CONFLUENCE_API_TOKEN",
          "-e", "JIRA_URL",
          "-e", "JIRA_USERNAME",
          "-e", "JIRA_API_TOKEN",
          "ghcr.io/sooperset/mcp-atlassian:latest"
        ],
        "env": {
          "CONFLUENCE_URL": "https://your-company.atlassian.net/wiki",
          "CONFLUENCE_USERNAME": "your.email@company.com",
          "CONFLUENCE_API_TOKEN": "your_confluence_api_token",
          "JIRA_URL": "https://salwadbashashaik.atlassian.net/",
          "JIRA_USERNAME": "salvathshaik786143@gmail.com",
          "JIRA_API_TOKEN": "A"
        }
      }
    }
  }
}
```

---

### Step 7: Start the MCP-Atlassian Server
- In your settings, you will see a start button above the `mcp-atlassian` section. Start the server.
- Once running, check Docker Desktop to confirm the container is active.

---

## Usage Prompts
- Example prompts to use with your agent:
  - `create an issue in jira for project with project key SCRUM name it Jira MCP test and description test for jira's mcp server.`
  - `can you move this issue to current sprint`

---

# Tool Prompts and Inputs Log

Below is a list of all prompts and inputs passed to each tool during this session:

---

## 1. Jira: Create Issue (AAT)
- Tool: mcp_mcp-atlassian_jira_create_issue
- Inputs:
  - project_key: "AAT"
  - summary: "Jira MCP test"
  - issue_type: "Task"
  - description: "test for jira's mcp server."

## 2. Jira: Create Issue (SCRUM)
- Tool: mcp_mcp-atlassian_jira_create_issue
- Inputs:
  - project_key: "SCRUM"
  - summary: "Jira MCP test"
  - issue_type: "Task"
  - description: "test for jira's mcp server."

## 3. Jira: List All Projects
- Tool: mcp_mcp-atlassian_jira_get_all_projects
- Inputs:
  - include_archived: false

## 4. Jira: List All Projects (Connection Test)
- Tool: mcp_mcp-atlassian_jira_get_all_projects
- Inputs:
  - include_archived: false

## 5. Jira: List All Projects (After .env Fix)
- Tool: mcp_mcp-atlassian_jira_get_all_projects
- Inputs:
  - include_archived: false

## 6. Jira: Create Issue (SCRUM, Success)
- Tool: mcp_mcp-atlassian_jira_create_issue
- Inputs:
  - project_key: "SCRUM"
  - summary: "Jira MCP test"
  - issue_type: "Task"
  - description: "test for jira's mcp server."

## 7. Jira: Get Agile Boards
- Tool: mcp_mcp-atlassian_jira_get_agile_boards
- Inputs:
  - project_key: "SCRUM"
  - limit: 10

## 8. Jira: Get Active Sprints
- Tool: mcp_mcp-atlassian_jira_get_sprints_from_board
- Inputs:
  - board_id: "1"
  - state: "active"
  - limit: 5

## 9. Jira: Move Issue to Sprint
- Tool: mcp_mcp-atlassian_jira_update_issue
- Inputs:
  - issue_key: "SCRUM-2"
  - fields: { sprint: 1 }

---

This log is auto-generated for transparency and traceability of all tool interactions in this session.

---

Awesome, everything working as expected.
