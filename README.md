# Task Master with Memory Bank & Rules System
### by [@eyaltoledano](https://x.com/eyaltoledano) (Task Master core)

A task management system for AI-driven development using Claude Task Master, enhanced with a structured Memory Bank and Cursor Rules system for improved AI context and workflow management, designed to work seamlessly with Cursor AI.

**IMPORTANT:** This project combines the standard Task Master CLI with a custom documentation system (Memory Bank & Rules). Setting it up correctly requires following the specific two-step process outlined below.

## Requirements

- Node.js 14.0.0 or higher
- Anthropic API key (`ANTHROPIC_API_KEY` in `.env`)
- (Optional) Perplexity API key (`PERPLEXITY_API_KEY` in `.env`) for enhanced features
- Task Master AI package (global or local installation)

## Setup & Initialization (Two-Step Process - Read Carefully!)

This project requires setting up both the Task Master core and the supporting Memory Bank/Rules structure.

**Step 1: Initialize Task Master Core**

1.  Install the Task Master package:
    ```bash
    # Install globally (recommended for easy CLI access)
    npm install -g task-master-ai

    # OR install locally within your project
    # npm install task-master-ai
    # Ensure your package.json has "type": "module"
    ```
2.  Run the Task Master initialization from your project's root directory (`shit_test`):
    ```bash
    # If installed globally
    task-master init

    # If installed locally
    # npx task-master init
    ```
    Follow the prompts. This will set up the basic `tasks/tasks.json`, the `tasks/` directory, and potentially other core Task Master files.
3.  Create a `.env` file in the project root (`shit_test`).
4.  Add your `ANTHROPIC_API_KEY` to the `.env` file. You can also add other optional configuration variables here (see Configuration section below).
    Example `.env` entry:
    ```
    ANTHROPIC_API_KEY=sk-ant-api03-...
    ```

**Step 2: Set Up Memory Bank & Cursor Rules (Required for AI Operation)**

The `task-master init` command **DOES NOT** create the necessary documentation structure for the AI. You **MUST** create this structure **manually or by instructing your AI assistant** *after* completing Step 1. This structure is essential for the AI to maintain context and follow project guidelines.

