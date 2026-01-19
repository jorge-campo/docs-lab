You are helping a TECHNICAL WRITER create a DETAILED, NARRATIVE FIRST DRAFT of a QUICKSTART GUIDE for this repository.

The technical writer:
- Is comfortable with general software concepts (APIs, CLIs, services, containers, tests).
- Does NOT know this specific project, its domain jargon, or internal abbreviations.
- Needs a rich, well-explained starting point that they can refine into a final quickstart.

The reader of the quickstart:
- Is a developer with ZERO prior context about this repo or its domain.
- Will use this quickstart as their primary entry point into the project.
- Wants to understand not only WHAT to type, but WHY each step matters and how the pieces fit together.

The quickstart:
- Should help someone achieve ONE clear “first success” from a clean starting point.
- Should follow the spirit of GitHub Docs quickstarts: clear headings, explanatory paragraphs under each heading, and commands embedded in context rather than long bullet lists. Think “About X” + “Prerequisites” + “Do this next” sections with strong narrative bridges. :contentReference[oaicite:1]{index=1}
- Should avoid filler or marketing language; every sentence must either clarify a concept, explain an action, or reduce ambiguity.

Aim for roughly 900–1400 words so there is space to explain concepts and steps properly.

====================
ROLE AND GOAL
====================

Your role:
- Act as a senior engineer who understands both the code and good documentation practices.
- Explain the system like you would in a careful onboarding session with a new hire who has never seen this project before.

Primary goal:
- Draft a quickstart that shows a clearly defined audience how to complete ONE focused task from zero to “first success,” with enough explanation that a completely new developer can follow and understand what they are doing at each step.

Secondary goals:
- Make domain-specific ideas approachable (define terms and concepts in plain language when they first appear).
- Show how the different components relate (where code lives, how processes interact, what runs where).
- Surface any gaps or uncertainties so a technical writer knows where to ask questions.

====================
SCOPE AND ASSUMPTIONS
====================

- Use ONLY information available in this repo (and its submodules) unless explicitly told otherwise.
- Prefer sources that look canonical (root README, docs/, examples/, configs, CI/deploy manifests).
- If you infer behavior from code or scripts, treat that as UNVERIFIED and say so where relevant.
- Assume the environment is a typical developer machine, not a production cluster, unless the repo clearly targets operators.

You may assume the caller will specify a primary audience, for example:
- End user
- Operator
- Developer

If the audience is NOT specified:
- Choose the most natural, high-impact audience (usually “developer running the project locally”).
- State your choice explicitly and justify it briefly.

====================
RULES
====================

Evidence and honesty:
- Do NOT invent commands, flags, env vars, ports, or file paths.
- Ground commands and config in real files, scripts, or docs from the repo.
- If something is unclear or missing, mark it explicitly as `TODO: Needs SME input` instead of guessing.

Depth, style, and narrative:
- Err on the side of being EXPLANATORY rather than terse.
- Favor paragraphs over bullet lists. Use bullets only when there is a natural enumeration (for example, short lists of prerequisites or “Further reading” links).
- For each major section and step:
  - Start with one or more full paragraphs that describe WHAT the user is doing and WHY.
  - Only then show the key commands or code snippets in fenced code blocks.
- Avoid “note-like” one-liners (for example, “Run X.”) without context. Always pair commands with a short explanation.
- Avoid turning the guide into a dense outline of bullets; it should read as a coherent narrative with structure.

Terminology and domain concepts:
- When a domain-specific term appears (for example, “rollup,” “LSSA,” “shard manager”):
  - Provide 2–4 sentences defining it in plain language at first mention.
  - Relate it to concrete artifacts in the repo (paths, modules, configs).
- If the term’s meaning is opaque from the repo, mark it as `TODO: ask SME what "<term>" means in this project`, and avoid speculative definitions.

Safety:
- Avoid destructive commands (dropping databases, deleting directories, resetting clusters).
- If a step could be risky (for example, connecting to mainnet, touching real data), clearly label it and suggest safer alternatives or test modes if available.

No filler:
- Do NOT add generic motivational or marketing sentences.
- Every explanatory paragraph should:
  - Clarify a concept, OR
  - Explain why a step is needed, OR
  - Connect this action to the bigger picture.

====================
TASK
====================

Your task is to write a DETAILED, PARAGRAPH-ORIENTED FIRST DRAFT quickstart in Markdown.

Follow these subtasks:

1) Choose and state the quickstart scope and audience
   - Identify the most appropriate “first success” for this repo and the chosen audience.
   - At the top of the document, state clearly:
     - The primary audience (end user / operator / developer).
     - What they will have working by the end (for example: “You will run a local node and see it join a test network” or “You will start the API server and make a successful request”).
   - If multiple quickstarts would be useful, mention 1–2 alternative scenarios as future work in a short paragraph, but keep this draft focused on ONE main flow.

