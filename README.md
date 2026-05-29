# Second Brain

We all have our notes spread across multiple apps such as Obsidian, Notion or Apple Notes (whatever app that you fancy). However, the biggest problem I faced was finding the right note at the right moment. AI can make life simpler but it can also complicate the process. People talk a lot about RAG and how you can use it for your benefits. While RAG certainly helps but then I found the translation layers loses it. I started thinking on how to build a simple app based on how I search and look for ideas or knowledge across my own personal notes. The Second Brain is my way to use my notes more effectively. Feel free to explore to see if it meets your needs. This a work in progress and will keep updating it on a weekly basis (based on issues I will find during its usage)

## What it does

Second Brain is my personal take on RAG and drawing lessons from Contextual RAG idea that Anthropic released. Traditional RAG breaks a document down into multiple parts and then when we query it, it searches and gives AI the required context to answer your query. Traditional RAG does have its issues so Anthropic tried to fix it via Contextual RAG for longer documents. 
Rather than using the Contextual RAG appraoch, I approached the issue in a different manner. Every document I store within my knowledge base is sumamrised (using AI) and rather then entire document, I embedd the Summary Document. I include the following sections within the summary document:
- Scope: What is this document about
- Three to Five Core questions that this document can answers
- Key Claims made by the document
- Key Concepts explained within the document
- Connections: Identify connections with other ideas (which AI is trained on) to include


## How does AI Search Work

