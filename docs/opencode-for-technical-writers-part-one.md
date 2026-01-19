# OpenCode for technical writers (Part 1)

#### Get useful answers from an unfamiliar repo without breaking anything

As a technical writer, you are often parachuted in unfamiliar codebases and repositories you need to document. You may not have access to software engineers right away and, even if you do, you lack the basic knowledge to ask the right questions.

This situation is even more challenging in large repositories that have evolved over time, where it's hard to tell what is authoritative and what is just technical debt.

This article shows how to use [OpenCode](https://opencode.ai/) (an open source AI coding agent) to understand an unfamiliar repository, and without breaking anything. It is written for technical writers who need to produce evidence-based docs (repo maps, quick start guides, documentation gap reports), often under time pressure and with limited resources.

## What is OpenCode

Most technical writers I know still use the ChatGPT chat interface to get assistance with their documents. I do too. It is convenient, fast, and good enough for a lot of tasks, specially for those where the input (your questions) are well-defined.

But when I land in a new GitHub repo, the first questions are usually fuzzy and exploratory

- What is this repo about, really?
- The `README` doesn't work for me; how do I run this?
- I can't find any useful information; is the documentation hiding in code comments?
- Is the `README` and existing documentation aligned with the code and CI, or are they outdated?

This is where OpenCode starts to make sense. In these situations, a chat interface starts to feel limiting and tedious. I must constantly copy and paste information between the repo and the chat and, at the same time, I am limiting the LLM capacities to explore the content by itself.

> [!NOTE]
>
> You can "attach" a GitHub repository to your chat in tools like ChatGPT or GitHub Copilot, but this [is not always a viable option](#why-not-using-chatgpt-directly).

OpenCode is an open-source AI coding agent you can use to explore repositories and ask questions about them. OpenCode is not the first coding agent out there, you might have heard of [OpenAI Codex](https://openai.com/codex/), [GitHub Copilot](https://github.com/features/copilot), or [Claude Code](https://claude.com/product/claude-code). These code agents typically focus on helping developers write code faster, but they are equally useful for technical writers who need to understand codebases or draft technical documentation.

OpenCode’s main advantage over other coding agents is that it’s provider-agnostic: you can use many LLM providers (including local models) and switch between them as needed. GitHub Copilot also supports multiple models, but [availability depends on the client and plan](https://docs.github.com/en/copilot/reference/ai-models/supported-models#supported-models-per-client).

## How I use OpenCode in this exercise

For this article, I only care about the [OpenCode terminal user interface (TUI) version](https://opencode.ai/docs/tui/) because it is the easiest to treat as a "repo research tool" for writers.

OpenCode can be used with different model providers. You can use [OpenCode subscription model](https://opencode.ai/black) or the pay-as-you-go model with [OpenCode Zen](https://opencode.ai/docs/zen/). For this exercise, I will use the ChatGPT Plus subscription.

## Step 1: Install OpenCode

OpenCode has [multiple installation options](https://opencode.ai/docs#install). Pick the one that matches your setup and operating system. Here are some installation options:

```bash
# install script
curl -fsSL https://opencode.ai/install | bash

# Arch Linux
paru -S opencode

# Homebrew
brew install anomalyco/tap/opencode

# npm
npm i -g opencode-ai
```

## Step 2: Configure "safe mode" in OpenCode

As a technical writer, you only want to understand the repository information and gather enough context to write your first draft. You don't want to change any content. To that end, create an `opencode.json` in the repo root with a conservative [permission profile](https://opencode.ai/docs/permissions):

- Deny file edits
- Ask before running shell commands
- Ask before fetching from the web

You can define the OpenCode settings in different [locations](https://opencode.ai/docs/config/#locations). For this exercise, we will use a global configuration file in the default configuration folder for your user:

1. Create the  `~/.config/opencode` folder if it does not exist:

    ```bash
    mkdir -p ~/.config/opencode
    ```

2. Create the `opencode.json` configuration file with these options:

    ```json
    {
      "$schema": "https://opencode.ai/config.json",
      "permission": {
        "edit": "deny",
        "bash": "ask",
        "webfetch": "ask"
      }
    }
    ```

What these options mean in practice:

- OpenCode can still read and search the repo.
- OpenCode can still generate content for you (in the chat output).
- OpenCode cannot “helpfully” edit files behind your back.

> [!NOTE]
>
> With `"webfetch": "ask"`, OpenCode will ask you before fetching anything from the web outside of the repository. This can be helpful if, for example, the repo documentation links to external resources that are outdated, and you don't want to pollute your context. Setting `"webfetch": "allow"` won't change the repository and is optional.

## Step 3: Pick a repo you can play with

1. Pick a GitHub repository from your organization, or another repository you are interested to know about.

    For this article, I will use `logos-co/logos-wallet-ui` as the [target repository](https://github.com/logos-co/logos-wallet-ui).

    > [!NOTE]
    >
    > I am a core contributor to the Logos Collective organization this repository belongs to. I picked it because I know it well enough to judge OpenCode’s output quality.

2. Clone the repository to your local machine:

    ```bash
    git clone https://github.com/logos-co/logos-wallet-ui.git
    cd logos-wallet-ui
    ```

## Step 4: Connect OpenCode to your LLM provider

In the past, you needed to [create an OpenAI API key](#connect-opencode-to-openai-using-your-api-key) to use OpenCode with OpenAI models. Now, you can connect your ChatGPT Plus or Pro subscription to OpenCode, without an API key. You can also use your GitHub Copilot subscription. For this exercise, I will use my ChatGPT Plus subscription, as this is what most people I know use.

To set up OpenCode with ChatGPT Plus or Pro:

1. Open a terminal in the repo root directory.
2. Run:

    ```bash
    opencode auth login
    ```
  
    This will open the OpenCode provider selector.

3. Using your arrow keys, navigate to **OpenAI** and press **Enter**.
4. In the **Login method**, select **ChatGPT Pro/Plus** and press **Enter**.
5. Copy the long URL in the **Go to:** line and open it in your browser.
6. Complete the ChatGPT login process.
7. Back in the terminal, you should see a "Login successful".

> [!NOTE]
>
> If you don't have a ChatGPT Plus or Pro subscription, you can still use OpenCode with OpenAI [by creating an API key](https://platform.openai.com/docs/quickstart). Alternatively, you can use [OpenCode Zen](#connect-opencode-to-opencode-zen).

## Step 5: Set up your first OpenCode session

In this first session, we will configure OpenCode in "Plan" mode, which focuses on planning and understanding the repo rather than generating code.

![OpenCode TUI main screen showing the active model and mode options](./opencode-for-technical-writers-part-one/images/opencode-model-selection.png)

Open the OpenCode command line interface (CLI):

1. Open a terminal in the repo root.
2. Run:

    ```bash
    opencode
    ```

3. In the OpenCode prompt, type `/models` and press **Enter**.
4. Using your arrow keys, navigate to **GPT-5.2 Codex** and press **Enter**.
5. Back in the OpenCode prompt, press the **Tab** key to change from the default "Build" mode to "Plan" mode.
6. Paste [this Markdown prompt](./opencode-for-technical-writers-part-one/files/prompt-understand-a-repo.md) into OpenCode and press **Enter**.

> [!TIP]
>
> If OpenCode asks permission to run a shell command, I usually approve only read-only commands early on. To do so, choose **Always allow** using your arrow keys and press **Enter**. Press **Enter** again to **Confirm**. These are some examples of safe commands I allow: `ls`, `find`, `rg`,`cat`, `git rev-parse --short HEAD`.

OpenCode will now analyze the repository using the [GPT-5.2 Codex](https://openai.com/index/introducing-gpt-5-2-codex/) model in "Plan" mode. At the end of the analysis, it provides a summary of its findings. You can copy this summary for your reference (press **Ctrl+x** and the press **y** to copy the last message to your clipboard).

At this point, you can read the report and ask follow-up with questions to dig deeper into specific areas of interest. You can clarify concepts that are new to you or unclear in the report, and even ask for specific details for your documentation needs; for example:

- If I had to write a quickstart for a new contributor, what are the missing pieces?
- List anything in the `README` that looks suspicious, outdated, or inconsistent with code.
- As a technical writer who must document this repository, what are the 5 top questions to ask to the SME?

> [!TIP]
>
> You can exit OpenCode by typing `/exit` and pressing **Enter**. You can always return to previous sessions by typing `/sessions` in the OpenCode prompt.

Once you have the information you need, you can ask OpenCode to draft specific documentation for you, such as a [quickstart guide](./opencode-for-technical-writers-part-one/files/prompt-quickstart-guide.md) or a [reference document](./opencode-for-technical-writers-part-one/files/prompt-reference-doc.md). These drafts won't be complete or perfect, but they can give you a solid starting point to work from.

## Why not using ChatGPT directly?

In chat-based interfaces like ChatGPT or GitHub Copilot, you can attach a GitHub repository to your chat and ask questions about it. This means that, to some extent, you can try to replicate this workflow using ChatGPT in the browser. However this has serious limitations:

- Your GitHub administrator may block repo access from third-party apps like ChatGPT.
- The repository may not be indexed by GitHub, so ChatGPT can't help you (or worse, it hallucinates).
- The content you sent to ChatGPT is used by OpenAI to train its models, which may violate your organization's data policies.
- You cannot automate and make this process repeatable easily, as you can do with OpenCode.

Using ChatGPT or GitHub Copilot to answer specific questions may be fine when you are familiar with a repository. But for a more systematic and structured approach in a codebase you don't know, OpenCode gives you more control and possibilities.

## What is next

In Part 2, I will turn this exercise into a repeatable workflow using [OpenCode "skills"](https://opencode.ai/docs/skills/) (so you do not rewrite the same prompt every time).

## Appendix: Connect OpenCode to OpenCode Zen

[OpenCode Zen](https://opencode.ai/docs/zen/) is a curated list of models that have been tested and verified by the OpenCode team. To use OpenCode Zen, you must link your GitHub or Google account to OpenCode and use a payment method to add credit to your account. If you are not comfortable with this, you can also [use OpenAI's models with your ChatGPT subscription](#step-4-connect-opencode-to-your-llm-provider).

> [!NOTE]
>
> Using Zen requires providing your payment details to OpenCode, even if you decide to not use paid models and stick to free ones.

To set up OpenCode Zen:

1. [Login to OpenCode](https://opencode.ai/auth) with your GitHub or Google account.
2. On the Zen page, click **Enable billing**.
3. Complete the Stripe form with your payment details.
4. Back in OpenCode website, go to the [API Keys](https://opencode.ai/account/api-keys) page and click **Create API Key**.
5. Enter the API name (for example, "testing") and click **Create**.
6. Hover over the new API key and click the copy icon to copy it to your clipboard.
7. Follow the procedure in the [Connect OpenCode to your LLM provider](#step-4-connect-opencode-to-your-llm-provider) and choose **OpenCode Zen** as the provider.

> [!CAUTION]
>
> By default, OpenCode will charge again your payment method when your credits run low. To avoid unexpected charges, disable the **Auto reload** option in the [Billing](https://opencode.ai/account/billing) page.
