# Macro-Pulse-Deterministic-Financial-Sentiment-Screener


<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/934450a0-95d3-485f-9118-fe7eac41ee81" />


Macro-Pulse is a lightweight, non-agentic tool that analyzes real-time financial news to determine market sentiment. Unlike "black box" autonomous agents, this project uses a strict linear RAG (Retrieval-Augmented Generation) pipeline to ensure auditability, speed, and zero hallucination regarding source data. 

Here is the updated `README.md`. I have stripped out the Streamlit app references, positioning this strictly as a backend library and CLI tool for developers and quantitative analysts.

---

# Macro-Pulse: Deterministic Financial Sentiment Engine

**Macro-Pulse** is a lightweight, non-agentic library that analyzes real-time financial news to determine market sentiment. Unlike "black box" autonomous agents, this project uses a strict **linear RAG (Retrieval-Augmented Generation) pipeline** to ensure auditability, speed, and zero hallucination regarding source data.

It connects a **Web Search API** (Tavily/Serper) with a **Fine-Tuned LLM** (Llama-3 or Mistral) to produce a structured "Market Impact Report" for any given financial query.

---

## 🏗️ Architecture

This project explicitly **avoids** agentic loops (like ReAct or AutoGPT) in favor of a predictable, linear flow suitable for batch processing and automated trading pipelines:

```mermaid
graph LR
    A[User Query] -->|Input| B(Search API Wrapper)
    B -->|Raw Snippets| C[Content Cleaner]
    C -->|Context + Prompt| D(Fine-Tuned LLM)
    D -->|Inference| E[Structured JSON Output]

```



1. **Searcher:** Queries the web for the latest filings and news (filtered by domain).
2. **Cleaner:** Removes HTML noise and irrelevant boilerplate.
3. **Interpreter:** A standard LLM (fine-tuned on financial phrasebanks) processes the text.
4. **Reporter:** Outputs a strict JSON containing Sentiment, Impact Score, and Rationale.

<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/4ca41b6f-94ba-4a4e-bed8-aa6eb9eefe68" />



---

## ✨ Features

* **Real-Time Intelligence:** Fetches data generated *seconds* ago (unlike static LLMs).
* **Deterministic Logic:** The same input always yields the same process flow (crucial for financial auditing).
* **Source Citations:** Every claim is linked back to the specific URL found by the searcher.
* **Sentiment Scoring:** Classifies news as `Hawkish`, `Dovish`, `Bullish`, or `Bearish` with a 1-10 impact intensity scale.
* **Library Ready:** Designed to be imported into larger quantitative finance projects.

---

## 🚀 Getting Started

### Prerequisites

* Python 3.10 or higher
* A Tavily API Key (Free tier available)
* A GPU if running the model locally (or an OpenAI/Anthropic key if using cloud models)

### Installation

1. **Clone the repository:**
```bash
git clone https://github.com/yourusername/macro-pulse.git
cd macro-pulse

```


2. **Create a virtual environment:**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

```


3. **Install dependencies:**
```bash
pip install -r requirements.txt

```


4. **Set up environment variables:**
Create a `.env` file in the root directory:
```ini
TAVILY_API_KEY=tvly-xxxxx
# Optional: If using OpenAI instead of local model
OPENAI_API_KEY=sk-xxxxx

```



---

## 💻 Usage

### 1. As a CLI Tool

Quickly analyze a topic from your terminal:

```bash
python main.py --query "NVIDIA Q3 Earnings" --depth 5

```

**Output Example:**

```json
{
  "query": "NVIDIA Q3 Earnings",
  "sentiment": "Bullish",
  "impact_score": 8.5,
  "summary": "Record revenue driven by data center demand; guidance exceeds analyst expectations.",
  "sources": ["bloomberg.com/...", "reuters.com/..."]
}

```

### 2. As a Python Library

Import `MacroPulse` into your own trading bot or analysis script:

```python
from macro_pulse import MacroAnalyzer

# Initialize with your preferred search provider
analyzer = MacroAnalyzer(model="mistral-7b-finetuned")

result = analyzer.scan("Federal Reserve Interest Rate Decision")

print(f"Sentiment: {result.sentiment}")
print(f"Impact Score: {result.score}/10")

```

---

## 🧠 Fine-Tuning Strategy

The "Interpreter" model in this pipeline is not a raw generic model. It undergoes a two-step adaptation process (scripts available in `training/`):

1. **Continual Pre-training (CPT):**
* *Dataset:* 10GB of SEC Filings and Central Bank Meeting Minutes.
* *Goal:* Adapt the tokenizer and embeddings to financial jargon (e.g., understanding "alpha" as return, not the Greek letter).


2. **Instruction Fine-Tuning (SFT):**
* *Method:* QLoRA (Quantized Low-Rank Adaptation) on a consumer GPU.
* *Dataset:* [Financial PhraseBank](https://www.google.com/search?q=https://huggingface.co/datasets/financial_phrasebank) + Custom "News-to-JSON" pairs.
* *Objective:* Force the model to output strict JSON without conversational filler.



---

## 📂 Project Structure

```text
macro-pulse/
├── main.py                # CLI Entry point
├── requirements.txt       # Dependencies
├── .env                   # Secrets (GitIgnored)
├── macro_pulse/           # Core Package
│   ├── __init__.py
│   ├── searcher.py        # Tavily API wrapper
│   ├── processor.py       # Text cleaning logic
│   └── llm_engine.py      # Model loading and inference
├── training/              # Fine-tuning scripts
│   ├── prepare_data.py    # Formatting datasets
│   └── train_lora.py      # QLoRA training script
└── tests/                 # Unit tests

```

---

## 🤝 Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## 📄 License

[MIT](https://choosealicense.com/licenses/mit/)
