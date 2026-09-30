# Dataset Card: Build Your Own Personalised AI Tutor

A small Tutor Preference Dataset for learning preference alignment. Each row is a teaching question, a learner profile, two tutor responses and which one that learner prefers, with a reason. The same two responses can be preferred differently by different learners: that is the point.

- **Task:** Conversational Assistant
- **Splits (train / validation / test):** 392 / 56 / 112
- **Source:** [AIRF Capstone Project Recipes (this library)](https://github.com/aims-ai-research-foundations/airf-capstone-project-recipes)
- **Licence:** [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/)
- **Adaptation:** Created for this library: responses written with the help of a language model and reviewed by hand, and preferences assigned from stated learner profiles (original dataset, not adapted from another source).

## Load it

```python
from datasets import load_dataset

REPO = "Similoluwa/capstone-datasets"
ds = load_dataset(REPO, "tutor_preferences", revision="v5.0")
```

Hugging Face configuration: `tutor_preferences` in [`Similoluwa/capstone-datasets`](https://huggingface.co/datasets/Similoluwa/capstone-datasets).

## Columns

| Column | Description |
|---|---|
| `id` | Row id |
| `prompt_id` | Id of the teaching question (the same question appears in several pairs) |
| `subject` | Subject of the question, e.g. `mathematics`, `science`, `computing` |
| `prompt` | The learner's question |
| `learner_profile` | `beginner`, `advanced` or `visual` |
| `profile_description` | What this learner prefers, in words |
| `response_a` | First tutor response |
| `response_b` | Second tutor response |
| `style_a` | Style of response A: `scaffolded`, `technical` or `visual` |
| `style_b` | Style of response B |
| `preference` | `A` or `B`: the response this learner prefers |
| `reason` | Why this learner prefers it |
| `source` | Dataset of origin |

## How it was built

- 80 teaching questions across mathematics (32), science (26), computing (16) and other subjects (6), each with three responses: scaffolded (hints before answers, simple language, encouragement), technical (precise terminology, concise, ends with a challenge question) and visual (an everyday analogy with numbered steps).
- Responses were written with the help of a language model (Claude, Anthropic) for this library and reviewed by hand for factual accuracy and style.
- Pairs are formed only where a profile has a clear preference: beginner prefers scaffolded over visual over technical (3 pairs per question); advanced prefers technical over the other two (2); visual prefers visual over the other two (2).
- The order of the two responses in each pair is random (seed 42), so the preferred response is A about half of the time.
- Splits are by question, so no question appears in two splits: 16 test questions (including 'Explain gravity to a 10-year-old'), 8 validation questions and the rest in train.
- The companion configuration `tutor_questions` holds 120 further questions without responses (80 train, 10 validation, 30 test), used to generate and select the tutor's own answers during alignment and to compare the tutor before and after.

## Limitations

- Preferences are assigned from stated learner profiles, not collected from real learners; they show how preference data works rather than what learners actually prefer.
- The three styles are deliberately distinct, which makes preferences easy to learn; real preference data is noisier and more subtle.
- Responses were written in English for secondary-school topics and reflect one writer's view of each style.

## Citation

AIMS AI Research Foundations (2026). Tutor Preference Dataset. AIRF Capstone Project Recipes.