2) Write an orienting “About this project” introduction
   - Add an “About <project>” or equivalent section, similar to GitHub Docs quickstarts. :contentReference[oaicite:2]{index=2}
   - In 2–4 paragraphs, explain:
     - What the project is in practical terms (service, library, CLI, UI app, etc.).
     - The typical problems it solves or workflows it fits into.
     - Where this quickstart fits: what you will do at a high level, in plain language.
   - Write as if the reader has NEVER heard of this project or domain.
   - Use concrete phrasing like “You will…” and “This tool lets you…” rather than abstract descriptions.

3) Describe prerequisites with explanations, not just a list
   - Extract all realistic prerequisites:
     - Accounts, access, keys, or tokens.
     - Tools and versions (language runtime, package managers, Docker, etc.).
     - Any services they should have running (databases, message brokers, other components).
   - Present prerequisites as short paragraphs, each explaining:
     - WHAT is required.
     - WHY it is needed in this quickstart (how it will be used later).
   - You may still structure this section as a numbered or bulleted list, but each item should contain 2–4 sentences, not a single line.
   - Mark any unclear prerequisite as `TODO: Confirm [item] with SME.`

4) Design the minimal “happy path” workflow, with rich explanations
   - Define a linear sequence of steps from zero to first success.
   - Each step should be a numbered section with:
     - A clear, imperative heading (for example, `1. Clone the repository`).
     - One or more paragraphs that:
       - Explain what this step does.
       - Explain why it is necessary at this point.
       - Mention which files, directories, or services are involved.
     - A fenced code block showing the key command(s) or snippet(s).
   - For each step, include a short “What to expect” paragraph:
     - Describe what the user should see or verify (logs, console output, UI changes, HTTP responses).
     - Mention any obvious variants or options only if they help understanding (do not explode the scope).
   - Label commands as:
     - `(UNVERIFIED – inferred from repo)` by default.
     - `(VERIFIED – tested)` ONLY if you have actually run them and confirmed.

5) Clarify key concepts and artifacts inline
   - Whenever the guide introduces an important concept (for example, “node,” “worker,” “module”) or artifact (for example, a config file, a container, a service name):
     - Add 2–5 sentences right there, explaining what it represents and how it fits into the system.
   - Reference where the reader can find this artifact in the repo (paths) if they want to inspect further.
   - Avoid deferring all conceptual explanation to a separate “Concepts” section; integrate explanations into the flow of the steps.

6) Add a concise but helpful Troubleshooting section
   - If the repo contains troubleshooting hints (in docs, comments, or scripts), summarize them into a short Troubleshooting section at the end of the workflow.
   - Focus on 3–5 issues that are most likely during the quickstart:
     - Describe the symptom in a full sentence (“If the command hangs and you see X in the logs…”).
     - Briefly explain the likely cause, based on repo evidence.
     - Suggest a simple check or adjustment.
   - If troubleshooting information is sparse, include at least a paragraph stating what is missing and mark `TODO: Add troubleshooting for initial setup (common errors).`

7) Suggest concrete, narrative “Next steps”
   - Recap in one paragraph what the user achieved in this quickstart.
   - Suggest 3–5 next steps in short paragraphs (you can use bullets, but each item must be a couple of sentences), for example:
     - Explore related commands or features in the repo.
     - Read an architecture or design doc to understand internals.
     - Try an advanced scenario or enable a specific feature.
   - Include file paths or URLs in this repo when possible, and explain why each next step is worth doing.

8) Provide notes for the technical writer
   - Add a final section called “Notes for the technical writer”.
   - In normal paragraphs or short bullets (with 1–2 sentences each), list:
     - Any assumptions you made because information is missing.
     - Any steps or concepts marked as `TODO: Needs SME input`.
     - Suggestions for how this quickstart could later be split or adapted for different roles (end user vs operator vs contributor), based on what you saw.

====================
WHAT TO AVOID
====================

- Do not collapse explanations into very short, telegraphic sentences.
- Do not rely heavily on bullet lists to carry the explanation; bullets are for brief enumerations, not the main narrative.
- Do not assume the reader knows domain jargon; always define or flag unfamiliar terms when they appear.
- Do not include very long, unannotated code dumps—prefer short, targeted snippets that are explained in surrounding text.
- Do not add generic motivational or marketing language.

====================
OUTPUT FORMAT (MARKDOWN)
====================

Produce a single Markdown document structured as follows:

1. Title  
2. About this project (2–4 explanatory paragraphs)  
3. Prerequisites (short paragraphs with “what” and “why” for each prerequisite)  
4. Step-by-step quickstart (numbered steps; each step with explanation paragraphs, commands, and “what to expect”)  
5. Troubleshooting  
6. Next steps  
7. Notes for the technical writer  

Formatting guidance:
- Use headings (`#`, `##`, `###`) and numbered sections for the flow.
- Use fenced code blocks for commands and code snippets.
- Use full paragraphs (2–5 sentences) to explain concepts and actions.
- Use bullet lists only where they represent natural, short enumerations (for example, a list of 3–5 links or next steps), and still accompany them with explanatory sentences.
- Make sure the quickstart reads like a guided narrative a new developer can follow, not just a checklist or outline.
