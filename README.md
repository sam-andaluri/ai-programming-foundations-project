# AI Programming Foundations Project: Titanic Data Workflow

A complete, reproducible data workflow analyzing the Titanic passenger dataset. This project demonstrates data ingestion, cleaning, exploratory analysis, and visualization as a foundation for future ML/DL projects.

**Dataset:** [Titanic - Machine Learning from Disaster](https://www.kaggle.com/c/titanic) (891 rows, 12 columns)

## Project Description

This project uses the Titanic passenger dataset to build a reusable analysis workflow in a Jupyter notebook. The notebook loads the data with Pandas, applies documented cleaning functions, performs exploratory analysis, and creates three visualizations that highlight relationships between survival, passenger class, sex, age, fare, and cabin availability.

## How to Run the Project

### 1. Install `uv`

If `uv` is not installed yet, install it with one of the following methods:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Then restart your shell, or reload your shell configuration so the `uv` command is available.

### 2. Create a Local Python Environment

```bash
uv venv .venv --python 3.8
source .venv/bin/activate
```

Python 3.8 is the minimum supported version for this project. If Python 3.8 is not available locally, you can create the environment with any newer compatible Python version supported by the project.

If `uv` cannot find your preferred Python interpreter automatically, point it to a local executable:

```bash
uv venv .venv --python /path/to/python3.8
source .venv/bin/activate
```

### 3. Sync Dependencies

```bash
uv pip sync --python .venv/bin/python requirements.txt
```

If you update the direct dependencies in `requirements.in`, regenerate the lock file before syncing:

```bash
uv pip compile requirements.in --python-version 3.8 --upgrade -o requirements.txt
uv pip sync requirements.txt
```

If you are using a newer Python version, replace `3.8` in the compile command with that interpreter version so the lock file matches your local environment.

### 4. Run the Notebook

Open and run `data_workflow.ipynb` in Jupyter:

```bash
jupyter notebook data_workflow.ipynb
```

Or using JupyterLab:

```bash
jupyter lab data_workflow.ipynb
```

Run all cells from top to bottom (Cell > Run All).

The notebook was verified to execute from top to bottom in a local virtual environment using Jupyter `nbconvert`.

## Project Structure

```
ai-programming-foundations-project/
├── data_workflow.ipynb    # Main analysis notebook
├── titanic.csv            # Dataset
├── module_summary.md      # Written summary with citations
├── requirements.in        # Python dependencies
├── requirements.txt       # Python dependencies lock file 
├── README.md              # This file
```

## Requirements

- Python 3.8+
- `requirements.in` lists the direct project dependencies
- `requirements.txt` is the compiled lock file used to reproduce the environment

## Cleaning Reflection

The cleaning steps were necessary because the raw dataset contains missing values and inconsistent feature usefulness. `Age` had substantial missingness, so I used grouped median imputation by passenger class and sex to preserve demographic structure better than a single global fill value. `Embarked` had two missing values, which were filled with the modal embarkation port because the gap was small and the feature is categorical. `Cabin` was mostly missing, so instead of forcing noisy imputation, I converted it into a simpler `HasCabin` indicator that preserved whether a cabin record existed.

## Bias Awareness

Poor cleaning choices could introduce bias into the analysis. A global age imputation strategy could flatten meaningful class and sex differences, which would distort later survival comparisons. Dropping all rows with missing values would also disproportionately remove passengers from some groups and could make the sample less representative. Even the chosen cleaning approach has tradeoffs: converting `Cabin` to `HasCabin` removes deck-level detail, and grouped median imputation assumes missing ages behave similarly within each class and sex group. These limitations are acknowledged in the notebook and summary so the analysis is not presented as more certain than the data allows.

## Reflection Questions

### How would this workflow change for a machine learning project?

For an ML workflow, the notebook would need to move beyond descriptive analysis into train/validation/test splitting, feature encoding, leakage checks, and metric-based model evaluation. I would separate preprocessing into reusable functions or a pipeline object so the exact same transformations could be applied during training and inference. I would also track experiments more formally, including saved model artifacts, evaluation tables, and configuration details.

### How would I prepare this dataset for a neural network?

A neural network would require stricter preprocessing than the current exploratory notebook. Numeric features would need scaling, categorical features such as `Sex` and `Embarked` would need encoding, and the final feature matrix would need to be fully numeric with no missing values. I would also consider creating trainable embeddings or one-hot encodings for categorical columns, then export the cleaned arrays or tensors into a dedicated training script rather than relying on notebook cells alone.

### Where could agentic automation help?

Agentic automation could speed up repeated workflow tasks such as profiling missing values, generating baseline charts, validating notebook outputs, checking citation formatting, and testing whether the notebook still executes after edits. It would also be useful for suggesting refactors from notebook code into reusable modules, summarizing changes between commits, and flagging when documentation no longer matches the actual analysis outputs.

## Professional Workflow Notes

This submission is designed to resemble a reusable foundation for later ML and deep learning work. The notebook is organized into clear workflow stages, the cleaning logic is encapsulated in functions with docstrings, the outputs are interpretable, and the repository history shows incremental development on a separate `develop` branch before eventual merge to `main`.
