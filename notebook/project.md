# RAG Evaluation with LangSmith

## Overview
This notebook demonstrates how to evaluate an LLM application using LangSmith and Google Gemini API.

## Workflow Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                    SETUP (Cell 0)                                │
│  Load environment variables (LangSmith & Google API keys)         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│              CREATE DATASET (Cell 1)                             │
│  • Create LangSmith dataset "rag_dataset"                         │
│  • Add 6 question-answer pairs for testing                        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│          DEFINE EVALUATORS (Cell 3)                              │
│  • Correctness: LLM-as-judge compares response vs reference       │
│  • Conciseness: Checks if response ≤ 2x reference length         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│            BUILD APPLICATION (Cell 5)                           │
│  • my_app(): Simple Q&A function using Gemini 2.5 Flash          │
│  • ls_target(): Wrapper for LangSmith evaluation                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│              RUN EVALUATION (Cell 7)                             │
│  • client.evaluate() runs app on dataset                        │
│  • Applies both evaluators to each response                      │
│  • Results logged to LangSmith dashboard                         │
└─────────────────────────────────────────────────────────────────┘
```

## Cell-by-Cell Explanation

### Cell 0: Environment Setup
- Loads API keys from `.env` file
- Sets LangSmith tracing enabled for monitoring

### Cell 1: Dataset Creation
- Creates LangSmith dataset with 6 test questions
- Each example has `inputs` (question) and `outputs` (reference answer)

### Cell 3: Evaluation Metrics
- **Correctness**: Uses Gemini as an LLM judge to grade answers as CORRECT/INCORRECT
- **Conciseness**: Simple length check - response must be ≤ 2x the reference answer length

### Cell 5: Application Definition
- `my_app()`: Takes a question, sends to Gemini with system instruction for concise responses
- `ls_target()`: LangSmith-compatible wrapper that returns `{"response": answer}`

### Cell 7: Evaluation Execution
- Runs the application on all dataset examples
- Applies both evaluators to measure performance
- Results viewable at LangSmith dashboard link

## Key Components
- **LangSmith**: Platform for dataset management and evaluation tracking
- **Google Gemini 2.5 Flash**: LLM used for both answering and judging
- **LLM-as-Judge**: Using an LLM to evaluate another LLM's outputs
