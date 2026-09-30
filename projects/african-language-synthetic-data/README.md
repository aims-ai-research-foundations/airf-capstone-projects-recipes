# Build a Synthetic Dataset for an African Language

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/african-language-synthetic-data/notebook.ipynb)
[![View on GitHub](https://img.shields.io/badge/View%20on-GitHub-black?logo=github)](https://github.com/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/african-language-synthetic-data/notebook.ipynb)
[![Dataset](https://img.shields.io/badge/Dataset-Hugging%20Face-yellow?logo=huggingface)](https://huggingface.co/datasets/Similoluwa/capstone-datasets/viewer/african_language_synthetic_data)

**Project Category:** Classification

**Model:** Gemma 1B / 4B

**Fine-tuning:** QLoRA (Gemma 1B)

**Recommended GPU:** NVIDIA T4, 16 GB VRAM (free Google Colab)

## 🎯 The Problem

Most African languages lack the labelled data needed to train and evaluate language models.

## 💡 The Solution

A synthetic dataset for training a language model on an African language task: generated with Gemma 4B, filtered, documented, and tested by fine-tuning Gemma 1B.

## 👥 Who is this for?

- Researchers building datasets for African languages
- NLP communities such as Masakhane
- Lecturers teaching data-centric AI

## 🔬 Research Question

> Under what conditions does Gemma-generated synthetic data help a language model learn a Swahili intent task when real labelled data is scarce?

## 📊 Dataset Card

- **Dataset:** INJONGO Intent (Swahili) (config `african_language_synthetic_data`; see [DATASET_CARD.md](DATASET_CARD.md))
- **Distribution:** 2,238 train / 320 validation / 640 test
- **Task:** Classification
- **Labels / fields:** Utterance and one of 40 intents; `origin` (real or synthetic) and `in_seed` (200 seed examples)
- **Languages:** Swahili
- **Reference:** https://huggingface.co/datasets/masakhane/InjongoIntent

## 📏 Evaluation

**Accuracy:** Measures the proportion of real test utterances assigned the correct intent.

**Macro-F1:** Measures intent-classification performance on the real test set, giving each of the 40 intents equal weight.

**Dataset quality analysis:** Measures how many generated utterances survive each filter, Gemini ratings of a sample, and native-speaker review.

## 🧠 What You'll Learn

- Design controllable generation prompts from a small seed set
- Filter synthetic text with language identification and embedding-based deduplication
- Fine-tune a language model on real, synthetic and combined data
- Write a dataset card and publish a dataset on Hugging Face

## 🚀 Getting Started

1. Open `notebook.ipynb`.
2. Open the notebook in Google Colab.
3. Enable a GPU if required.
4. Follow the setup instructions in the notebook.
5. Run the notebook from beginning to end.

To see every output before you run it, open the [completed run](../../completed/african-language-synthetic-data.ipynb) of this notebook.

## 🔭 Go Further

- Repeat the pipeline for Hausa, Yoruba or isiZulu (all in INJONGO)
- Compare Gemma 1B and Gemma 4B as data generators
- Vary the amount of synthetic data and plot its effect on test macro-F1
- Generate data for a new set of intents that has no labelled data at all, such as questions to an agricultural extension service, and label a small real test set by hand

## ❓ Frequently Asked Questions

<details>
<summary>What is the Gemma 3 1B model?</summary>

Gemma 3 1B is a small open-weight language model from Google with 1 billion parameters and a 32K-token context window, trained on 2 trillion tokens covering more than 140 languages. It is small enough to run and fine-tune on a free Colab T4 GPU, which makes it a good starting point for experiments.

[Gemma 3 model card](https://ai.google.dev/gemma/docs/core/model_card_3) · [google/gemma-3-1b-it](https://huggingface.co/google/gemma-3-1b-it)

</details>

<details>
<summary>When should I use Gemma 3 4B instead of 1B?</summary>

Gemma 3 4B has 4 billion parameters, a 128K-token context window and was trained on 4 trillion tokens. It usually gives better answers, especially in African languages and as an LLM judge, but it is slower and uses more GPU memory. Change `MODEL_ID` in the notebook's Settings cell to switch.

[google/gemma-3-4b-it](https://huggingface.co/google/gemma-3-4b-it)

</details>

<details>
<summary>Can I publish a dataset generated with Gemma?</summary>

Yes. Google claims no rights in Gemma's outputs, so your dataset can keep the seed data's licence. Generation must follow the Gemma Prohibited Use Policy, and any model you train on the data counts as a Gemma Model Derivative under the Gemma Terms of Use. Say so in your dataset card.

[Gemma Terms of Use](https://ai.google.dev/gemma/terms) · [Prohibited Use Policy](https://ai.google.dev/gemma/prohibited_use_policy)

</details>

<details>
<summary>How are near-copies of the seed examples detected?</summary>

An embedding model (EmbeddingGemma) turns each sentence into a vector of numbers so that sentences with similar meaning get similar vectors. A generated utterance whose vector is almost identical to a seed example's, or to another generated utterance's (cosine similarity above a threshold), is a near-copy that adds little new information, so it is removed. EmbeddingGemma covers more than 100 languages.

[EmbeddingGemma](https://huggingface.co/google/embeddinggemma-300m) · [Sentence Transformers](https://sbert.net)

</details>

<details>
<summary>What is QLoRA and why do we use it?</summary>

QLoRA loads the base model in 4-bit precision and trains only small low-rank adapter (LoRA) matrices added to it. Memory use drops enough to fine-tune a language model on a single free T4 GPU, while keeping quality close to full fine-tuning.

[QLoRA paper](https://arxiv.org/abs/2305.14314) · [Hugging Face PEFT](https://huggingface.co/docs/peft/index) · [TRL SFTTrainer](https://huggingface.co/docs/trl/sft_trainer)

</details>

<details>
<summary>What is LLM-as-a-Judge, and why Gemini?</summary>

A second, stronger language model rates generated answers against clear criteria (here on a 1–5 scale, normalised to 0–1). It captures qualities such as correctness and helpfulness that word-overlap metrics miss. A model should not judge its own outputs, so an independent model (Gemini) is used, on a small sample, as a demonstration; check some ratings yourself.

[Judging LLM-as-a-Judge](https://arxiv.org/abs/2306.05685) · [Gemini API](https://ai.google.dev/gemini-api/docs)

</details>

## 📚 References

1. Yu et al. (2025). INJONGO: A Multicultural Intent Detection and Slot-filling Dataset for 16 African Languages. ACL 2025. https://aclanthology.org/2025.acl-long.464/
2. Gyamfi et al. (2026). Synthetic Data Generation Pipeline for Low-Resource Swahili Sentiment Analysis. AfricaNLP 2026. https://aclanthology.org/2026.africanlp-main.12/
3. Gemma Team, Google DeepMind (2025). Gemma 3 Technical Report. https://arxiv.org/abs/2503.19786
