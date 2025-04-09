# Task Master with Integrated Memory Bank & Rules System
### Task Master Core by [@eyaltoledano](https://x.com/eyaltoledano)

This project combines the **Task Master CLI**, a powerful tool for AI-driven task management, with a custom **Memory Bank & Cursor Rules system**. This integrated approach provides the AI assistant (like Cursor) with both structured tasks and the essential context, state, and guidelines needed for effective development across sessions.

**Key Concept:** The system relies on the `task-master` CLI to manage tasks defined in `tasks/tasks.json`, while the `memory-bank/` and `.cursor/rules/` directories provide the necessary operational context and instructions for the AI. Correct setup and continuous synchronization between these parts are crucial.

## Requirements

- Node.js 14.0.0 or higher
- Anthropic API key (`ANTHROPIC_API_KEY` in `.env`)
- (Optional) Perplexity API key (`PERPLEXITY_API_KEY` in `.env`) for research-enhanced features
- Task Master AI package (global or local installation)

## Setup & Initialization: A Crucial Two-Step Process

Getting started requires setting up **both** the Task Master tool and the supporting documentation framework. Please follow these steps carefully:

**Step 1: Initialize Core Task Master Components**

This step sets up the task management engine.

1.  **Install Task Master:**
    ```bash
    # Globally (recommended for CLI ease):
    npm install -g task-master-ai

    # Or Locally (ensure package.json has "type": "module"):
    # npm install task-master-ai
    ```
2.  **Run Task Master Init:** Execute this command from your project root (`shit_test`):
    ```bash
    # If installed globally:
    task-master init

    # If installed locally:
    # npx task-master init
    ```
    Follow any prompts. This typically creates the `tasks/` directory and the base `tasks/tasks.json` file.
3.  **Configure API Keys:** Create a `.env` file in the project root. Add your `ANTHROPIC_API_KEY`. Add `PERPLEXITY_API_KEY` if you plan to use the research features.
    ```dotenv
    # Example .env content
    ANTHROPIC_API_KEY=sk-ant-api03-...
    PERPLEXITY_API_KEY=pplx-... # Optional
    ```
    *(See the Configuration section below for more `.env` options)*

**Step 2: Establish the AI's Context Framework (Memory Bank & Rules)**

This step provides the AI with its essential "memory" and operating instructions. **The `task-master init` command does NOT perform this step.** It must be done manually or by instructing your AI assistant immediately after Step 1.

1.  **Create Required Directories:** Ensure these directories exist in your project root:
    ```bash
    mkdir memory-bank
    mkdir -p .cursor/rules
    ```
2.  **Create and Populate Core Files:** Create the following files within the specified directories. Initial placeholder content should be added (refer to previous setup documentation or instruct your AI to generate based on the defined structure). These files form the AI's knowledge base.

    *   **Memory Bank Files (in `memory-bank/`)**:
        *   `projectbrief.md`: High-level goals and scope.
        *   `productContext.md`: The "why" - problem, users, value.
        *   `systemPatterns.md`: Architecture, key technical decisions.
        *   `techContext.md`: Tech stack, setup details, constraints.
        *   `activeContext.md`: **Crucial:** Current focus, recent changes, next steps. (Needs frequent updates!)
        *   `progress.md`: **Crucial:** Overall status, what's done/left, issues. (Needs frequent updates!)

    *   **Cursor Rules & Learning Files (in `.cursor/` and `.cursor/rules/`)**:
        *   `.cursor/rules/_meta_instructions.mdc`: **Essential:** Defines how the AI uses this entire system. Must be read first by AI.
        *   `.cursor/rules/cursor_rules.mdc`: Guidelines for creating/maintaining rules in `.cursor/rules/`.
        *   `.cursor/rules/self_improve.mdc`: Process for evolving rules.
        *   `.cursor/rules/dev_workflow.mdc`: **Essential:** Detailed reference for the `task-master` CLI commands and their integration into the workflow.
        *   `.cursor/learned_preferences.md`: Captures dynamic learnings, user preferences, project nuances.

**Setup Complete:** Once both Step 1 and Step 2 are finished, the full system is ready. The AI assistant *must* operate according to the instructions defined in `.cursor/rules/_meta_instructions.mdc`.

## Configuration (`.env` Options)

Control Task Master behavior via environment variables in your `.env` file:

-   `ANTHROPIC_API_KEY`: **(Required)** Anthropic key.
-   `PERPLEXITY_API_KEY`: (Optional) Perplexity key.
-   `MODEL`: Claude model (default: `"claude-3-7-sonnet-20250219"`).
-   `MAX_TOKENS`: Response length limit (default: `4000`).
-   `TEMPERATURE`: Response creativity (default: `0.7`).
-   `PERPLEXITY_MODEL`: Perplexity model (default: `"sonar-medium-online"`).
-   `DEBUG`: Enable verbose logging (default: `false`).
-   `LOG_LEVEL`: Logging detail (`info`, `debug`, `warn`, `error`, default: `info`).
-   `DEFAULT_SUBTASKS`: Default subtask count for `expand` (default: `3`).
-   `DEFAULT_PRIORITY`: Default new task priority (default: `medium`).
-   `PROJECT_NAME`: Project name metadata in `tasks.json`.
-   `PROJECT_VERSION`: Project version metadata in `tasks.json`.

