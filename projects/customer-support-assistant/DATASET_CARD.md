# Dataset Card: Build a Customer Support Intent Classifier

Online-banking customer queries, each labelled with one of 77 fine-grained intents.

- **Task:** Classification
- **Splits (train / validation / test):** 8,951 / 995 / 3,075
- **Source:** [BANKING77 (PolyAI)](https://huggingface.co/datasets/legacy-datasets/banking77)
- **Licence:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- **Adaptation:** Adapted from BANKING77 by removing near-duplicate and test-overlapping queries, carving a validation split from the original training data and normalising label names (filtering and re-splitting).

**Labels:** 77 intents (see `features['label'].names`)

## Load it

```python
from datasets import load_dataset

REPO = "Similoluwa/capstone-datasets"
ds = load_dataset(REPO, "customer_support", revision="v5.0")
```

Hugging Face configuration: `customer_support` in [`Similoluwa/capstone-datasets`](https://huggingface.co/datasets/Similoluwa/capstone-datasets).

## Columns

| Column | Description |
|---|---|
| `id` | Row id |
| `text` | Customer query |
| `label` | Intent id (ClassLabel, 77 classes) |
| `label_text` | Intent name, e.g. `card_arrival` |
| `language` | ISO 639-3 code (`eng`) |
| `in_small_test` | True for a 770-row stratified test subset (10 per intent) for fast LLM evaluation |
| `source` | Dataset of origin |

## How it was built

- Near-duplicate queries (ignoring case and punctuation) removed within each split, dropping every copy when copies carry different labels; training queries that also appear in the test set removed.
- Validation carved from the original train split (10%, stratified by intent, seed 42). Test is the official test split minus 5 near-duplicate queries (3,075 rows).
- Label names lowercased and `?` removed (`Refund_not_showing_up` → `refund_not_showing_up`, `reverted_card_payment?` → `reverted_card_payment`).

## Limitations

- English only; a single (banking) domain.
- Training classes are imbalanced (32–168 examples per intent in the train split).

## Citation

Casanueva et al. (2020). Efficient Intent Detection with Dual Sentence Encoders. NLP4ConvAI. https://arxiv.org/abs/2003.04807