### Documents
Feel free to store any articles, notes, research papers, YouTube Transcipts, Book Notes in your document Library (currently supports only Text). Once a document is added, AI creates a summary documnent as explained above. Review the summary and embedd the document. You can use Ollama for embedding in most systems (I use [nomic](https://ollama.com/library/nomic-embed-text)) or cloud models based on your needs. Initial summary documents are not great and it will need some fixes from your end to make it work.

### Chat 
When you ask AI a question, there is a pre-processing phase, where AI breaks down your query into a granualar search queries (which you can review and add more if needed). Once the queries are finalised, it retrieves the most relevant documents from your knowledge base using hybrid vector + keyword search. It then shortlists the key documents and then each and every document is sent to AI as context (not the AI summary). AI then asnwers your query based on complete context rather than just sinppets. This works as new AI models context window has expanded (you will need to use cloud models). 

### Search
A lot of times, I have a query and just want to read more on the topic and not get AI to answer me. In such cases, I do a simple search and AI does the same as above but gives me the list of documents to read on my own.

## Additional Features
Initially, I built it for simple RAG search but over a period of few weeks, I added more features based on my needs

### Review queue
A simple spaced-repetition review for the documents you save. An automated review schedule ensures you can recall what you read and saved into your knowledge. It asks you a bunch of questions to help you understand if you recall what you read else, its time to brush up on them again

## Critiques
AI is wrong more often than we know but it can also self-correct itself when asked. Once you have arrived at a final response that appears reasonable, its good to get an audit. I have a critique function, where I initiate a new AI chat session (with another model) to review the response and provide feedback. Within the settings options, there is an option to include on how to evalute based on certain patterns (Thinking Reference). I include a simple reference document but I highly recommend updating it with your own. Each model is trained in similar fashion but quite different. This ensures you get an independent assessment. Explore different models and use the ones that makes more sense. 

### Crucible
As you add more documents within your library, you will find document summaries that are almost similar, missing the subtle differennces between them. This features find such documents and gets AI to add additional context on why they differ. The subtle differences between each documents allows the AI to provide a response that is more grounded to your library and not miss the minor diffreence (details do matter)

### Writing workflow
Over the past few years, I have understood benefits of writing. Writing improves my thinking and ideas much better than any other process I have experimented. The whole need for this app was to aid me in my research phase during my writing. After using this app for two weeks, I decided to incorporate my own writing process within the app itself. The workflow works for me but feel free to ignore

I broke down the entire writing process into six different stages:

**Step 1: Grill**
I provide a basic thesis on what I wish to write about. I use AI to grill me on my thesis till we both agree on a shared understanding. The goal is to generate the following:
- Core Claim
- Context
- Future Outcome
Once this is established, we move onto the next phase

**Step 2: Research**
Once the thesis is established in Step 1, AI breaks it down into research themes and searches across your knowledge base (even online if required). For each theme, it generate a clean research brief that serve as the context for the essay. You can add more themes if you feel the need to do so.

**Step 3: Draft**
A simple text editor where you start writing. No AI assistance is included. The research briefs are availale for review. A year ago, I used to ask AI to write the initial draft but over time realised, I was stuck within the framework of the essay it wrote. Hence, you structure it the way you want.

**Step 4: Review**
Once I have a draft in place, get AI to review your writing. AI uses the critique model to review and provide you with feedback. You are even provided with a list of changes to help improve the draft. 

**Step 5: Iterate**
Over time, I realised it requires around 3-4 such reviews before I can say its good. Hence, I force a min of 3 such reviews before I move onto the next stage. The focus is more on structure, flawed thinking and other errors that we usually make. You can undertake as many reviews as you want but improvement scope do drop once we do a honest 3 cycle review. 

**Step 5: Style Guide**
I have built a list of writing rules that I wish to enforce across everything I write. In this stage, I ask AI to review my final essay against my Style guide. You can define your own style guide within settings. 

## AI providers

Initially, I built it to work with Ollama (Cloud and local) but added other AI providers. You need to add the following information:
- Chat Model: Your daily workhorse model
- Critique Model: Secondary model (preferrable more capabale) to help review across your drafts and responses
- Embedding Model: The model you use for generating the embeddings for RAG
- Web Search: Ollama supports Web search and its enabled within the app. Anthropic, OpenAI, Gemini and Openrouter also have specific tools for Web search which is included within the app. 


## Data

All data lives in a SQLite database on your machine at (On Mac its available at `~/Library/Application Support/com.secondbrain.tauri/`). No data is sent to any server except to the AI provider you choose. If you have a beefy machine, you can run the entire app locally via Ollama or define your own OpenAI compatible API end points. 

## Download
The app is currently built for usage only on Mac. You can go to [Releases](https://github.com/kiran-brahma/second-brain-releases/releases) and download the latest `.dmg`.

The app is still in development stage and will have lot of issues. I use a mac so my focus is to use the app daily and fix. Once every two or three weeks, I will update the app. I will include a log, explaining changes done. Once I feel the app is stable, I may release the windows version

### Development
The core app is built on Nextjs and I use Tauri to develop the Mac DMG. I will use the same Tauri to build the apps across other platforms. I am not a Software engineer by profession and I have never built any apps. Every app I build for my internal use is with AI tools. I use Claude Code, Codex and Pi as my tools of choice. Hence, there can be lot of AI slop within my codebase. I undertook a lot of fixes by using the below skills to improve the code:

- [Improve Codebase Architecture](https://github.com/mattpocock/skills/tree/main/skills/engineering/improve-codebase-architecture)
- [DeSlop](https://github.com/cursor/plugins/tree/3347cbab5b54136f6fba0994c3a01a56f7fb7fca/cursor-team-kit/skills/deslop)
- [Code Quality Review](https://github.com/cursor/plugins/tree/3347cbab5b54136f6fba0994c3a01a56f7fb7fca/cursor-team-kit/skills/thermo-nuclear-code-quality-review)

Every change I make, I follow the below process:
- Step 1: Use Codebase architecture skill to plan out the changes. Once the entire planning is completed, I create a PRD using [to-prd skill](https://github.com/mattpocock/skills/tree/main/skills/engineering/to-prd)
- Step 2: I ask either Claude Code or Codex to build. I build tests for each feature before I start coding and only once all tests are passed, I consider as intial work completed
- Step 3: Use the Deslop Skill to review all changes made
- Step 4: I undertake the code quality review
- Step 5: Build the app locally and test out everything myself
- Step 6: Commit the changes

I am still learning how to build apps with AI. I have built an ERP tool for my business (in production since March 2026) and [document my learnigns in my blog](https://www.theoperatorstack.com/themes/building-o9x)


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

## Tips
- Ideally, write the documents in your own words than just copy-paste. However, its not possible in every screnario, so at the end of each document, I add a 200-350 word section where I document my own learnings and understanding. This ensures that AI gets a better understanding of your own thinking. 

- I read a lot of books in Kindle and highlight religiously as I am reading. Once I am done reading a book, I review my highlights (adding few more lines or removing). Once done, I export it as a markdown file and then I groups them into different themes. Under each themse, I write in my own words a key summary for each theme ([Literature Note](https://forum.obsidian.md/t/what-is-a-literature-note/39353)). I upload this document into my knowledge base. Feel free to explore what works best for you

- Apart from Chat Persona and Document Summary prompt, feel free to change every other prompt used across the app. I include simple prompts to get started but I highly recommend changing them

