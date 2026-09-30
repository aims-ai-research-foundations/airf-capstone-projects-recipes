# Dataset Card: Classify Healthcare SMS by Urgency

1,256 short patient or relative text messages rewritten from 1,267 emergency-department records whose urgency was assigned by three triage experts (KTAS 1–5). KTAS 1–2 → emergency, 3 → urgent, 4–5 → routine.

- **Task:** Classification
- **Splits (train / validation / test):** 879 / 188 / 189
- **Source:** [KTAS emergency department triage dataset (Moon et al., PLOS ONE 2019, S1 Appendix)](https://doi.org/10.1371/journal.pone.0216972)
- **Licence:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- **Adaptation:** Adapted from the KTAS triage records by rewriting each record's chief complaint, age, sex, injury, consciousness and pain score as an SMS-style message with fixed templates, and mapping the five expert KTAS levels to three urgency classes (text transformation and label mapping).

**Labels:** `emergency`, `urgent`, `routine`

## Load it

```python
from datasets import load_dataset

REPO = "Similoluwa/capstone-datasets"
ds = load_dataset(REPO, "healthcare_sms_urgency", revision="v5.0")
```

Hugging Face configuration: `healthcare_sms_urgency` in [`Similoluwa/capstone-datasets`](https://huggingface.co/datasets/Similoluwa/capstone-datasets).

## Columns

| Column | Description |
|---|---|
| `id` | Row id |
| `text` | SMS-style message (model input) |
| `label` | 0 = `emergency`, 1 = `urgent`, 2 = `routine` (ClassLabel) |
| `label_text` | Label name |
| `language` | ISO 639-3 code (`eng`) |
| `age` | Patient age in years |
| `sex` | `female` or `male` |
| `chief_complaint_raw` | Original clinical chief complaint (analysis only) |
| `ktas_expert` | Expert KTAS level 1–5 (source of `label`) |
| `ktas_nurse` | Triage nurse's original KTAS level 1–5: a human baseline, never a model input |
| `source` | Dataset of origin |

## How it was built

- Chief complaints translated from clinical shorthand to plain language with a hand-written mapping covering all 427 source strings (Korean entries translated).
- Alert patients write in the first person (with pain score if recorded); patients who are not alert are described by a relative (voice/pain/unresponsive). One of several fixed phrasings is chosen per row (seed 42). Vital signs are not included.
- Duplicate messages removed (every copy dropped when copies have different labels); split 70/15/15 stratified by label (seed 42).

## Limitations

- A triage aid for prioritising human review only, not a clinical decision system.
- Experts also used vital signs, which the messages do not contain, so text-only accuracy has a ceiling.
- Template messages are cleaner than real SMS; data come from patients over 15 at two Korean emergency departments, not African community health services.
- Headline metric: emergency-class recall. The nurse baseline (`ktas_nurse` mapped to 3 classes) recalls 86% of emergencies (209 of 243 across all splits).

## Citation

Moon, S.-H., Shim, J. L., Park, K.-S. and Park, C.-S. (2019). Triage accuracy and causes of mistriage using the Korean Triage and Acuity Scale. PLOS ONE 14(9): e0216972. https://doi.org/10.1371/journal.pone.0216972
