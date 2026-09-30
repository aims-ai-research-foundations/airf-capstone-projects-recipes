# Recipe Guidelines

A recipe is a short, end-to-end research workflow for a capstone project, which a lecturer or learner can understand in two minutes and run in one sitting:

**Implement → Evaluate → Inspect → Reflect → Extend**

## Folder

```text
projects/<recipe-name>/
├── README.md         # the recipe in two minutes
├── DATASET_CARD.md   # the dataset in detail
└── notebook.ipynb    # the experiment

completed/<recipe-name>.ipynb   # a completed run of the notebook, with every output
```

Use lowercase, hyphenated folder names, e.g. `healthcare-sms-urgency`. Keep the notebook in `projects/` free of outputs; the completed run shows learners what to expect.

## README.md

Use exactly these sections, keeping each one short:

| Section | Content |
|---|---|
| The Problem | One sentence on the real-world problem |
| The Solution | One sentence on what the AI system does |
| Who is this for? | Two or three user groups |
| Research Question | One experimentally testable question |
| Dataset Card | Dataset, distribution, task, labels/fields, languages, reference |
| Evaluation | Two or three established metrics, each with one line on what it measures |
| What You'll Learn | Four or five specific outcomes |
| Go Further | Three or four ways to build on the recipe: an improvement, a local adaptation, something new to build |
| References | The dataset and key papers |

## Notebook

Every notebook follows the same structure:

1. **Header:** Open in Colab, View on GitHub and Dataset badges, then the title and category.
2. **Introduction:** Problem, What You'll Build, What You'll Learn.
3. **Setup:** accounts, token and GPU steps.
4. **Install, Imports, Helpers.** Helpers go in their own visible cell so learners can read them; each has type annotations and a one-sentence docstring.
5. **Load Dataset:** splits and a few examples only.
6. **Build and Run the Model:** the reference condition first (e.g. zero-shot, base model), then the technique being tested.
7. **Evaluate:** one results table that answers the research question, with established metrics.
8. **Inspect:** example-level analysis, such as disagreements between approaches, errors, and where the metrics disagree.
9. **Reflect:** four or five questions that use the evidence above and lead towards the learner's own research questions.
10. **What Next? (Extend):** three or four ways to build on the recipe, the same list as the README's Go Further section.

Do not assume the more sophisticated method will win. Design the comparison so learners can discover an unexpected result and investigate why it happened.

Code standards:

- Write as little code as the research idea needs. Prefer established libraries.
- Keep the main experiment visible. Only reusable plumbing goes in helpers.
- Follow PEP 8, use type annotations and write one-sentence docstrings.
- Comment only to explain *why*.
- No decorative banners or emojis.
- Set random seeds (`SEED = 42`) and pin the dataset revision.
- Put settings a learner may change (`MODEL_ID`, `N_EVAL`, `BATCH_SIZE`) in one cell near the top.

## Responsible framing

- Health, safety and public-service recipes support human decisions; they never replace them. Say so in the README and the notebook.
- Name the dataset's limitations, such as its country, language, synthetic content or label noise, in the README and the dataset card.
