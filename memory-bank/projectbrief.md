# Project Brief: Task Master Project

## Core Goal:
To create a robust task management system that integrates AI capabilities for task analysis, planning, and execution tracking.

## High-Level Scope:
- Task parsing and generation from PRDs
- Task dependency management
- AI-powered task complexity analysis
- Task status tracking and workflow management
- Integration with Memory Bank system for context awareness

## Primary Technology Focus:
Node.js backend with AI integration (Anthropic API), using a structured Memory Bank system for context management.

## Source of Truth:
This Memory Bank, `.cursor/rules`, and `tasks/tasks.json` (managed by `task-master` CLI documented in `[dev_workflow.mdc](mdc:.cursor/rules/dev_workflow.mdc)`). 