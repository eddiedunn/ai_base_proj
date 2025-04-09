# Technical Context

## Core Technologies:
- Node.js (v18.0.0 or higher)
- pnpm for node package management
- Task Master CLI (custom implementation)
- Anthropic API (via `@anthropic-ai/sdk`)
- Perplexity API (via `openai` SDK) - Optional
- Commander.js for CLI interface
- Jest for testing

## Available Technologies (Use Only When Appropriate):
- **Frontend Development:**
  - Next.js - React framework for web applications
  - TypeScript - Type-safe JavaScript
  - Tailwind CSS - Utility-first CSS framework
  - Framer Motion - Animation library for React
  - Storybook - UI component development environment

- **Backend & Data:**
  - tRPC - End-to-end typesafe APIs
  - Prisma - Type-safe ORM for database access
  - NextAuth - Authentication for Next.js
  - Resend - Email delivery API
  - AWS S3 - Object storage service
  - Ingest - Data ingestion and processing

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
- Only integrate additional technologies when there is a clear justification
- Maintain focus on core Task Master functionality as the priority

## Key Dependencies:
- `@anthropic-ai/sdk` - For AI capabilities
- `commander` - For CLI interface
- `jest` - For testing
- `dotenv` - For environment configuration 