## How the Integrated System Operates

This project uses a layered system (defined in `.cursor/rules/_meta_instructions.mdc`) to provide the AI with comprehensive context and guidance:

1.  **Layer 1: Memory Bank (`memory-bank/`)**: Provides foundational project context, rationale, technical overview, and crucially, the **current state** (`activeContext.md`, `progress.md`). The AI **must** read all files in this layer at the start of every session to regain context.
2.  **Layer 2: Actionable Rules (`.cursor/rules/`)**: Contains specific, often context-dependent rules (`globs`), coding standards, and the essential `dev_workflow.mdc` which details how to use the `task-master` CLI.
3.  **Layer 3: Dynamic Learning (`.cursor/learned_preferences.md`)**: A place to record specific user preferences or project nuances discovered during interactions that aren't yet formal rules.

**Critical Synchronization Requirement:**

The `task-master` CLI modifies the state in `tasks/tasks.json` (e.g., task status). However, it **does not automatically update the AI's contextual knowledge** stored in the Memory Bank (`memory-bank/activeContext.md`, `memory-bank/progress.md`).

**Therefore, after executing `task-master` commands that change the project state (like `set-status`, `update`, `add-task`, `expand`), it is ESSENTIAL to manually, or by explicitly instructing the AI, update the relevant Memory Bank files IMMEDIATELY.** Failure to do this will lead to the AI working with outdated information, causing confusion and errors in subsequent steps. Think of it as manually syncing the AI's "diary" with the actual task list changes.

## Recommended Development Workflow

The AI assistant should facilitate this workflow, guided by `.cursor/rules/dev_workflow.mdc`:

1.  **Initialization:** Use `task-master parse-prd <prd_file>` and `task-master generate`. **Sync Memory Bank.**
2.  **Task Selection:** Use `task-master list` and `task-master next`. Update `activeContext.md`.
3.  **Task Understanding:** Use `task-master show <id>`. Consult Memory Bank/Rules.
4.  **Implementation:** Code the task, adhering to rules and context.
5.  **Task Completion:** Use `task-master set-status --id=<id> --status=done`. **Sync Memory Bank immediately.**
6.  **Handling Complexity:** Use `analyze-complexity`, `complexity-report`, `expand`. **Sync Memory Bank.**
7.  **Adapting to Changes ("Drift"):** Use `task-master update --from=<id> --prompt="..."`. **Sync Memory Bank (context, patterns, tech).**
8.  **Iteration:** Repeat steps 2-7, consistently maintaining Memory Bank synchronization.

## Key Task Master Commands Overview

Execute via `task-master <command>` (global) or `node scripts/dev.js <command>` (local).
*(See `.cursor/rules/dev_workflow.mdc` for full command details and options)*

-   `init`: Setup core Task Master files (Setup Step 1).
-   `parse-prd <file>`: Create `tasks.json` from Product Requirements Document.
-   `list`: Display tasks from `tasks.json`.
-   `next`: Identify the next ready task based on status and dependencies.
-   `show <id>`: Get details for a specific task or subtask.
-   `generate`: Create/update individual `tasks/*.txt` files (run after `tasks.json` changes).
-   `set-status --id=<id> --status=<status>`: Modify task status (**Requires Memory Bank Sync**).
-   `expand --id=<id> | --all [...]`: Break down tasks into subtasks (**Requires Memory Bank Sync** if scope changes).
-   `update --from=<id> --prompt="..."`: Revise future tasks based on prompt (**Requires Memory Bank Sync**).
-   `analyze-complexity [...]`: Assess task complexity and recommend subtasks.
-   `complexity-report`: Display the analysis report readably.
-   `add-dependency`, `remove-dependency`: Manage task links.
-   `validate-dependencies`, `fix-dependencies`: Check and repair dependency links.
-   `add-task --prompt="..."`: Use AI to add a new task (**Requires Memory Bank Sync**).

## Using with Cursor AI

1.  **Ensure BOTH setup steps are fully completed.**
2.  Direct the AI to **strictly adhere** to the procedures in `.cursor/rules/_meta_instructions.mdc`.
3.  Insist that the AI uses the documented `task-master` commands for all task manipulations.
4.  **Explicitly instruct and verify** that the AI updates the Memory Bank files (esp. `activeContext.md`, `progress.md`) immediately following actions that change project state. Reinforce that this synchronization is mandatory for accurate context.

## Troubleshooting

(Add relevant troubleshooting tips here if needed, e.g., regarding global command path issues or specific errors).