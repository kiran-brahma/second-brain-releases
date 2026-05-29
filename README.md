# Second Brain

A local AI knowledge base for macOS. Your documents, your models, your machine — nothing leaves unless you send it to an AI provider.

## What it does

Second Brain lets you build a personal knowledge base and have AI conversations grounded in what you actually know. Every answer draws from your own documents before responding. The AI never invents context it cannot find.

### Core features

**Chat with your knowledge base**
Ask questions in natural language. Before answering, the AI decomposes your question into precise search queries, retrieves the most relevant documents from your knowledge base using hybrid vector + keyword search, and synthesises a cited answer. You can review and edit the search queries before retrieval runs.

**Document library**
Store articles, notes, research papers, transcripts — any text. Each document gets an AI-generated structured summary (scope, key claims, concepts, connections). That summary is what gets searched. Fix a weak summary and retrieval improves immediately.

**Review queue**
Spaced-repetition review built into your library. The system schedules documents before you forget them based on your recall ratings. Short daily sessions compound faster than occasional long ones.

**Critiques**
After any AI response, run a critique. A second AI model — separate from your chat model — reads the response and stress-tests the reasoning: logical fallacies, missing perspectives, unsupported claims, framing bias, cognitive distortions. Two models trained differently disagree more, which produces a more independent assessment.

**Crucible**
Finds pairs of documents in your knowledge base that contradict each other. Contradictions are not problems — they are signal. When you find a tension, the AI rewrites both document summaries to encode where they agree and where they diverge. The tension is then captured in search.

**Writing workflow**
Six-phase pipeline from raw thesis to reviewed essay, grounded in your own knowledge:

1. **Grill** — Socratic dialogue. The AI pushes back on your thesis until it has a clear picture of your core claim and desired outcome.
2. **Research** — Decomposes the thesis into research themes. Each theme searches your knowledge base and optional live web. Produces structured research briefs.
3. **Draft** — Full-screen rich-text editor with your research briefs one click away.
4. **Review** — Four cognitive audits: foundational argument check, bias surface, replicability test, style rules. Each streamed token by token.
5. **Iterate** — Revise and resubmit. Minimum three review cycles before finalising — enforced, not optional.
6. **Style guide** — Final check against your own writing rules. You define the rules in Settings.

## AI providers

Works with Ollama (local, fully private), Claude, OpenAI, Gemini, or any OpenAI-compatible endpoint. Chat and embedding providers are configured independently. Your API keys stay on your machine, stored in a local SQLite database.

## Data

All data lives in a SQLite database on your machine at `~/Library/Application Support/com.secondbrain.tauri/`. Nothing is synced or sent to any server except the AI provider you choose.

## Download

Go to [Releases](https://github.com/kiran-brahma/second-brain-releases/releases) and download the latest `.dmg`.

## Installation

1. Open the `.dmg` and drag **Second Brain.app** to Applications.
2. Run this once in Terminal to remove the quarantine flag:
   ```bash
   xattr -d com.apple.quarantine "/Applications/Second Brain.app"
   ```
3. Launch the app normally.

The app is not notarized with an Apple Developer certificate. The `xattr` command is a one-time step — subsequent updates install automatically inside the app and do not require it again.

## Updates

The app checks for updates on every launch. When a new version is available, a banner appears in the bottom-right corner. Click **Install** and the app updates and relaunches automatically.
