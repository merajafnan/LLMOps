# EduAssist LLMOps assignment

Name: Meraj Hassan  
BITS ID: 2025ae05085  
BITS Mail ID: 2025ae05085@wilp.bits-pilani.ac.in

## What the project contains

The notebook evaluates three prompt versions on five academic questions. It records request latency and tokenizer usage, estimates local hosting cost, validates JSON output, registers the prompts in MLflow, logs one evaluation run per version, and assigns the `production` alias to version 3.

The submitted execution uses `Qwen/Qwen2.5-0.5B-Instruct` locally because no remote API credential was configured. The code also contains an optional remote API backend that reads `OPENAI_API_KEY` from the environment. No credential is stored in the project.

## Runtime

- Python 3.13.9
- MLflow 3.17.2
- PyTorch 2.15.0
- Transformers 5.22.1
- Operating system: Linux x86_64

## Installation

Create a virtual environment and install the pinned packages:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Transformers downloads the local model from Hugging Face on the first run. About 1 GB of free disk space is required for the model files.

## Environment variables

The local execution does not require a secret. These variables are optional:

```bash
export HF_HOME="$PWD/.hf_cache"
export OPENAI_API_KEY="your-key"       # only for the optional remote backend
export OPENAI_MODEL="gpt-4.1-mini"    # optional remote model selection
```

Keep API keys in the shell environment or a secret manager. Do not place them in the notebook, README, CSV files, or screenshots.

## Reproduce the experiment

1. Activate the virtual environment.
2. Start Jupyter from this directory with `jupyter lab`.
3. Open `MLOps_2025ae05085_Assignment2.ipynb`.
4. Run all cells from top to bottom.
5. Confirm that `latency_tokens_cost.csv`, `prompt_evaluation_results.csv`, `outputs/`, and `mlflow_tracking/` are created.
6. Check that all 15 requests have `success` status and that prompt v3 has a `valid_json` rate of 1.0.

The notebook recreates its own `mlflow_tracking` directory so that prompt versions remain 1, 2, and 3 on a clean rerun.

## Open the MLflow UI

Run this command from the project directory after notebook execution:

```bash
mlflow server \
  --backend-store-uri sqlite:///$PWD/mlflow_tracking/mlflow.db \
  --default-artifact-root file://$PWD/mlflow_tracking/artifacts \
  --host 127.0.0.1 \
  --port 5001 \
  --workers 1
```

Open `http://127.0.0.1:5001`. Use the Model training view to inspect the three experiment runs and their metrics. Use the GenAI Prompts view to inspect versions 1 through 3, compare versions 2 and 3, and verify that `production` points to version 3.

## Cost assumption

The local execution has no token-based API charge. The notebook converts measured inference time to a hosting estimate using the Hugging Face CPU Upgrade price of USD 0.03 per hour, accessed on 1 November 2026. Input and output cost shares are allocated according to token count for the requested table. Actual local hosting is billed by instance time.
