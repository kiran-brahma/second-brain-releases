# Second Brain

My notes lived across Obsidian, Notion, and Apple Notes. The problem was never storage. The problem was finding the right note at the right moment. I tried standard RAG tools and found something gets lost in the translation. I built Second Brain to search my own knowledge the way I actually think. This is a personal tool I use daily. I update it as I find issues.

---

## The Core Idea

Standard RAG breaks documents into chunks, retrieves the chunks, and hands them to the AI. Chunks lose context. The AI gets fragments.

I took a different approach, drawing from Anthropic's [Contextual RAG](https://www.anthropic.com/research/contextual-retrieval). Every document I store gets an AI-generated summary with five sections:

- **Scope** — what this document covers
- **Questions** — three to five core questions this document answers
- **Key Claims** — the main arguments the document makes
- **Key Concepts** — the central ideas explained within it
- **Connections** — links to broader ideas the AI knows

The system embeds this summary, not the raw document. Search retrieves the summary to find the right document. The AI then reads the full document text as context. This gives the AI complete information rather than isolated snippets.

---

## Features

### Chat

Ask a question in natural language. The AI breaks the query into precise search queries, which you can review and adjust before running. It runs a hybrid vector and keyword search, retrieves the most relevant documents, and synthesises a cited answer grounded in your own knowledge. The AI reads full documents, not summaries.

### Search

When I want to read rather than ask, I use Search. It runs the same retrieval process but returns a list of documents. No AI answer. Just my notes, surfaced.

### Document Library

Store articles, notes, research papers, YouTube transcripts, book notes. Any text. Once added, the AI generates the structured summary above. Review it, fix what needs fixing, and embed it. The summary quality determines retrieval quality. Initial summaries need editing. That editing pays off in every future search.

You can link documents to each other. When chat retrieves a linked document, its connected documents surface with it. More links mean richer context.

### Review Queue

Spaced-repetition review built into the library. The system schedules documents before you forget them, based on your recall ratings. It generates questions to test whether you actually remember what you read. Ten documents reviewed daily compounds faster than 70 reviewed once a week.

### Critiques

After any AI response, run a critique. A second AI model reads the response and stress-tests the reasoning: logical fallacies, missing perspectives, unsupported claims, framing bias, cognitive distortions. Two models trained differently disagree more. That disagreement is the point.

Within Settings, there is a Thinking Reference prompt that shapes how the critique model evaluates. I include a simple default. Update it with your own mental models.

### Crucible

As the library grows, document summaries start to look similar, missing subtle differences between them. The Crucible finds these pairs. The AI rewrites both summaries to encode where they agree and where they diverge. The difference is what matters for good retrieval. This feature makes the difference visible.

### Writing Workflow

Writing improves my thinking more than any other process I have tried. The original need for this app was the research phase of my writing. After two weeks of use, I built my writing process directly into the app. This workflow works for me. Skip it if it does not fit yours.

**Step 1: Grill**

Start with a rough thesis. The AI runs Socratic dialogue, pushing back until we reach shared understanding: a core claim, essential context, and the outcome I want the reader to have. The research phase does not unlock until this is settled.

**Step 2: Research**

The thesis decomposes into research themes. Each theme searches the knowledge base and optional web sources. The output is a structured research brief: key insights, sub-themes, contradictions, writing prompts. You can add more themes.

**Step 3: Draft**

A clean rich-text editor. No AI assistance. Research briefs sit one click away. I stopped asking AI to write the first draft. I got stuck inside the structure it chose. You write. You decide the shape.

**Step 4: Review**

Submit the draft for a cognitive audit. The AI checks the foundational argument, surfaces bias, tests replicability, and applies strict style rules. The feedback is specific. There is no praise.

**Step 5: Iterate**

Three review cycles before the essay is done. This is a hard minimum, not a suggestion. First drafts have blind spots the writer cannot see. Three honest cycles is where improvement flattens. You can do more.

**Step 6: Style Guide**

Final check against your own writing rules. Define your rules in Settings → Prompts → Style Guide. The AI flags every violation. Fix what needs fixing, then finalise.

---

## AI Providers

The app works with Ollama (local and cloud), Claude, OpenAI, Gemini, and any OpenAI-compatible endpoint. You configure three models independently:

- **Chat Model** — your daily workhorse
- **Critique Model** — a second model, preferably more capable, for review and critique
- **Embedding Model** — for generating document vectors

Web search works with Ollama, Anthropic, OpenAI, Gemini, and OpenRouter. Your API keys stay on your machine, stored in a local SQLite database. If you have a capable local machine, run everything through Ollama with no external calls.

---

## Data

All data lives in a SQLite database at `~/Library/Application Support/com.secondbrain.tauri/`. No data goes to any server except the AI provider you choose.

---

## Download

The app builds for macOS only, for now. Download the latest `.dmg` from [Releases](https://github.com/kiran-brahma/second-brain-releases/releases).

I use this daily. I release updates every two to three weeks as I find issues. Each release includes a log of changes. A Windows build is possible once the macOS version is stable.

---

## Installation

1. Open the `.dmg` and drag **Second Brain.app** to Applications.
2. Run this once in Terminal:
   ```bash
   xattr -d com.apple.quarantine "/Applications/Second Brain.app"
   ```
3. Launch normally.

The app is not notarized with an Apple certificate. The `xattr` command removes the quarantine flag. It is a one-time step. All subsequent updates install automatically inside the app.

---

## Updates

The app checks for updates on every launch. When a new version is available, a banner appears in the bottom-right corner. Click **Install**. The app updates and relaunches.

---

## Default Prompts

All prompts ship with defaults and are editable in Settings → Prompts. The defaults live in the [`prompts/`](./prompts/) folder in this repo.

| Prompt | Key | Purpose |
|--------|-----|---------|
| [Critical Analysis](./prompts/critical-analysis.md) | `critical` | How the critique model stress-tests reasoning |
| [Thinking Reference](./prompts/thinking-reference.md) | `thinking` | Analytical framework injected during critique |
| [Grill System](./prompts/grill-system.md) | `grill_system` | Socratic editor for Phase 1 |
| [Review System](./prompts/review-system.md) | `review_system` | Four-audit cognitive reviewer for Phase 4 |
| [Research System](./prompts/research-system.md) | `research_system` | Research synthesiser for Phase 2 |
| Style Guide | `style_guide` | Empty by default. Define your own writing rules. |

---

## Tips

Write documents in your own words where possible. When you paste someone else's content, add a 200 to 350 word section at the end with your own understanding of what you took from it. The AI builds a better picture of your thinking when your voice is in the document.

I read on Kindle and highlight as I go. Once done with a book, I review the highlights, group them by theme, and write a short summary in my own words under each theme. One document per book, structured by theme, with my voice throughout.

Apart from Chat Persona and Document Summary, every other prompt is a full replacement. The defaults are starting points. Change them to match how you think.

---

## About

I am not a software engineer. I have never built a production app before this. Everything I build runs with AI tools. I use Claude Code, Codex, and Pi. The codebase reflects that. I run regular code quality passes to clean it up, but things slip through.

I built an ERP tool for my business, in production since March 2026, and document my learning on [The Operator Stack](https://www.theoperatorstack.com/themes/building-o9x).

The source code is private. This repo hosts releases and the update manifest for the built-in updater.
