# Second Brain

We all have notes spread across Obsidian, Notion, Apple Notes — whatever app we fancy. The biggest problem I faced was finding the right note at the right moment. AI can simplify this, but I found that most RAG implementations lose something in the translation. I started thinking about how to build a simple app based on how I search for ideas and knowledge across my own notes. Second Brain is my answer. Feel free to explore and see if it meets your needs. I use this daily and update it as I find issues.

---

## How It Works

### The Core Idea

Standard RAG breaks documents into chunks. When you query, it retrieves chunks and hands them to the AI. The problem: chunks lose context.

I took a different approach, drawing from Anthropic's [Contextual RAG](https://www.anthropic.com/research/contextual-retrieval) idea. Every document in the knowledge base gets an AI-generated summary with five sections:

- **Scope** — what this document covers
- **Questions** — three to five core questions this document answers
- **Key Claims** — the main arguments the document makes
- **Key Concepts** — the central ideas explained within it
- **Connections** — links to broader ideas and adjacent knowledge

The system embeds this summary, not the raw document. When you search, it retrieves the summary to find the right document, then sends the full document text to the AI as context. This gives the AI complete context rather than isolated snippets — and works well now that AI model context windows have expanded significantly.

---

## Features

### Chat with Your Knowledge Base

Ask a question in natural language. Before answering, the AI breaks your query into precise search queries (which you can review and adjust). It runs a hybrid vector and keyword search, retrieves the most relevant documents, and synthesises a cited answer grounded in your own knowledge.

### Search

When you want to read rather than ask, use Search. It runs the same retrieval process but returns a list of documents to read on your own — no AI answer, just your notes, surfaced.

### Document Library

Store articles, notes, research papers, YouTube transcripts, book notes — any text. Once you add a document, the AI generates the structured summary described above. Review it, fix what needs fixing, and embed it. The summary quality determines retrieval quality. Initial summaries need some editing to work well.

### Review Queue

Spaced-repetition review built into the library. The system schedules documents before you forget them based on your recall ratings. It generates questions to test whether you actually remember what you read. Short daily sessions compound faster than occasional long ones.

### Critiques

After any AI response, run a critique. A second AI model reads the response and stress-tests the reasoning: logical fallacies, missing perspectives, unsupported claims, framing bias, cognitive distortions. Two models trained differently disagree more, which produces a more independent assessment. Within Settings, there is a Thinking Reference prompt that shapes how the critique model evaluates. I include a simple default — update it with your own mental models.

### Crucible

As your library grows, you will find documents whose summaries are almost identical, missing the subtle differences between them. The Crucible finds these pairs and asks the AI to rewrite both summaries to encode where they agree and where they diverge. The subtle difference between documents matters more than the similarity. This feature surfaces it.

### Writing Workflow

Writing improves my thinking more than any other process I have tried. The original need for this app was to help with the research phase of writing. After using it for a few weeks, I built my writing process directly into the app. This workflow works for me — feel free to skip it.

**Step 1 — Grill**
Start with a rough thesis. The AI runs Socratic dialogue, pushing back until we reach shared understanding: a core claim, essential context, and the outcome you want the reader to have. Only then does the research phase unlock.

**Step 2 — Research**
The thesis decomposes into research themes. Each theme searches your knowledge base and optional web sources. The output is a structured research brief: key insights, sub-themes, contradictions, writing prompts. You can add more themes if needed.

**Step 3 — Draft**
A clean rich-text editor. No AI assistance. Research briefs sit one click away. I stopped asking AI to write the initial draft — I got stuck inside its structure. You write. You decide the shape.

**Step 4 — Review**
Submit the draft for a cognitive audit. The AI checks the foundational argument, surfaces your biases, tests replicability, and applies strict style rules. You get specific feedback, not praise.

**Step 5 — Iterate**
I require three review cycles before the essay is done. The first draft always has blind spots the writer cannot see. Three honest cycles is where the improvement flattens. You can do more — the minimum is three.

**Step 6 — Style Guide**
Final check against your own writing rules. Define your rules in Settings → Prompts → Style Guide. The AI flags every violation. Fix what needs fixing, then finalise.

---

## AI Providers

The app supports Ollama (local and cloud), Claude, OpenAI, Gemini, and any OpenAI-compatible endpoint. You configure three models independently:

- **Chat Model** — your daily workhorse
- **Critique Model** — a second model (preferably more capable) for review and critique
- **Embedding Model** — for generating document vectors

Web search works with Ollama, Anthropic, OpenAI, Gemini, and OpenRouter.

Your API keys stay on your machine, stored in the local SQLite database.

---

## Data

All data lives in a SQLite database at `~/Library/Application Support/com.secondbrain.tauri/`. No data goes to any server except the AI provider you choose. If you have a capable local machine, you can run everything through Ollama with no external calls at all.

---

## Download

The app currently builds for macOS only. Download the latest `.dmg` from [Releases](https://github.com/kiran-brahma/second-brain-releases/releases).

I release updates every two to three weeks as I use the app daily and find issues. Each release includes a log of changes.

---

## Installation

1. Open the `.dmg` and drag **Second Brain.app** to Applications.
2. Run this once in Terminal to remove the macOS quarantine flag:
   ```bash
   xattr -d com.apple.quarantine "/Applications/Second Brain.app"
   ```
3. Launch normally.

The app is not notarized with an Apple Developer certificate. The `xattr` command is a one-time step. All subsequent updates install automatically inside the app — no repeat required.

---

## Updates

The app checks for updates on launch. When a new version is available, a banner appears in the bottom-right corner. Click **Install** — the app updates and relaunches.

---

## Default Prompts

The app ships with default prompts for each AI function. All prompts are editable in Settings → Prompts. The defaults live in the [`prompts/`](./prompts/) folder in this repo — copy, adapt, or replace them with your own.

| Prompt | Key | Notes |
|--------|-----|-------|
| [Critical Analysis](./prompts/critical-analysis.md) | `critical` | How the critique model stress-tests reasoning |
| [Thinking Reference](./prompts/thinking-reference.md) | `thinking` | Analytical framework injected during critique |
| [Grill System](./prompts/grill-system.md) | `grill_system` | Socratic editor persona for Phase 1 |
| [Review System](./prompts/review-system.md) | `review_system` | Four-audit cognitive reviewer for Phase 4 |
| [Research System](./prompts/research-system.md) | `research_system` | Research synthesiser for Phase 2 |
| Style Guide | `style_guide` | Empty by default — define your own writing rules |

---

## Tips

- Write documents in your own words where possible. When you paste someone else's content, add a 200–350 word section at the end with your own understanding and what you took from it. The AI gets a much better picture of your thinking when your voice is in the document.

- I read on Kindle and highlight as I go. Once done with a book, I review the highlights, group them by theme, and write a short summary in my own words under each theme. I upload this as a single document. One document per book, structured by theme, with your voice in it.

- The Chat Persona and Document Summary prompts work as additions to the core prompt — write only what you want to add, not a full replacement. Every other prompt is a full replacement. Change them.

---

## About

I am not a software engineer. I have never built a production app before this. Everything I build for personal use runs with AI tools — I use Claude Code, Codex, and Pi. The codebase reflects that. I run regular code quality passes to clean up AI slop, but things slip through.

I built an ERP tool for my business (in production since March 2026) and document my learning on [The Operator Stack](https://www.theoperatorstack.com/themes/building-o9x).

The source code is private. This repo hosts releases and the update manifest for the built-in updater.
