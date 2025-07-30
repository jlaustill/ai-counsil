Got it! Here's the revised `README.md`, updated to reflect **support for any number of configurable AI models**, with a dynamic voting/judging system where **each AI evaluates all the others**.

---

````markdown
# AI Council CLI

**AI Council** is a terminal-based AI assistant that leverages the collective intelligence of multiple large language models to deliver high-quality answers and code suggestions through democratic consensus.

Rather than relying on a single model, AI Council queries **any number of configurable AIs in parallel**—such as Claude, ChatGPT, DeepSeek, Mistral, etc.—then asks **each model to judge the others' responses**, scoring them based on quality. The best response is selected through rank-based voting and presented to the user for approval or refinement.

## 🧠 Key Features

- 🧾 **Chat-style CLI interface** – Ask natural language questions and receive clear, contextual responses.
- 🤖 **Multi-model orchestration** – Supports an arbitrary number of AI models, configured in one place.
- ⚖️ **Decentralized peer review** – Each AI agent ranks the others’ solutions.
- 🏆 **Ranked-choice voting** – Uses rank voting (e.g., Borda count) to determine the winner.
- 🔁 **Human-in-the-loop refinement** – Accept, reject, or iterate on the winning result.
- 🧱 **Modular model adapters** – Easily add/remove AI backends (Claude, GPT-4, Ollama, etc.).

---

## 🔧 Supported AI Backends

Out of the box, AI Council supports:

- 🧠 **Claude** (Anthropic API)
- 💬 **ChatGPT** (OpenAI API)
- 🧰 **DeepSeek Coder** (via [Ollama](https://ollama.com))
- 🧪 ...and any other model via plugin-style adapter.

---

## 🏗️ Planned Architecture

```text
+--------------------------+
|      AI Model Agents     |
| (Configured dynamically) |
+------------+-------------+
             |
             v
+----------------------------+
|  Prompt Dispatcher         |
| - Sends user prompt to all |
|   configured models        |
+----------------------------+
             |
             v
+----------------------------+
|  Peer Judging System      |
| - Each model ranks the    |
|   responses of all others |
+----------------------------+
             |
             v
+----------------------------+
|  Rank Voting Aggregator   |
| - Scores models based on  |
|   peer rankings           |
| - Picks the winner        |
+----------------------------+
             |
             v
+----------------------------+
|  Terminal Interaction UI  |
| - Shows result            |
| - Accept / Reject / Refine|
+----------------------------+
````

---

## ✨ Example Workflow

```bash
$ aicouncil ask "Refactor this React component to use hooks instead of a class..."

🤖 Claude is thinking...
🤖 ChatGPT is thinking...
🤖 DeepSeek is thinking...
🤖 Mistral is thinking...

🏁 Peer Reviews Complete:

Claude ranked: 1. ChatGPT, 2. DeepSeek, 3. Mistral  
ChatGPT ranked: 1. Claude, 2. Mistral, 3. DeepSeek  
DeepSeek ranked: 1. Claude, 2. ChatGPT, 3. Mistral  
Mistral ranked: 1. Claude, 2. ChatGPT, 3. DeepSeek

🏆 Final Winner: **Claude’s response**

--- Claude’s Suggested Code ---
[...code block...]

Do you want to:
[A] Accept  [F] Provide Feedback  [R] Rerun Round
```

---

## 📦 Project Structure (Planned)

```text
aicouncil/
├── adapters/
│   ├── claude.py
│   ├── chatgpt.py
│   ├── deepseek.py
│   └── mistral.py  # Example custom model
├── config/
│   └── models.yaml  # List of enabled models + credentials
├── judge.py         # Prompts each model to rank the others
├── rank_voting.py   # Borda count or Condorcet-style vote logic
├── cli.py           # Terminal interface
├── session_history/
│   └── ... (logs and feedback)
├── utils.py
└── main.py
```

---

## 🔧 Setup (Planned)

### Requirements

* Python 3.10+
* Pipenv or virtualenv
* API Keys for the models you enable
* Ollama (for any local models like DeepSeek or Mistral)

### Clone & Setup

```bash
git clone https://github.com/YOUR_USERNAME/aicouncil.git
cd aicouncil
pip install -r requirements.txt
```

### Configure Models

Create `config/models.yaml` like this:

```yaml
models:
  - name: claude
    adapter: claude
    api_key: ${ANTHROPIC_API_KEY}

  - name: chatgpt
    adapter: chatgpt
    api_key: ${OPENAI_API_KEY}

  - name: deepseek
    adapter: ollama
    local_name: deepseek-coder

  - name: mistral
    adapter: ollama
    local_name: mistral
```

You can comment out any model to disable it.

---

## 🗳️ Voting Logic

Uses **Borda count** by default:

* Each model ranks the others.
* Scores assigned as:

  * 1st = N-1 points
  * 2nd = N-2 points
  * ...
  * Last = 0 points
* The model with the **highest total score wins**.

You could later switch to:

* **Condorcet**
* **Plurality**
* **Weighted judge voting**

---

## 🙋‍♀️ Contributing

We’re looking for contributors passionate about:

* AI orchestration and meta-model coordination
* CLI developer productivity tools
* Transparent and ethical model comparison

---

## 📄 License

MIT

---

## 💡 Inspiration

This project is inspired by the belief that **no single AI is perfect**. By enabling them to **peer-review and vote on each other's output**, we combine their strengths into a single, intelligent, trustworthy assistant—**an AI council**.

---

```

Let me know if you'd like this turned into a real repository structure or if you'd like help with a `models.yaml` parser or a CLI prototype to go with it!
```
