Keivan Kalantari — AI Professional Portfolio
Project Reference / Maintenance Guide

Last known working architecture: September 2026

1. Project purpose

A recruiter-facing professional portfolio website with an AI assistant.

The website allows recruiters to ask questions about Keivan Kalantari's professional experience. The AI answers using a controlled professional knowledge base rather than relying on general model knowledge.

Primary objective:
Provide accurate, recruiter-friendly answers about Keivan's experience while minimizing hallucination.

2. Live components
🌐 Public website

https://keivan74.github.io

GitHub repository:

GitHub repository — keivan74/keivan74.github.io

The website is a GitHub Pages site. GitHub Pages publishes the repository's HTML/CSS/JavaScript as the live website.

📚 Professional knowledge base

knowledge.md — live knowledge source

THIS IS THE SINGLE SOURCE OF TRUTH FOR PROFESSIONAL CONTENT.

When Keivan's professional information changes, update knowledge.md.

Examples:

new job
new project
new certification
new technology
new achievement
new responsibility
updated experience
corrected information

Normally, no Worker or website code change is required.

⚙️ Cloudflare Worker

Worker name:

keivan-ai

Worker endpoint:

https://keivan-ai.keivan-kalantari.workers.dev/

Cloudflare dashboard:

Cloudflare Dashboard

The Worker is the backend/API between the website and the AI.

Architecture:

Recruiter
   ↓
GitHub Pages website
   ↓
Cloudflare Worker: keivan-ai
   ↓
fetch knowledge.md
   ↓
Cloudflare Workers AI / Llama
   ↓
Answer
   ↓
Website

The Worker uses a Cloudflare Workers AI binding available as env.AI.

3. Current AI model

Current model configured in the Worker:

@cf/meta/llama-3.2-3b-instruct

Current generation limit:

max_tokens: 2048

The Worker prompt instructs the model to:

use only knowledge.md
avoid inventing experience
be accurate with dates and numbers
calculate employment duration when appropriate
say when information is not specified
answer professionally
not mention the internal knowledge file
4. IMPORTANT — What to change when updating the project
🟢 Professional information changes

Change only:

knowledge.md

Do not modify:

index.html
Cloudflare Worker
AI configuration

unless there is a separate reason.

🟡 Website appearance/function changes

Modify:

index.html

Examples:

colors
layout
fonts
images
buttons
wording
additional website sections
chat interface

The current website is essentially a single index.html containing HTML, CSS and JavaScript.

🔴 AI behavior changes

Modify:

Cloudflare Worker: keivan-ai

Examples:

change AI model
change system prompt
change token limit
add authentication
add logging
add rate limiting
change how knowledge is retrieved
introduce a real RAG/vector database later
5. Current knowledge architecture

The project does NOT currently use Cloudflare AI Search.

An earlier AI Search/RAG implementation was tested but abandoned because the results were unreliable.

Do NOT accidentally reintroduce:
Cloudflare AI Search
vector search
reranking
AI Search chat endpoint
the old AI Search public endpoint

The current design intentionally uses the simpler approach:

knowledge.md
      ↓
Cloudflare Worker
      ↓
Llama

This simplicity is intentional.

6. Authoritative professional source

The knowledge base was created from the authoritative:

Keivan Kalantari — Senior Automotive Engineering Resume — 2026-09 AI Edition

For future resume/knowledge updates:

Do not use the old "008 Resume" or previous resume versions as the authoritative source.

If a new authoritative resume is supplied in the future, rebuild/update knowledge.md from that new source.

7. Current repository structure

At the working state:

keivan74.github.io/
│
├── index.html
├── knowledge.md
└── README.md
index.html

Website + frontend JavaScript.

knowledge.md

Professional AI knowledge base.

README.md

Repository documentation.

8. Important URLs
Purpose	Location
Live website	keivan74.github.io
GitHub repository	GitHub repo
AI knowledge	knowledge.md
AI Worker	keivan-ai Worker
Cloudflare Dashboard	Cloudflare
GitHub Pages documentation	GitHub Pages Docs
Cloudflare Workers AI documentation	Workers AI Docs
9. Troubleshooting order

If the AI stops working, do not immediately redesign the system.

Check in this order:

1. Website

Open:

https://keivan74.github.io

Can the website load?

2. Knowledge

Open:

https://keivan74.github.io/knowledge.md

Does the current knowledge appear?

3. Worker

Check:

keivan-ai

Is it deployed?

4. Worker → AI

Check that the Worker still has its Workers AI binding named:

AI

Cloudflare's documentation confirms that the binding is exposed to Worker code as env.AI.

5. Only then change code.
10. Golden rule for future AI assistance

If this document is given to an AI assistant in the future, it should understand:

Do not rebuild this project from scratch. First inspect the existing architecture and preserve the working separation between the website, knowledge.md, and the Cloudflare Worker.

And:

For professional-content updates, modify knowledge.md only unless the user explicitly asks to change the application itself.

11. Current known-good state

As of September 2026, the system has successfully demonstrated:

Website → Worker communication
Worker → knowledge.md
knowledge.md → Llama
Correct GM employment calculation
Automotive systems questions
HIL experience questions
Patent questions
Refusal to invent unsupported aerospace experience
2048-token response limit

Status: WORKING BASELINE
