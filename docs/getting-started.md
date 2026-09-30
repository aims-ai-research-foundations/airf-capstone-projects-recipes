# Getting Started

Every recipe runs in one Google Colab notebook on a free T4 GPU.

## 1. Accounts

1. Create a free [Hugging Face account](https://huggingface.co/join).
2. Accept the Gemma licence on each model page you will use: [Gemma 3 1B](https://huggingface.co/google/gemma-3-1b-it) and [Gemma 3 4B](https://huggingface.co/google/gemma-3-4b-it).
3. Create a [Hugging Face access token](https://huggingface.co/settings/tokens) with **Read** access.
4. For recipes with an LLM judge, create a free [Gemini API key](https://aistudio.google.com/apikey) in Google AI Studio. The notebook shows the steps.

## 2. Open a notebook

1. Pick a recipe from the [recipe catalogue](../README.md#-recipe-catalogue) and click **Open in Colab** at the top of its notebook.
2. In Colab, open the key icon (**Secrets**) in the left sidebar, add a secret named `HF_TOKEN` with your token (and `GEMINI_API_KEY` with your Gemini key, if the recipe uses one), and turn on **Notebook access**.
3. Select **Runtime → Change runtime type → T4 GPU**.
4. Run the cells from top to bottom.

## 3. Load a dataset

All recipes use one Hugging Face repository with one configuration per recipe:

```python
from datasets import load_dataset, get_dataset_config_names

REPO = "Similoluwa/capstone-datasets"
print(get_dataset_config_names(REPO))

dataset = load_dataset(REPO, "healthcare_sms_urgency", revision="v5.0")
```

Each configuration has `train`, `validation` and `test` splits.

## 4. Choose a model size

Generation recipes use **Gemma 3 4B** (`google/gemma-3-4b-it`), fine-tuned with QLoRA so it fits a T4; classification recipes use the smaller **Gemma 3 1B** (`google/gemma-3-1b-it`). To run faster, change the model ID in a notebook's Settings cell to the 1B model: results will usually be weaker, which is itself worth comparing.

## 5. Make it your own

Each notebook ends with **What Next?** ideas (the same as the README's **Go Further**). The most useful change is usually to use local data, such as a local language, your country's documents or your institution's help pages, and then re-run the same evaluation.

## Troubleshooting

| Problem | Fix |
|---|---|
| `401` or `GatedRepoError` when loading Gemma | Accept the Gemma licence and check that `HF_TOKEN` is set with notebook access on |
| `CUDA out of memory` | Restart the runtime, then lower `BATCH_SIZE` or use Gemma 3 1B |
| Slow generation | Check the runtime type is T4 GPU; start with the notebook's small test subset |
| Gemini `429` or quota error | Wait a minute and re-run the judge cell; the notebooks rate only about 10 items |
