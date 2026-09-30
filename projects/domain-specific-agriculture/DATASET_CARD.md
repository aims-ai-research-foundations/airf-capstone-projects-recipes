# Dataset Card: Build a Domain-Specific Agricultural Small Language Model

Questions asked by smallholder farmers in Uganda about cassava, maize, beans and general crop management, with short answers written by agricultural experts (the paper credits an expert team at Uganda's National Crops Resources Research Institute, NaCRRI). Questions were collected in farmer interviews in Kole district and through a pilot app in eastern and central Uganda.

- **Task:** Conversational Assistant
- **Splits (train / validation / test):** 2,503 / 137 / 234
- **Source:** [AgroQA Dataset (Omara et al., 2023)](https://github.com/JonaOmara/AgroQA-Dataset)
- **Licence:** [MIT](https://opensource.org/license/mit)
- **Adaptation:** Adapted from AgroQA by removing empty and duplicate rows, grouping paraphrased questions, removing hand-checked wrong answers and creating train/validation/test splits by question group; question and answer text is otherwise unchanged (filtering and re-splitting).

## Load it

```python
from datasets import load_dataset

REPO = "Similoluwa/capstone-datasets"
ds = load_dataset(REPO, "agriculture_qa", revision="v5.0")
```

Hugging Face configuration: `agriculture_qa` in [`Similoluwa/capstone-datasets`](https://huggingface.co/datasets/Similoluwa/capstone-datasets).

## Columns

| Column | Description |
|---|---|
| `id` | Row id |
| `question` | Farmer's question, as recorded (spelling and speech-to-text errors kept) |
| `answer` | Expert reference answer |
| `crop` | `cassava`, `maize`, `beans` or `general` |
| `question_group` | Id of the group of paraphrased questions this row belongs to (a group is always in one split) |
| `language` | ISO 639-3 code: `eng` |
| `country` | ISO 3166 alpha-3 code: `UGA` |
| `source` | Dataset of origin |

## How it was built

- Source: `AgroQA Dataset.csv` at commit `437e21a` of the GitHub repository (3,044 rows).
- Whitespace stripped; rows with an empty question or answer removed; duplicate question–answer pairs (after lowercasing and removing punctuation) removed.
- Paraphrased questions (word-set Jaccard similarity of 0.8 or more) are grouped, and every group is kept in a single split so that no paraphrase of a test question is seen in training.
- Validation (150) and test (250) hold one question per group, sampled with seed 42 and stratified by crop. They only use groups with a single answer that is not just "yes"/"no" and does not begin with "depends", so each has one clear reference. Other paraphrases in those groups are removed.
- Every validation and test answer was then read by hand, and answers that are wrong or do not answer the question were removed, so these splits are slightly smaller than sampled.
- Train holds the remaining groups, including paraphrases. Where the same question has several expert answers, one is kept at random (seed 42).

## Limitations

- Uganda only, mainly cassava, maize and beans; very little on livestock, irrigation or climate-smart farming. It is not a pan-African dataset.
- Expert answers are short (median 6 words), sometimes generic, and collected around 2021–22. They are not validated advice and must not be deployed without review by extension officers.
- Only validation and test answers were checked by hand; train answers can be generic or occasionally wrong, which the fine-tuned model may learn.
- Some answers name commercial products (pesticides and herbicides).
- Questions keep farmers' spelling and speech-to-text errors, e.g. 'Sunday soil' for sandy soil.
- The public CSV has 3,044 rows, fewer than the 3,939 pairs reported in the paper, and is not lowercased as the paper describes, so it is not the paper's exact training file.
- Short references reward short answers: report answer length next to ROUGE and token F1, and use the LLM judge and human preference as well.

**Licence notice** (reproduced as the licence requires)

```text
MIT License

Copyright (c) 2022 Jonathan Omara

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Citation

Omara, J., Talavera, E., Otim, D., Turcza, D., Ofumbi, E. and Owomugisha, G. (2023). A field-based recommender system for crop disease detection using machine learning. Frontiers in Artificial Intelligence, 6, 1010804. https://doi.org/10.3389/frai.2023.1010804
