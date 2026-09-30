# Dataset Card: Build a Synthetic Dataset for an African Language

Swahili utterances written by native speakers for 40 intents across banking, travel, home, utility and kitchen/dining. The train split marks a 200-example seed set that learners use to prompt Gemma to generate synthetic training data; validation and test are real and are never replaced by synthetic data.

- **Task:** Classification
- **Splits (train / validation / test):** 2,238 / 320 / 640
- **Source:** [INJONGO Intent (Masakhane), Swahili](https://huggingface.co/datasets/masakhane/InjongoIntent)
- **Licence:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- **Adaptation:** Adapted from INJONGO Intent (Swahili) by keeping only text and intent, removing duplicate and cross-split repeated utterances, and marking a 200-example seed set (filtering and seed annotation; official splits kept).

**Labels:** 40 intents (see `features['label'].names`)

## Load it

```python
from datasets import load_dataset

REPO = "Similoluwa/capstone-datasets"
ds = load_dataset(REPO, "african_language_synthetic_data", revision="v5.0")
```

Hugging Face configuration: `african_language_synthetic_data` in [`Similoluwa/capstone-datasets`](https://huggingface.co/datasets/Similoluwa/capstone-datasets).

## Columns

| Column | Description |
|---|---|
| `id` | Row id |
| `text` | Utterance |
| `label` | Intent id (ClassLabel, 40 classes) |
| `label_text` | Intent name, e.g. `pay_bill` |
| `language` | ISO 639-3 code (`swa`) |
| `origin` | `real` (human-written) or `synthetic` (model-generated) |
| `in_seed` | True for the 200 real seed examples (5 per intent) used to prompt generation |
| `generator` | Model that generated the row (empty for real rows) |
| `source` | Dataset of origin |

## How it was built

- Official INJONGO `swa` splits are kept (2,240 / 320 / 640). Duplicate utterances, and train/validation utterances that also occur in a later split (case- and punctuation-insensitive), are removed; the test split is unchanged.
- 5 train examples per intent are marked `in_seed` (seed 42).
- Slot annotations (`spans`, `target`) are dropped; only text and intent are kept.

## Limitations

- Licence metadata differs across sources (paper: CC BY 4.0; Hugging Face card: Apache-2.0; GitHub code: GPL-3.0). We follow the paper's CC BY 4.0 data statement.
- Synthetic rows are for training only; always report results on the real test split.
- Models trained on Gemma-generated data are Gemma Model Derivatives under the Gemma Terms of Use.

## Citation

Yu, H. et al. (2025). INJONGO: A Multicultural Intent Detection and Slot-filling Dataset for 16 African Languages. ACL 2025. https://aclanthology.org/2025.acl-long.464/
