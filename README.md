Got it! Here's the revised `README.md`, updated to reflect **support for any number of configurable AI models**, with a dynamic voting/judging system where **each AI evaluates all the others**.

---

````markdown
# AI Council CLI

**AI Council** is a self-managing, terminal-based AI assistant that leverages the collective intelligence of multiple large language models to deliver high-quality answers and code suggestions through democratic consensus.

Rather than relying on a single model, AI Council queries **any number of configurable AIs in parallel**—such as Claude, ChatGPT, DeepSeek, Mistral, etc.—then asks **each model to judge the others' responses**, scoring them based on quality. The best response is selected through rank-based voting and presented to the user for approval or refinement.

**The council manages itself**: Simply ask it to add or remove AI members, and it will guide you through the process. It tracks performance metrics and voting history to help you optimize your council over time.

## 🧠 Key Features

- 🧾 **Chat-style CLI interface** – Ask natural language questions and receive clear, contextual responses.
- 🤖 **Self-managing council** – Add/remove AI members by simply asking the council.
- 📊 **Performance tracking** – Tracks response times, win rates, and voting history for each AI.
- ⚖️ **Decentralized peer review** – Each AI agent ranks the others' solutions.
- 🏆 **Intelligent ranking** – Balances quality scores with response speed for optimal results.
- 🔁 **Human-in-the-loop refinement** – Accept, reject, or iterate on the winning result.
- ⚙️ **JSON-based configuration** – No code changes needed to add new AI providers.

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

## ✨ Example Workflows

### Asking Questions
```bash
$ aicouncil ask "Refactor this React component to use hooks instead of a class..."

🤖 Claude is thinking... (1.2s)
🤖 ChatGPT is thinking... (0.8s)
🤖 DeepSeek is thinking... (2.1s)
🤖 Mistral is thinking... (1.5s)

🏁 Peer Reviews Complete:

Claude ranked: 1. ChatGPT, 2. DeepSeek, 3. Mistral  
ChatGPT ranked: 1. Claude, 2. Mistral, 3. DeepSeek  
DeepSeek ranked: 1. Claude, 2. ChatGPT, 3. Mistral  
Mistral ranked: 1. Claude, 2. ChatGPT, 3. DeepSeek

🏆 Winner: **Claude's response** (Quality: 8.7/10, Speed: 1.2s)

--- Claude's Suggested Code ---
[...code block...]

Do you want to:
[A] Accept  [F] Provide Feedback  [R] Rerun Round
```

### Managing Your Council
```bash
$ aicouncil add "I want to add Gemini to my council"

🎯 Adding new AI to council...

Please provide the following information:
• API Endpoint: https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent
• API Key: [Enter your Gemini API key]
• Model Name: gemini-pro
• Max Tokens (default 4000): 
• Temperature (default 0.7): 

✅ Gemini added to council! Testing connection... Success!

$ aicouncil remove mistral
🗑️  Mistral removed from council.

$ aicouncil stats
📊 Council Performance (Last 30 days):
Claude:    Win Rate: 45% | Avg Speed: 1.1s | Quality: 8.8/10
ChatGPT:   Win Rate: 35% | Avg Speed: 0.9s | Quality: 8.5/10  
DeepSeek:  Win Rate: 15% | Avg Speed: 2.3s | Quality: 7.9/10
Gemini:    Win Rate: 5%  | Avg Speed: 1.4s | Quality: 8.2/10
```

---

## 📦 Project Structure (Planned)

