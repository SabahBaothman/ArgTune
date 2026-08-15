# ArgTune

ArgTune is a modular pipeline for Arabic function calling developed for the **AISA-ArabicFC 2026 Shared Task (Track A: Core)**.

The system separates tool selection from argument extraction so that each model focuses on a more specific prediction task. ArgTune uses three LoRA-fine-tuned **AISA-AR-FunctionCall-Think (270M)** models:

1. **Tool Selector** — predicts the appropriate function.
2. **General Argument Model** — extracts arguments for the majority of tools.
3. **Specialist Argument Model** — extracts arguments for seven challenging tools.

The extracted arguments are further refined using lightweight **query-grounded post-processing** to improve argument consistency and reduce unsupported predictions.

## Pipeline

```text
Arabic Query
     |
     v
Tool Selector
     |
     v
Predicted Tool
     |
     +----------------------+
     |                      |
     v                      v
General Argument       Specialist Argument
Model                  Model (7 tools)
     |                      |
     +----------+-----------+
                |
                v
Query-Grounded Post-Processing
                |
                v
Final Function Call
```

## Models

The three fine-tuned models are available on Hugging Face:

- **Tool Selector:** https://huggingface.co/SabahBa67/ArgTune-Tool-Selector
- **General Argument Model:** https://huggingface.co/SabahBa67/ArgTune-General-Arguments
- **Specialist Argument Model:** https://huggingface.co/SabahBa67/ArgTune-Specialist-Arguments

## Code

The complete ArgTune inference pipeline is provided in:

`argtune_pipeline.ipynb`

The notebook loads the three models, performs tool selection and argument extraction, routes the seven challenging tools to the specialist model, and applies the query-grounded post-processing rules to produce the final function call.

## Results

On the official **AISA-ArabicFC Track A** test set, ArgTune achieved:

| Metric | Score |
|---|---:|
| Function Accuracy (FnAcc) | 0.9964 |
| Argument Exact Match (ArgEM) | 0.8506 |
| Overall | 0.9089 |
| Rank | 8th |

## Evaluation

Official evaluation uses the normalization and scoring utilities provided by the AISA-ArabicFC organizers. These utilities are not included in this repository.

## Paper

**ArgTune at AISA-ArabicFC: A Modular Pipeline for Arabic Function Calling**

System description paper for **AISA-ArabicFC at ArabicNLP 2026**.
