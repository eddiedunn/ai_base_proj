# Technical Context

## Core Technologies:
- Node.js (v18.0.0 or higher)
- Task Master CLI (custom implementation)
- Anthropic API (via `@anthropic-ai/sdk`)
- Perplexity API (via `openai` SDK) - Optional
- Commander.js for CLI interface
- Jest for testing

## Development Setup:
1. Install Node.js (v18.0.0 or higher)
2. Clone the repository
3. Install dependencies: `npm install`
4. Create `.env` file with required API keys:
   ```
   ANTHROPIC_API_KEY=your_key_here
   PERPLEXITY_API_KEY=your_key_here  # Optional
   ```
5. Run tasks via `task-master <command>` or `npm run <script_name>`

## Technical Constraints:
- Must maintain compatibility with Node.js v18.0.0+
- API rate limits for Anthropic and Perplexity
- Memory Bank system must be self-contained
- CLI must be intuitive and well-documented

## Key Dependencies:
- `@anthropic-ai/sdk` - For AI capabilities
- `commander` - For CLI interface
- `jest` - For testing
- `dotenv` - For environment configuration 