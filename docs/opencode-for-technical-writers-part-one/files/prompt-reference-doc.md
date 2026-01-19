You are helping a TECHNICAL WRITER produce a PARAGRAPH-ORIENTED REFERENCE DOCUMENT for an unfamiliar repository.

The technical writer:
- Understands general software concepts (services, CLIs, APIs, config, tests, deployment).
- Does NOT know this project’s domain, internal terminology, or architecture.
- Needs a reference doc that explains how the repository’s pieces fit together, so they can:
  - Answer “how does this work?” questions quickly.
  - Write accurate quickstarts and how-tos later.
  - Identify gaps and the right SME questions.

The reader:
- Is a developer/operator new to this project who needs a solid mental model.
- Wants clear explanations and concrete pointers to where things live in the repo.

====================
GOAL
====================

Write a REFERENCE document in Markdown that gives a clear, structured understanding of:
- The system’s main responsibilities.
- The major components/modules and how they interact.
- The primary runtime flows (startup, request/response, background jobs, etc.).
- The configuration surface and operational concerns.
- The boundaries: what this repo covers vs what is external or assumed.

This document is NOT a quickstart and NOT a tutorial. It should explain how things work together and where to look, not guide the user through a single task.

====================
SCOPE AND SOURCES
====================

- Use ONLY information available in this repo (and submodules) unless the prompt explicitly allows other sources.
- Do NOT invent commands, flags, env vars, ports, endpoints, file paths, or component names.
- If something is not supported by the repo, say "Unknown" and list what evidence is missing.
- If you infer something from code structure, label it clearly as "Inferred" and explain your reasoning.

Evidence requirement (lightweight but real):
- For each major claim (component purpose, entry points, key flows, config keys), cite evidence as:
  - File path(s)
  - A short excerpt (1–2 lines) OR a brief paraphrase of what the file shows
- Do not overquote. Keep excerpts short.

====================
STYLE REQUIREMENTS
====================

Paragraph-first:
- This document must be paragraph-oriented.
- For every section, start with 1–3 paragraphs explaining the concept in plain language.
- Use bullet lists only for short enumerations (3–7 items) when scanning is genuinely useful.
- Do not write “outline-like” content made mostly of bullets.

Clarity and onboarding tone:
- Assume the reader has zero context about the project and its domain.
- Define domain-specific terms on first use (2–4 sentences).
- Avoid marketing language and filler. Every paragraph must add understanding.

Formatting constraints:
- Output Markdown only.
- Use exactly one H1 title at the top.
- Use an H4 subtitle directly below the title (one sentence, ends with a period).
- After that, use only H2 and H3 headings (no H4–H6 beyond the subtitle).
- Use '-' for bullet lists.
- Use straight quotes only (no curly quotes).
- Avoid a "Further reading" section at the end.

Title guidance:
- Use sentence case.
- Prefer cue words like "Guide to", "About", or "Reference for".
- Avoid punctuation in the title (no colon/dash).

Optional visuals:
- If relationships or flows are hard to understand in prose, include ONE simple Mermaid diagram where it adds real clarity.
- Keep diagrams small and directly tied to the section’s idea.

====================
TASK
====================

1) Establish what this repo is (scope and audience)
- In 2–4 paragraphs, explain what this repo contains and who it is for (developer/operator/end user).
- Clarify what it does NOT contain if that boundary is visible (e.g., depends on other repos, external services).
- Cite evidence.

2) Map the repo into functional areas
- Create a high-level map of the top-level directories and key files.
- Do NOT just list folders; explain what each area is responsible for and how it contributes to the system.
- Keep the map readable: use short paragraphs with occasional short bullet lists.
- Cite evidence for key areas and entry points.

3) Explain the system “as running software”
- Identify the main runtime components and how they interact.
- Cover these (only if they exist in the repo; otherwise mark Unknown):
  - Primary process(es) and entry point(s)
  - Background workers / schedulers
  - Networking surface (HTTP/gRPC/p2p/etc.)
  - Storage layer(s) (DB, filesystem, object store, etc.)
  - Dependencies (other services, external APIs)
- For each component, explain:
  - What it does
  - What it depends on
  - How to find it in the repo
- Cite evidence.

4) Describe the core flows (how requests/jobs move through the system)
- Pick 2–4 of the most important flows (based on what the repo suggests). Examples:
  - Startup and initialization flow
  - Handling a typical request/message
  - A background job or sync loop
  - A data write/read lifecycle
- For each flow:
  - Write a short narrative: what triggers it, what happens step by step, what the output is.
  - Mention which modules/files implement each step.
  - If exact details are unclear, mark Unknown and suggest what to verify.
- Include one Mermaid diagram if it significantly clarifies the flow.

5) Configuration and operational surface
- Explain how the system is configured:
  - Config files
  - Environment variables
  - CLI flags
  - Defaults
- Describe where configuration is defined and how it is loaded (with evidence).
- Include operational concerns if visible:
  - Logging
  - Metrics/health checks
  - Error handling
  - Deployment assets (Docker/Kubernetes/Terraform/CI)
- Do not invent ports or env vars; cite what exists.

6) "The basics" section (required)
- Add an H2 heading with the exact text: "The basics"
- Under it, include exactly three bullet items (each one complete sentence ending with a period) summarizing:
  - The main purpose of the system
  - The primary interaction surface(s)
  - The most important moving part or dependency
- No code blocks or callouts in this section.

7) Interfaces and extension points (developer-focused reference)
- Explain how a developer typically extends or integrates with this project:
  - Public APIs/modules
  - Plugin systems/hooks (if any)
  - Where to add features
  - Testing strategy as it relates to correctness of behavior
- Be concrete: point to directories, patterns, and key files.
- Cite evidence.

8) Known gaps and SME questions (reference-oriented)
- Identify the top documentation gaps that prevent this reference doc from being fully confident:
  - Missing architecture explanation
  - Unclear boundaries between repos
  - Missing description of critical flows
  - Undocumented configuration keys
- Provide 5–10 SME questions that would close the biggest uncertainties.
- Phrase questions so a tech writer can paste them into an issue or chat.

====================
OUTPUT STRUCTURE
====================

Produce a single Markdown document with this structure:

# <Title>
#### <Subtitle sentence.>

<Intro paragraphs: what this is, who it is for, what it covers.>

## The basics
- <Sentence 1.>
- <Sentence 2.>
- <Sentence 3.>

## Repository map and responsibilities
<Paragraphs explaining key directories and files, with evidence.>

## How the pieces fit together
<Paragraphs describing major components and interactions, with evidence. Optional small Mermaid diagram.>

## Core flows
### <Flow 1 name>
<Paragraph narrative + evidence.>
### <Flow 2 name>
<Paragraph narrative + evidence.>
(Include up to 4 flows total.)

## Configuration and operations
<Paragraphs explaining config surfaces and operational concerns, with evidence.>

## Interfaces and extension points
<Paragraphs on APIs/modules/integration/testing as relevant, with evidence.>

## Gaps and questions for SMEs
<Paragraph + short lists of gaps and questions.>

Throughout the document:
- Keep it paragraph-oriented.
- Use bullets sparingly and only when scanning benefits.
- Ground key statements with file paths and brief evidence.
- Mark Unknown items explicitly and suggest how to verify.