1.  **Create Directories** (if they don't exist after Step 1):
    ```bash
    # Run these from your project root (shit_test)
    mkdir memory-bank
    mkdir -p .cursor/rules
    ```
2.  **Create and Populate Files:** Create the following files. Your AI assistant can help generate the initial placeholder content based on the project's defined structure. If working manually, create the files and add basic placeholder content initially. (The detailed content for these files should have been provided separately or exists within the project if cloned).
    *   `memory-bank/projectbrief.md`
    *   `memory-bank/productContext.md`
    *   `memory-bank/systemPatterns.md`
    *   `memory-bank/techContext.md`
    *   `memory-bank/activeContext.md` (Crucial for state tracking)
    *   `memory-bank/progress.md` (Crucial for state tracking)
    *   `.cursor/rules/_meta_instructions.mdc` (**Mandatory reading for the AI**)
    *   `.cursor/rules/cursor_rules.mdc` (Rules for writing rules)
    *   `.cursor/rules/self_improve.mdc` (Process for updating rules)
    *   `.cursor/rules/dev_workflow.mdc` (Detailed Task Master commands/workflow)
    *   `.cursor/learned_preferences.md` (For dynamic AI learning)

**Setup Completion:** Once both steps are done, the project is ready. The AI *must* be instructed to read and follow the process defined in `.cursor/rules/_meta_instructions.mdc` for all operations.

## Configuration (`.env` file)

Configuration is managed via a `.env` file in the project root.

-   **`ANTHROPIC_API_KEY`**: (Required) Your Anthropic API key.
-   **`PERPLEXITY_API_KEY`**: (Optional) Your Perplexity API key for research features.
-   **`MODEL`**: Claude model (default: "claude-3-7-sonnet-20250219").
-   **`MAX_TOKENS`**: Max tokens for AI responses (default: 4000).
-   **`TEMPERATURE`**: AI response temperature (default: 0.7).
-   **`PERPLEXITY_MODEL`**: Perplexity model (default: "sonar-medium-online").
-   **`DEBUG`**: Enable debug logging (default: false).
-   **`LOG_LEVEL`**: Log level (default: info).
-   **`DEFAULT_SUBTASKS`**: Default subtask count for `expand` (default: 3).
-   **`DEFAULT_PRIORITY`**: Default task priority (default: medium).
-   **`PROJECT_NAME`**: Override project name in `tasks.json`.
-   **`PROJECT_VERSION`**: Override version in `tasks.json`.

## How the Combined System Works

This project uses a layered approach for AI context and task management, defined in `.cursor/rules/_meta_instructions.mdc`:

1.  **Layer 1: Memory Bank (`memory-bank/`)**: Foundational project context (Why, What, How-HighLevel, Current State). Read fully by the AI at the start of every session. **Must be kept up-to-date MANUALLY or by AI instruction** after relevant changes.
2.  **Layer 2: Actionable Rules (`.cursor/rules/`)**: Specific coding standards, workflow definitions (`dev_workflow.mdc` detailing Task Master usage), and meta-rules. Consulted as needed.
3.  **Layer 3: Dynamic Learning (`.cursor/learned_preferences.md`)**: Captures user preferences and nuances learned during interaction.

**CRITICAL:** Task state changes made via the `task-master` CLI (e.g., setting status to 'done') **MUST** be reflected by updating the Memory Bank files (especially `activeContext.md` and `progress.md`). This synchronization is currently **manual or requires explicit AI instruction** – it is not automatic.

## Development Workflow (AI-Driven)

The typical workflow involves using the `task-master` CLI (often executed by the AI assistant) and ensuring the Memory Bank stays synchronized:

1.  **Initialize Tasks:** Start with `task-master parse-prd <prd_file>` and `task-master generate`. **Update Memory Bank** (progress, active context).
2.  **Find Next Task:** Use `task-master list` and `task-master next`. Update `activeContext.md` with the chosen task.
3.  **Understand Task:** Use `task-master show <id>`. Consult Memory Bank and relevant rules.
4.  **Implement:** Code the task.
5.  **Complete Task:** Use `task-master set-status --id=<id> --status=done`. **IMMEDIATELY update `progress.md` and `activeContext.md` to reflect completion.**
6.  **Handle Complexity:** Use `analyze-complexity`, `complexity-report`, `expand`. Update Memory Bank.
7.  **Handle Drift:** Use `task-master update --from=<id> --prompt="..."`. Update Memory Bank (context, patterns, tech).
8.  **Repeat:** Continue finding and completing tasks, always syncing with the Memory Bank.

Refer to `.cursor/rules/dev_workflow.mdc` for full details on the workflow and commands.

## Key Task Master Commands

Use `task-master <command>` (if installed globally) or `node scripts/dev.js <command>`.

-   `init`: Initialize core Task Master files (Step 1 of Setup).
-   `parse-prd <file>`: Generate `tasks.json` from PRD.
-   `list`: Show tasks.
-   `next`: Find the next task to work on.
-   `show <id>`: Show task details.
-   `generate`: Create/update individual `tasks/*.txt` files from `tasks.json`.
-   `set-status --id=<id> --status=<status>`: Update task status (**Requires Memory Bank update**).
-   `expand --id=<id> | --all [...]`: Add subtasks.
-   `update --from=<id> --prompt="..."`: Modify future tasks (**Requires Memory Bank update**).
-   `analyze-complexity [...]`: Generate complexity report.
-   `complexity-report`: View complexity report.
-   `add-dependency`, `remove-dependency`, `validate-dependencies`, `fix-dependencies`: Manage task links.
-   `add-task --prompt="..."`: Add a new task via AI (**Requires Memory Bank update**).

**See `.cursor/rules/dev_workflow.mdc` for the complete command reference and options.**

## Integrating with Cursor AI

1.  **Complete BOTH setup steps** described above accurately.
2.  Instruct the AI to **strictly follow** the operational guidelines in `.cursor/rules/_meta_instructions.mdc`.
3.  The AI must use the `task-master` commands for all task operations.
4.  The AI **must be explicitly instructed or reminded** to update the Memory Bank files (`activeContext.md`, `progress.md`, etc.) immediately after relevant `task-master` commands are executed or significant project changes occur. Emphasize that this synchronization is crucial and not automatic.

## Troubleshooting

(Include relevant troubleshooting tips from the original README if needed, e.g., issues with global commands).