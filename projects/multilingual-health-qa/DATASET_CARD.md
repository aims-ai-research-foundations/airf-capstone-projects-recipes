# Dataset Card: Build a Multilingual Health QA System

Community questions on maternal, sexual and reproductive health (HIV, STIs, contraception, gender-based violence, PrEP) with reference answers in Swahili (Kenya), Amharic (Ethiopia), Luganda (Uganda), Akan/Twi (Ghana) and English (Kenya).

- **Task:** Conversational Assistant
- **Splits (train / validation / test):** 4,000 / 500 / 750
- **Source:** [Zindi – Multilingual Health Question Answering in Low-Resource African Languages Challenge (ITU / HASH, 2026)](https://zindi.world/competitions/multilingual-health-question-answering-in-low-resource-african-languages-challenge)
- **Licence:** [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
- **Adaptation:** Adapted from the Zindi challenge data by keeping five language subsets, filtering duplicates and answer length, and re-splitting the labelled files by answer so that no reference answer appears in two splits (subsetting, filtering and re-splitting).

## Load it

```python
from datasets import load_dataset

REPO = "Similoluwa/capstone-datasets"
ds = load_dataset(REPO, "multilingual_health_qa", revision="v5.0")
```

Hugging Face configuration: `multilingual_health_qa` in [`Similoluwa/capstone-datasets`](https://huggingface.co/datasets/Similoluwa/capstone-datasets).

## Columns

| Column | Description |
|---|---|
| `id` | Row id |
| `question` | Health question |
| `answer` | Reference answer |
| `language` | ISO 639-3 code: `swa`, `amh`, `lug`, `aka`, `eng` |
| `country` | ISO 3166 alpha-3 code: `KEN`, `ETH`, `UGA`, `GHA` |
| `subset` | Original Zindi subset, e.g. `Swa_Ken` |
| `source` | Dataset of origin |

## How it was built

- Five subsets kept: Swa_Ken, Amh_Eth, Lug_Uga, Aka_Gha, Eng_Ken.
- Answers kept if 5–150 words (keeps generation feasible for Gemma 1B); duplicate questions removed; HTML entities decoded.
- 7 Amharic source rows whose question and answer are swapped (the question is not in Ethiopic script) are removed.
- The Zindi Train and Val files (both with reference answers) are pooled. Many answers are shared by several paraphrased questions, so the data are split **by answer**: no answer appears in more than one split.
- test (150/subset) and validation (100/subset) contain one question per answer; train (800/subset) is sampled from the remaining rows and may contain paraphrases of the same answer. Seed 42.
- The official Zindi Test file is not included because it has no reference answers.
- Built from the file-identical mirror `Bedru/zindi-multilingual-health-qa`; original Zindi IDs are not kept (their hash suffixes repeat across files).

## Limitations

- Sensitive subject matter (sexual and reproductive health, gender-based violence). Generated answers must not be used as medical advice.
- Reference answers are long and free-form, so Exact Match is near zero; use ROUGE, semantic similarity and LLM-as-a-Judge.
- Some local-language subsets may be translations of English subsets.
- In the source, most Swahili, Luganda and English (Kenya) answers are shared by several paraphrased questions; the answer-grouped split prevents these from leaking into the test set.

## Citation

ITU and HASH (Hub for AI in Maternal, Sexual and Reproductive Health) (2026). Multilingual Health Question Answering in Low-Resource African Languages Challenge. Zindi.
