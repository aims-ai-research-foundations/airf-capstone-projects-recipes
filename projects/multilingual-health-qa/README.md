# Build a Multilingual Health QA System

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/multilingual-health-qa/notebook.ipynb)
[![View on GitHub](https://img.shields.io/badge/View%20on-GitHub-black?logo=github)](https://github.com/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/multilingual-health-qa/notebook.ipynb)
[![Dataset](https://img.shields.io/badge/Dataset-Hugging%20Face-yellow?logo=huggingface)](https://huggingface.co/datasets/Similoluwa/capstone-datasets/viewer/multilingual_health_qa)

**Project Category:** Conversational Assistant

**Model:** Gemma 4B

**Fine-tuning:** None

**Recommended GPU:** NVIDIA T4, 16 GB VRAM (free Google Colab)

## 🎯 The Problem

Many communities across Africa have limited access to reliable health information in their local languages.

## 💡 The Solution

A multilingual language model that answers health questions in African languages, prompted with and without a few example answers in the same language.

## 👥 Who is this for?

- Community health workers
- Health-information platforms and hotlines
- Researchers working on low-resource African languages

## 🔬 Research Question

> Do a few example answers in the prompt improve a language model's health answers in African languages, or do they only change the style? Does the effect depend on the language?

## 📊 Dataset Card

- **Dataset:** Multilingual Health Question Answering in Low-Resource African Languages Challenge (Zindi / ITU / HASH) (config `multilingual_health_qa`; see [DATASET_CARD.md](DATASET_CARD.md))
- **Distribution:** 4,000 train / 500 validation / 750 test
- **Task:** Conversational Assistant
- **Labels / fields:** Health question and reference answer, with language and country
- **Languages:** Swahili (Kenya), Amharic (Ethiopia), Luganda (Uganda), Akan/Twi (Ghana), English (Kenya)
- **Reference:** https://zindi.world/competitions/multilingual-health-question-answering-in-low-resource-african-languages-challenge

## 📏 Evaluation

**ROUGE-1 F1:** Measures unigram overlap between the generated and reference answers, per language.

**ROUGE-L F1:** Measures similarity based on the longest common subsequence between the generated and reference answers.

**LLM-as-a-Judge:** Measures factual accuracy, completeness and language appropriateness of a sample of answers, rated 1–5 by Gemini and normalised to 0–1.

## 🧠 What You'll Learn

- Generate answers with a language model in several African languages
- Add example questions and answers to the prompt (few-shot prompting)
- Evaluate generated text per language with ROUGE and an independent LLM judge
- Tell apart a change in answer style from a change in answer content

## 🚀 Getting Started

1. Open `notebook.ipynb`.
2. Open the notebook in Google Colab.
3. Enable a GPU if required.
4. Follow the setup instructions in the notebook.
5. Run the notebook from beginning to end.

To see every output before you run it, open the [completed run](../../completed/multilingual-health-qa.ipynb) of this notebook.

## 🔭 Go Further

- Fine-tune Gemma on the training answers with QLoRA and compare with few-shot prompting
- Retrieve the most similar answered questions for each new question and put them in the prompt (retrieval-augmented generation)
- Add another African language or health topic, with answers reviewed by health workers
- Test the assistant with community health workers and collect their preferences between answers

## ❓ Frequently Asked Questions

<details>
<summary>When should I use Gemma 3 4B instead of 1B?</summary>

Gemma 3 4B has 4 billion parameters, a 128K-token context window and was trained on 4 trillion tokens. It usually gives better answers, especially in African languages and as an LLM judge, but it is slower and uses more GPU memory. Change `MODEL_ID` in the notebook's Settings cell to switch.

[google/gemma-3-4b-it](https://huggingface.co/google/gemma-3-4b-it)

</details>

<details>
<summary>What is few-shot prompting?</summary>

Few-shot prompting adds a handful of labelled examples to the prompt before the real input. The model is not trained; it simply sees what the task looks like. It often helps small models follow a label format and understand a definition.

[Prompt Engineering Guide: few-shot prompting](https://www.promptingguide.ai/techniques/fewshot)

</details>

<details>
<summary>What is LLM-as-a-Judge, and why Gemini?</summary>

A second, stronger language model rates generated answers against clear criteria (here on a 1–5 scale, normalised to 0–1). It captures qualities such as correctness and helpfulness that word-overlap metrics miss. A model should not judge its own outputs, so an independent model (Gemini) is used, on a small sample, as a demonstration; check some ratings yourself.

[Judging LLM-as-a-Judge](https://arxiv.org/abs/2306.05685) · [Gemini API](https://ai.google.dev/gemini-api/docs)

</details>

<details>
<summary>How can I deploy this project?</summary>

Wrap the final model or pipeline in a small Gradio app and host it on Hugging Face Spaces, or serve it behind an API on your institution's server. Keep a human in the loop for any decision that affects people.

[Gradio quickstart](https://www.gradio.app/guides/quickstart) · [Hugging Face Spaces](https://huggingface.co/docs/hub/spaces-overview)

</details>

## 📚 References

1. ITU and HASH (2026). Multilingual Health Question Answering in Low-Resource African Languages Challenge. Zindi. https://zindi.world/competitions/multilingual-health-question-answering-in-low-resource-african-languages-challenge
2. Gemma Team, Google DeepMind (2025). Gemma 3 Technical Report. https://arxiv.org/abs/2503.19786
3. Brown et al. (2020). Language Models are Few-Shot Learners. https://arxiv.org/abs/2005.14165
