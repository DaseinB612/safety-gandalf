# Safety Gandalf 🧙

**An AI research navigator that points you to the exact argument, in the exact paper, that answers your question.**

---

## The Problem

AI safety has a navigation problem. The most important arguments in the field are buried inside dense papers, buried inside longer works, buried inside reading lists that assume you already know where to look. General-purpose AI assistants give you summaries. What you actually need is someone to say: *"You're asking about deceptive alignment — read Evan Hubinger's 'Risks from Learned Optimization', specifically section 3.2."* That tool does not exist yet.

---

## Proposed Solution

Safety Gandalf is a research navigator purpose-built for AI safety. Ask it a question in plain English and it returns not just a paper, but a specific section and the specific argument that answers you. It indexes the canonical sources in the field — arXiv, the Alignment Forum, LessWrong, key technical reports — and treats them as a structured knowledge base rather than a search corpus. The goal is precision, not breadth.

---

## Architecture Overview

At a high level, Safety Gandalf works in three layers:

1. **Indexing** — a scraper pulls papers and posts from target sources, chunks them into sections and arguments, and stores them with metadata (author, date, source, section heading)
2. **Embedding** — each chunk is converted into a vector embedding so that semantic similarity search can match a user's question to the most relevant passage, not just keyword matches
3. **Retrieval + Response** — when a user asks a question, the system finds the top matching chunks and returns them with full citations: paper title, author, section, and a direct quote or summary of the argument

---

## Data Sources

- arXiv (cs.AI, cs.LG, stat.ML)
- Alignment Forum (alignmentforum.org)
- LessWrong (lesswrong.com) — filtered for AI safety content
- Key technical reports: ARC Evals, Anthropic, DeepMind Safety, OpenAI
- JSTOR (where accessible) for philosophy of mind and ethics adjacent work
- 80,000 Hours AI safety syllabus as a curated seed list

---

## Known Challenges

- **Access and licensing** — JSTOR and some publishers restrict scraping; open-access sources are safer to start with
- **Chunking quality** — splitting a paper into meaningful argument-level chunks is harder than splitting by paragraph; section headers help but are inconsistent across sources
- **Keeping it current** — the field moves fast; the index needs regular updates or a live feed
- **Hallucination risk** — the system must cite real passages, not confabulate them; retrieval-augmented generation (RAG) helps but needs careful validation
- **Scope creep** — AI safety adjacent content (philosophy, ML theory, policy) is vast; the tool needs clear boundaries to stay useful

---

## Why This Matters

The AI safety field is growing faster than its onboarding infrastructure. Researchers, policymakers, and newcomers all face the same problem: knowing that the answer exists somewhere, but not knowing where. Every hour spent navigating reading lists is an hour not spent thinking. Safety Gandalf is infrastructure for the field — a tool that makes collective knowledge actually navigable. Given the stakes of the problems the field is working on, that is not a minor convenience.

---

## How to Build It

A builder picking this up could start here:

1. **Choose a RAG framework** — LlamaIndex or LangChain are the most accessible starting points
2. **Build a scraper** — start with Alignment Forum and LessWrong via their APIs, then add arXiv
3. **Chunk and embed** — use section-level chunking, embed with OpenAI `text-embedding-3-small` or an open-source equivalent
4. **Store in a vector database** — Pinecone, Weaviate, or Chroma all work; Chroma is easiest for local development
5. **Build a simple query interface** — a basic chat UI or even a CLI is enough for a first version
6. **Validate citations** — every returned result must include a verifiable source link; build this check in from the start

The MVP is: user types a question, gets back three results, each with paper title, author, section, and a one-sentence summary of the argument. That alone would be genuinely useful.

---

## Personal Note

I built this proposal because I needed this tool and it didn't exist. Breaking into AI safety as an outsider means spending weeks just figuring out what to read, in what order, and why it matters. Safety Gandalf is what I wish someone had handed me on day one. If you build it, let me know.
