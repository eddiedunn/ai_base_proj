# System Patterns & Architecture

## Overall Architecture:
The system follows a modular CLI-based architecture with three main components:
1. Task Management Core (CLI interface and task processing)
2. AI Integration Layer (Anthropic API integration)
3. Memory Bank System (Context management and persistence)

## Key Technical Decisions:
- Node.js as the primary runtime environment
- Command-line interface for task management
- JSON-based task storage and configuration
- Memory Bank system for context persistence
- Anthropic API for AI capabilities
- Modular design for easy extension

## Core Design Patterns:
- Command Pattern for CLI operations
- Repository Pattern for task storage
- Observer Pattern for task status updates
- Strategy Pattern for AI model selection
- Factory Pattern for task generation

## Component Relationships:
- CLI commands interact with the Task Management Core
- Task Management Core uses AI Integration for complex operations
- Memory Bank system provides context to both Task Management and AI Integration
- All components share a common task data model 