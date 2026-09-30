# Dataset Card: Build a News Headline Generator

News articles with the headline each was published under, in Swahili, Yoruba, isiZulu, isiXhosa and Tigrinya. [AfriHG](https://arxiv.org/abs/2412.20223) combines XL-Sum (BBC News in African languages) and MasakhaNEWS (BBC, VOA, Isolezwe and other African news sites). The task is to write the headline from the article.

- **Task:** Summarization
- **Splits (train / validation / test):** 25,000 / 5,444 / 5,431
- **Source:** [AfriHG: News Headline Generation for African Languages (Ogunremi et al., 2024)](https://github.com/dadelani/AfriHG)
- **Licence:** [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)
- **Adaptation:** Adapted from the public AfriHG release by keeping five languages, removing articles that contain their own headline and duplicates across splits, and sampling up to 5,000 training articles per language (filtering and subsampling); validation and test keep AfriHG's splits.

## Load it

```python
from datasets import load_dataset

REPO = "Similoluwa/capstone-datasets"
ds = load_dataset(REPO, "news_headlines", revision="v5.0")
```

Hugging Face configuration: `news_headlines` in [`Similoluwa/capstone-datasets`](https://huggingface.co/datasets/Similoluwa/capstone-datasets).

## Columns

| Column | Description |
|---|---|
| `id` | Row id |
| `text` | Article text |
| `headline` | Published headline (the reference) |
| `language` | ISO 639-3 code: `swa`, `yor`, `zul`, `xho`, `tir` |
| `source` | Dataset of origin |

## How it was built

- Source: the `train`, `dev` and `test` files of the public AfriHG release for Swahili, Yoruba, isiZulu, isiXhosa and Tigrinya; `dev` becomes `validation`.
- Whitespace stripped; rows with an empty article or headline removed.
- Articles that contain their own headline word for word removed (2–3% of rows), so the task cannot be solved by copying.
- Duplicate headlines and articles removed within each language, keeping test copies first, then validation, so no test headline or article appears in training.
- Up to 5,000 training articles sampled per language (seed 42); validation and test are kept in full.

## Limitations

- Most articles come from BBC News services in African languages, so the topics and style follow one international broadcaster.
- Articles are news content shared for research and teaching under the non-commercial, share-alike licence of the source datasets.
- The public training files are smaller than the paper reports for Swahili (13,406 vs 18,914), Yoruba (10,735 vs 15,172) and Tigrinya (8,901 vs 12,351); validation and test sizes match the paper.
- Each article has one reference headline, although many headlines can be good; ROUGE against a single reference undervalues valid alternatives.
- Because a few test rows were removed, scores are close to, but not exactly comparable with, the AfriHG paper.

## Citation

Ogunremi, T., Akojenu, S., Soronnadi, A., Adekanmbi, O. and Adelani, D. I. (2024). AfriHG: News headline generation for African Languages. AfricaNLP Workshop at ICLR 2024. https://arxiv.org/abs/2412.20223
