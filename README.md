# Decision Tree + ML/NLP Review Analysis Notebooks

This folder contains two **Jupyter notebooks** used for modelling and analysis over the Online Product Reviews dataset.

- **`Decision_Tree.ipynb`**: builds a **Decision Tree Regressor** to predict review *helpfulness votes* (with a log-transform), and explores model stability across `max_depth`.
- **`ML_NLP_Review_Analysis.ipynb`**: builds a more feature-rich ML pipeline (including **TF–IDF text features**) and evaluates models (e.g., Random Forest), using both review/meta JSON and summarised annotation data.

> **Run these notebooks from the repository root** (they use relative paths).

---

## Getting the input data

The required input data is available at:

```text
https://github.com/jamumford/Online-Product-Reviews
```

### Directory mapping (important)

These notebooks expect some directory names that differ from the dataset repo. The mapping is:

| Notebook expects | In `Online-Product-Reviews` repo | What to do |
|---|---|---|
| `ML_datasets/ML_datasets/` | `ML_datasets` | Either **create** `ML_datasets/ML_datasets/` and copy/symlink JSONs into it, **or** edit the notebook path to `ML_datasets`. |
| `Review_Metadata_all_product_categories/` | `Amazon/Meta_data` | Either **rename/copy/symlink** `Amazon/Meta_data` to `Review_Metadata_all_product_categories`, **or** edit the notebook path to `Amazon/Meta_data`. |
| `Summary_Annotations/` | `Summary_Annotations` | Same name—place it in the repo root as-is. |

### Recommended setup option: symlinks (macOS/Linux)

If you have cloned `Online-Product-Reviews` adjacent to (or inside) this repo, you can symlink the folders to the expected names:

```bash
# Example: if Online-Product-Reviews is a sibling directory
ln -s ../Online-Product-Reviews/ML_datasets ML_datasets
ln -s ../Online-Product-Reviews/Amazon/Meta_data Review_Metadata_all_product_categories
ln -s ../Online-Product-Reviews/Summary_Annotations Summary_Annotations

# If Decision_Tree.ipynb expects ML_datasets/ML_datasets/, create a nested link:
mkdir -p ML_datasets/ML_datasets
# Link or copy JSONs into ML_datasets/ML_datasets (choose one approach):
ln -s ../*.json ML_datasets/ML_datasets/ 2>/dev/null || true
```

If symlinks are inconvenient (Windows or restricted environments), just copy the directories instead.

---

## Installation

Create a virtual environment and install dependencies:

```bash
python -m venv .venv
source .venv/bin/activate  # (Windows: .venv\Scripts\activate)
pip install -U pip
pip install jupyter numpy pandas matplotlib seaborn scipy scikit-learn joblib imbalanced-learn nltk
```

### NLTK resources (required for `ML_NLP_Review_Analysis.ipynb`)

The notebook downloads these at runtime:

- `punkt`
- `stopwords`
- `wordnet`

If you’re running offline, you’ll need to pre-download them:

```python
import nltk
nltk.download("punkt")
nltk.download("stopwords")
nltk.download("wordnet")
```

---

## Notebook details

### 1) `Decision_Tree.ipynb`

#### Inputs (read from disk)
- **Directory:** `ML_datasets/ML_datasets/`
- **Files:** all `*.json` files in that directory

The notebook loads each JSON file and constructs a modelling dataset (features include, for example, rating and reviewer history statistics when present), then trains a `DecisionTreeRegressor` on a log-transformed helpful-vote target.

#### Outputs (written)
- `label_encoder.pkl` (saved with `joblib.dump`)

> There is also commented-out code that can write a CSV (e.g., `helpful_votes_with_rating.csv`), but it is not produced unless you uncomment it.

---

### 2) `ML_NLP_Review_Analysis.ipynb`

#### Inputs (read from disk)
- **Directory:** `Review_Metadata_all_product_categories/`
  - **Files:** all `*.json` files
- **Directory:** `Summary_Annotations/`
  - **Files:** all `*.json` files

The notebook combines review/meta information with summarised annotation data, engineers features (including text features via `TfidfVectorizer`), and trains/evaluates ML models (e.g., Random Forest). It also saves label encoders for key categorical variables.

#### Outputs (written)
- `category_label_encoder.pkl`
- `reviewerID_label_encoder.pkl`
- `asin_label_encoder.pkl`

---

## How to run

From the repo root:

```bash
source .venv/bin/activate  # (Windows: .venv\Scripts\activate)
jupyter notebook
```

Open and run the notebooks:

- `Decision_Tree.ipynb`
- `ML_NLP_Review_Analysis.ipynb`

---

## License

This repository is released under the **Creative Commons Zero v1.0 Universal (CC0 1.0)** public-domain dedication (often referred to as “CC0 1.0 Universal”).  
You may copy, modify, distribute, and perform the work, even for commercial purposes, all without asking permission.

- Human-readable summary (deed): https://creativecommons.org/publicdomain/zero/1.0/
- Full legal code: https://creativecommons.org/publicdomain/zero/1.0/legalcode

> CC0 includes a warranty disclaimer and limitation of liability. See the legal code for details.
