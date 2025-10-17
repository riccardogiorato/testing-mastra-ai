# Testing Mastra AI with Together AI

A demonstration project showcasing how to integrate **Together AI** with **Mastra** using the **Vercel AI SDK** provider integration.

## Features

- 🤖 **AI Agent**: Weather assistant powered by Together AI's Kimi model
- 🌤️ **Weather Tool**: Fetches real-time weather data
- 💾 **Memory**: Persistent conversation memory using LibSQL
- ⚡ **Fast Setup**: Ready to run with minimal configuration

## Installation

```bash
# Clone the repository
git clone <repository-url>
cd testing-mastra-ai

# Install dependencies
pnpm install
```

## Setup

1. **Get your Together AI API key** from [Together AI](https://together.ai)

2. **Create environment file**:

   ```bash
   cp .env.example .env
   ```

3. **Add your API key** to `.env`:
   ```env
   TOGETHER_API_KEY=your-together-ai-api-key-here
   ```

## Usage

### Using Together AI with Vercel AI SDK

This project demonstrates how to integrate Together AI with Mastra using the Vercel AI SDK provider:

```typescript
import { createTogetherAI } from "@ai-sdk/togetherai";
import { Agent } from "@mastra/core";

// Initialize Together AI provider
const together = createTogetherAI({
  apiKey: process.env.TOGETHER_API_KEY ?? "",
});

// Create an agent with Together AI model
export const weatherAgent = new Agent({
  name: "Weather Agent",
  instructions: `
    You are a helpful weather assistant that provides accurate weather information.
    Use the weatherTool to fetch current weather data.
  `,
  model: together("kimi"), // Together AI model
  tools: { weatherTool },
  memory: new Memory({
    storage: new LibSQLStore({
      url: "file:../mastra.db",
    }),
  }),
});
```

### Key Integration Points

- **Provider Setup**: Use `createTogetherAI()` from `@ai-sdk/togetherai`
- **Model Selection**: Choose from Together AI's model catalog (e.g., `kimi`)
- **Seamless Integration**: Works with Mastra's agent framework out of the box

## Running the Project

```bash
# Development mode
pnpm run dev

# Build for production
pnpm run build

# Start production server
pnpm run start
```

## Project Structure

```
src/
├── mastra/
│   ├── agents/
│   │   └── weather-agent.ts    # Weather agent with Together AI
│   ├── tools/
│   │   └── weather-tool.ts     # Weather data fetching tool
│   ├── workflows/
│   │   └── weather-workflow.ts # Weather workflow
│   └── index.ts                # Mastra configuration
.env.example                     # Environment variables template
```

## Learn More

- [Mastra Documentation](https://mastra.ai/docs)
- [Together AI Models](https://together.ai/models)
- [Vercel AI SDK](https://docs.together.ai/docs/using-together-with-vercels-ai-sdk)

## License

ISC
