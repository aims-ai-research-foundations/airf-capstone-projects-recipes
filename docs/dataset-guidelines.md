# Dataset Guidelines

Every recipe uses a configuration of the Hugging Face repository [`Similoluwa/capstone-datasets`](https://huggingface.co/datasets/Similoluwa/capstone-datasets).

## Requirements

A dataset can be used only if it is:

- **Public:** downloadable without a paid account or special approval.
- **Annotated:** has human labels or reference answers, including for the test set.
- **Redistributable:** its licence allows us to share an adapted copy. Non-commercial licences are acceptable; "no derivatives" and "no redistribution" terms are not.
- **Small and clear:** a defined supervised task, with a size that runs on a free T4 GPU.

Synthetic data may be used for training, but the test set must be real, human-written or human-validated data.

## Structure

- **Configuration name:** lowercase `snake_case` named after the task, e.g. `healthcare_sms_urgency`.
- **Splits:** exactly `train`, `validation` and `test`.
- **Files:** `<config>/<split>.parquet`, with a CSV copy at `<config>/<split>.csv`.

| Column | Used in | Meaning |
|---|---|---|
| `id` | all | Unique row id within the configuration |
| `language` | all | ISO 639-3 code, e.g. `eng`, `swa`, `hau` |
| `source` | all | Dataset of origin |
| `text`, `label`, `label_text` | classification | Input, integer class label, label name |
| `in_small_test` | classification, QA | Stratified test subset for fast LLM evaluation |
| `question`, `answer` | QA | Question and reference answer |

Label names are lowercase `snake_case`, e.g. `emergency`, `cash_withdrawal_not_recognised`.

## Building a configuration

1. Write `build/<config>.py`. It downloads the source, builds the splits with `SEED = 42` and holds the card text in `META`.
2. Remove duplicates and any text shared between splits. For question answering, make sure no reference answer appears in two splits.
3. Run `python build_all.py`, then `python verify.py`. Every check must pass.
4. Read at least 20 random rows yourself.

## Dataset card

Each configuration's card must state:

- The source dataset, with a link, citation and licence.
- An **Adaptation** sentence saying what was changed and how (e.g. filtering, re-splitting, text transformation, label mapping).
- Every column and every label.
- How the splits were built.
- Limitations, including country, language, synthetic content, sensitive content and label noise.

## Versioning

Releases are tagged `vMAJOR.MINOR`, and notebooks pin `revision="v5.0"`.

- A new configuration or column is a MINOR release.
- Any change to existing rows, splits or labels, or removing a configuration, is a MAJOR release.
- Record every change in `CHANGELOG.md`.
