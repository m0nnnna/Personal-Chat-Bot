# Matrix Chatbot with Ollama Integration

This is a Node.js-based chatbot for Matrix, integrated with Ollama for natural language responses. It features persistent memory, scheduled reminders, inactivity nudges, and utility functions like web search and temperature conversion.

## Features
- **Matrix Integration**: Connects to a Matrix server, joins a room, and responds to messages.
- **Ollama AI**: Uses a local Ollama instance for conversational replies.
- **Memory Storage**: Saves user details (e.g., name, birthday) and reminders in a SQLite database.
- **Reminders & Events**: Triggers messages for birthdays, events, and timed reminders.
- **Inactivity Nudges**: Sends messages if the user is quiet for too long (daytime only).
- **Utilities**: Supports web search, temperature conversion, file reading, and network scanning.

## Setup Guide
    Clone the Repository:
    bash

git clone <repository-url>
cd matrix-chatbot

Install Dependencies:
bash

npm install matrix-js-sdk node-fetch@2 sqlite3

    Uses node-fetch@2 for CommonJS compatibility.

Configure the Script:

    Open chatbot.js and update these placeholders:
        baseUrl: Your Matrix server (e.g., https://matrix.example.com).
        user: Your bot’s Matrix ID (e.g., @bot:matrix.example.com).
        password: Your bot’s password.
        roomId: The room to join (e.g., !roomid:matrix.example.com).

Run the Bot:
bash

    node chatbot.js

        On first run, it logs in and saves credentials to bot_device.json.
        Subsequent runs use these credentials.

Usage

    Chat: Send messages in the Matrix room. The bot responds via Ollama.
    Memory: Say things like "My name is John" or "My birthday is 12/25".
    Reminders: Use "Remind me to call tomorrow at 3 PM".
    Recall: Ask "What’s my name?" or "What’s my birthday?"
    Utilities: Try "search cats", "convert 32 F to C", "file ./test.txt", or "network".

### Prerequisites
- **Node.js**: v16+ (v22 tested). Install from [nodejs.org](https://nodejs.org).
- **Ollama**: Running locally at `http://localhost:11434`. Install from [ollama.ai](https://ollama.ai) and pull the model:
  ```bash
  ollama pull hf.co/NousResearch/Hermes-2-Theta-Llama-3-8B-GGUF:Q4_K_M

### Customization
- **Persona**: Edit `loadPersona()` to change the bot’s personality. For example, make it formal, playful, or authoritative.
- **Memory**: Add patterns to `saveMemory()` for more data types. Update the regex array to capture custom phrases like "I prefer tea" or "My goal is to run".
- **Triggers**: Adjust `checkTriggers()` for custom reminders. Add new conditions to handle different events or tweak timing (e.g., weekly reminders).

## How It Works
- **Matrix Client**: Connects to a server using `matrix-js-sdk`, joins a specified room, and listens for messages via the `Room.timeline` event.
- **SQLite DB**: Stores chat history in the `chats` table (timestamp, sender, message) and user data in the `memories` table (keyword, value, trigger_date).
- **Ollama**: Powers natural language responses using a local instance at `http://localhost:11434`. It receives context from recent chats for continuity.
- **Event Handling**: Ignores messages from before startup (using `startupTime`), ensuring replies only to new inputs.

## Troubleshooting
- **Fetch Errors**: Ensure `node-fetch@2` is installed correctly (`npm ls node-fetch` should show `node-fetch@2.x.x`). Use v2 for CommonJS compatibility.
- **DB Issues**: If memory doesn’t persist or errors occur, delete `chatbot_memory.db` to reset and let the script recreate it.
- **Matrix Sync**: If the bot won’t connect, verify `baseUrl`, username, and password. Check server logs for authentication issues.
- **Ollama**: Confirm it’s running with `curl http://localhost:11434/api/tags`. Restart if needed (`ollama run <model>`).

## License
MIT License—feel free to modify, distribute, and use this code however you like!
