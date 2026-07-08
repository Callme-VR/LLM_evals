# LLM Evaluation & Security Framework

A comprehensive framework for evaluating, testing, and securing Large Language Model (LLM) applications with a focus on RAG systems, guardrails, and agentic AI security.

## 🎯 Project Overview


major thing is i prefer here in the llm evals use the Open ai embedding model for vector storage and retrieval
because it is more accurate and reliable and faster here in this project i have used the google which give me error ,thats why i am using open ai embedding model



This project provides tools and methodologies for:
- **RAG Evaluation**: Systematic evaluation of Retrieval-Augmented Generation systems using LangSmith
- **Security Testing**: Testing for prompt injections, jailbreaks, and adversarial attacks
- **Guardrails Implementation**: Implementing robust guardrails for LLM applications
- **Multi-Provider Support**: Working with multiple LLM providers (OpenAI, Google, Groq, Mistral, etc.)
- **Agentic AI Security**: Securing autonomous AI agents and workflows

## 📁 Project Structure

```
LLM_evals/
├── .env                    # API keys and configuration
├── requirements.txt        # Python dependencies
├── topic.txt               # Security topics and guardrail notes
├── notebook/
│   ├── RagwithEval.ipynb   # RAG evaluation notebook with LangSmith
│   └── project.md          # Detailed workflow documentation
└── README.md              # This file
```

## 🚀 Quick Start

### Prerequisites

- Python 3.11 or higher
- API keys for LLM providers (Google, OpenAI, etc.)
- LangSmith account for evaluation tracking

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd LLM_evals
```

2. Create a virtual environment:
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Configure environment variables:
```bash
cp .env.example .env
# Edit .env with your API keys
```

Required environment variables:
- `GOOGLE_API_KEY`: Google Gemini API key
- `LANGSMITH_API_KEY`: LangSmith API key
- `OPENAI_API_KEY`: OpenAI API key (optional)
- `GROQ_API_KEY`: Groq API key (optional)

## 📊 RAG Evaluation with LangSmith

The main notebook (`notebook/RagwithEval.ipynb`) demonstrates a complete RAG evaluation workflow:

### Workflow Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    SETUP                                         │
│  Load environment variables (LangSmith & Google API keys)         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│              CREATE DATASET                                     │
│  • Create LangSmith dataset "rag_dataset"                         │
│  • Add question-answer pairs for testing                        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│          DEFINE EVALUATORS                                      │
│  • Correctness: LLM-as-judge compares response vs reference       │
│  • Conciseness: Checks if response ≤ 2x reference length         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│            BUILD APPLICATION                                   │
│  • my_app(): Simple Q&A function using Gemini 2.5 Flash          │
│  • ls_target(): Wrapper for LangSmith evaluation                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│              RUN EVALUATION                                     │
│  • client.evaluate() runs app on dataset                        │
│  • Applies both evaluators to each response                      │
│  • Results logged to LangSmith dashboard                         │
└─────────────────────────────────────────────────────────────────┘
```

### Running the Evaluation

1. Open the notebook:
```bash
jupyter notebook notebook/RagwithEval.ipynb
```

2. Execute cells sequentially to:
   - Set up environment variables
   - Create evaluation dataset
   - Define custom evaluators (correctness, conciseness)
   - Build the RAG application
   - Run evaluation and view results in LangSmith

### Key Components

- **LangSmith**: Platform for dataset management and evaluation tracking
- **Google Gemini 2.5 Flash**: LLM used for both answering and judging
- **LLM-as-Judge**: Using an LLM to evaluate another LLM's outputs
- **Custom Evaluators**: Correctness and conciseness metrics

## 🔒 Security & Guardrails

### Focus Areas

- **Prompt Injections**: Detecting and preventing malicious prompt injections
- **Jailbreaks**: Testing for adversarial jailbreak attempts
- **Guardrails**: Implementing robust security guardrails
- **Agentic AI Security**: Securing autonomous AI agents

### Guardrail Solutions

The project explores various guardrail implementations:

- **Nemo Guardrails**: NVIDIA's guardrail framework
- **Llama Firewall**: Meta's security firewall for LLM applications
- **AWS Bedrock Guardrails**: Amazon's managed guardrail service

### Security Principles

- **Robustness**: Building fault-tolerant systems
- **Latency-free**: Ensuring real-time security checks
- **Gateways**: Implementing secure API gateways
- **Defense in Depth**: Multiple layers of security

## 🛠️ Technology Stack

### Core LLM Clients
- OpenAI
- Groq
- Google Generative AI
- Mistral AI

### LangChain Ecosystem
- LangChain Core
- LangChain Community
- LangChain OpenAI
- LangChain Google GenAI
- LangChain Groq
- LangChain HuggingFace
- LangChain MistralAI

### LangGraph
- LangGraph (for agentic workflows)
- LangGraph Checkpoint
- LangGraph Checkpoint SQLite

### Vector Stores
- FAISS CPU
- ChromaDB

### Document Processing
- PyPDF
- python-docx
- LangChain Text Splitters

### ML / Deep Learning
- PyTorch
- Transformers (HuggingFace)
- Sentence Transformers
- Accelerate

### API & UI
- FastAPI
- Uvicorn
- Streamlit

### Web Scraping
- Playwright

### Observability
- LangSmith
- Tenacity (retry logic)

## 📝 Usage Examples

### Basic RAG Evaluation

```python
from langsmith import Client
from google import genai

# Initialize clients
client = Client()
gemini_client = genai.Client()

# Create dataset
dataset = client.create_dataset(dataset_name="rag_dataset")

# Run evaluation
results = client.evaluate(
    target_function,
    data="rag_dataset",
    evaluators=[correctness, conciseness],
)
```

### Custom Evaluator

```python
def correctness(inputs, outputs, reference_outputs) -> bool:
    """LLM-as-judge evaluator for answer correctness"""
    prompt = f"""
    Question: {inputs['question']}
    Reference: {reference_outputs['answer']}
    Student Answer: {outputs['response']}
    
    Respond with CORRECT or INCORRECT
    """
    response = gemini_client.models.generate_content(
        model="gemini-2.5-flash",
        contents=prompt
    )
    return response.text.strip().upper() == "CORRECT"
```

## 🧪 Testing

The project includes evaluation datasets for testing:
- Question-answer pairs for correctness evaluation
- Reference answers for comparison
- Multiple test scenarios

## 📈 Evaluation Metrics

- **Correctness**: LLM-as-judge evaluation comparing responses to reference answers
- **Conciseness**: Length-based metric ensuring responses are not excessively verbose
- **Custom Metrics**: Extensible framework for adding custom evaluators

## 🤝 Contributing

Contributions are welcome! Areas for contribution:
- Additional evaluators and metrics
- More guardrail implementations
- Support for additional LLM providers
- Enhanced security testing scenarios

## 📄 License

[Specify your license here]

## 🔗 Resources

- [LangSmith Documentation](https://docs.smith.langchain.com/)
- [LangChain Documentation](https://python.langchain.com/)
- [Google Gemini API](https://ai.google.dev/gemini-api/docs)
- [Nemo Guardrails](https://github.com/NVIDIA/NeMo-Guardrails)

## 📧 Contact

For questions or issues, please open an issue on the repository.

---

**Note**: This project is focused on research and development of secure, evaluable LLM systems. Always ensure you have proper API quotas and follow rate limits when using LLM providers.