```text
aicouncil/
├── config/
│   ├── models.json      # Dynamic AI provider configuration
│   └── voting.json      # Voting rules and judge prompts
├── data/
│   ├── performance.db   # SQLite database for metrics
│   ├── voting_history/  # Historical voting records
│   └── session_logs/    # Conversation logs
├── src/
│   ├── council.py       # Main council orchestrator
│   ├── http_client.py   # Universal HTTP client for all providers
│   ├── judge.py         # Peer ranking system
│   ├── voting.py        # Ranking algorithms (Borda, etc.)
│   ├── performance.py   # Metrics tracking and analysis
│   ├── cli.py           # Terminal interface
│   └── config_manager.py # Dynamic config management
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

The initial `config/models.json` will look like this:

```json
{
  "models": [
    {
      "name": "claude",
      "provider": "anthropic",
      "endpoint": "https://api.anthropic.com/v1/messages",
      "auth": {
        "type": "bearer",
        "token": "${ANTHROPIC_API_KEY}"
      },
      "model": "claude-3.5-sonnet",
      "parameters": {
        "max_tokens": 4000,
        "temperature": 0.7
      },
      "enabled": true,
      "performance": {
        "wins": 0,
        "total_votes": 0,
        "avg_response_time": 0,
        "avg_quality_score": 0
      }
    }
  ],
  "voting": {
    "method": "weighted_borda",
    "speed_weight": 0.3,
    "quality_weight": 0.7,
    "judge_prompt": "Rate each response 1-10 on accuracy, clarity, and usefulness"
  }
}
```

**But you don't need to edit this manually!** Just ask the council to add new AIs for you.

---

## 🗳️ Intelligent Voting & Performance Tracking

### Weighted Voting System
The council uses **Weighted Borda Count** that considers both quality and speed:

* Each model ranks the others on quality (1-10 scale)
* Response times are measured for each AI
* Final score = `(quality_score × 0.7) + (speed_bonus × 0.3)`
* Speed bonus: Faster responses get higher speed scores
* **The highest combined score wins**

### Performance Metrics Tracked
- **Win Rate**: Percentage of times each AI's response was selected
- **Judge Accuracy**: How often an AI correctly identifies the winning response
- **Average Quality Score**: Peer-rated quality across all responses  
- **Average Response Time**: How quickly each AI responds
- **Consistency Score**: How reliable the AI is across different question types
- **Vote History**: Complete record of all peer rankings and judging accuracy

### Historical Analysis
```bash
$ aicouncil history claude --last-month
📈 Claude Performance Trends:
Win Rate: 45% → 52% (↑7%)
Judge Accuracy: 78% → 82% (↑4%) - Great at picking winners!
Quality: 8.5/10 → 8.8/10 (↑0.3)
Speed: 1.3s → 1.1s (↑15% faster)
Best Categories: Code Review, Architecture Design
Weakness: Math Problems (6.2/10 avg)

$ aicouncil judges --top-performers
🏆 Best Judges (Judge Accuracy):
1. DeepSeek: 89% accuracy (0.3s avg) - Fast & accurate judge!
2. Claude: 82% accuracy (1.1s avg)
3. ChatGPT: 79% accuracy (0.9s avg)
4. Mistral: 71% accuracy (1.5s avg)

💡 Insight: DeepSeek rarely wins but excellent at identifying quality!

$ aicouncil compare claude chatgpt
🆚 Head-to-Head Comparison (30 days):
Claude vs ChatGPT: 23-17 (57% win rate)
Quality Gap: +0.3 in Claude's favor
Speed Gap: ChatGPT 0.2s faster
Judge Accuracy: Claude +3% better at picking winners
Recommendation: Keep both - Claude for complex tasks, ChatGPT for quick responses
```

Alternative voting methods available:
- **Condorcet** (head-to-head comparisons)
- **Plurality** (simple first-choice voting)  
- **Custom weights** (adjust speed vs quality importance)

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

## 🎯 Complete Command Reference

### Core Commands
```bash
# Ask questions
aicouncil ask "How do I optimize this SQL query?"
aicouncil ask --file mycode.py "Review this code"

# Council management  
aicouncil add "Add OpenAI GPT-4 to my council"
aicouncil remove claude
aicouncil list                    # Show all council members
aicouncil enable mistral          # Re-enable a disabled AI
aicouncil disable deepseek        # Temporarily disable an AI

# Performance analysis
aicouncil stats                   # Overall performance summary
aicouncil stats --detailed        # Extended metrics
aicouncil history claude          # Individual AI performance
aicouncil judges                  # Judge accuracy rankings
aicouncil compare claude chatgpt  # Head-to-head comparison

# Configuration
aicouncil config voting --method condorcet    # Change voting method
aicouncil config weights --quality 0.8 --speed 0.2  # Adjust scoring
aicouncil test connections         # Test all API connections
```

### Council Member Value Types
The council recognizes different types of valuable members:

1. **🏆 Winners**: AIs that frequently produce the best responses
2. **🎯 Judges**: AIs that excel at identifying quality (high judge accuracy)  
3. **⚡ Speed Demons**: Fast responders that maintain decent quality
4. **🎨 Specialists**: AIs that excel in specific domains
5. **🧠 Reliable**: Consistent performers across all question types

**Example**: An AI with 15% win rate but 89% judge accuracy is incredibly valuable for council decision-making!

---

## 💡 Inspiration

This project is inspired by the belief that **no single AI is perfect**. By enabling them to **peer-review and vote on each other's output**, we combine their strengths into a single, intelligent, trustworthy assistant—**an AI council**.

The council learns and evolves, tracking which AIs excel at different tasks and which are best at judging quality. This creates a dynamic, self-optimizing system that gets better over time.

---

```

Let me know if you'd like this turned into a real repository structure or if you'd like help with a `models.yaml` parser or a CLI prototype to go with it!
```
