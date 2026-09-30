# Build a Domain-Specific Agricultural Small Language Model

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/domain-specific-agriculture/notebook.ipynb)
[![View on GitHub](https://img.shields.io/badge/View%20on-GitHub-black?logo=github)](https://github.com/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/domain-specific-agriculture/notebook.ipynb)
[![Dataset](https://img.shields.io/badge/Dataset-Hugging%20Face-yellow?logo=huggingface)](https://huggingface.co/datasets/Similoluwa/capstone-datasets/viewer/agriculture_qa)

**Project Category:** Conversational Assistant

**Model:** Gemma 4B

**Fine-tuning:** QLoRA (domain adaptation)

**Recommended GPU:** NVIDIA T4, 16 GB VRAM (free Google Colab)

## 🎯 The Problem

Smallholder farmers need quick, practical answers about crops, pests and soil, but there are too few extension officers to answer every question.

## 💡 The Solution

A small language model adapted to African agriculture by fine-tuning on farmers' questions answered by agricultural experts, compared with the general model on the same held-out questions.

## 👥 Who is this for?

- Agricultural extension services and farmer advisory platforms
- Agricultural researchers and universities
- Students learning domain adaptation of language models

## 🔬 Research Question

> Does domain adaptation improve a small language model's answers to farmers' questions, or does it mainly teach the style of the answers? What does the model lose in exchange?

## 📊 Dataset Card

- **Dataset:** AgroQA: questions from Ugandan smallholder farmers answered by agricultural experts (Omara et al., 2023) (config `agriculture_qa`; see [DATASET_CARD.md](DATASET_CARD.md))
- **Distribution:** 2,503 train / 137 validation / 234 test
- **Task:** Conversational Assistant
- **Labels / fields:** Farmer question, expert reference answer, crop (`cassava`, `maize`, `beans`, `general`) and paraphrase group
- **Languages:** English (Uganda)
- **Reference:** https://github.com/JonaOmara/AgroQA-Dataset

## 📏 Evaluation

**Answer accuracy (token F1):** Measures word overlap between the generated answer and the expert's answer, per crop.

**LLM-as-a-Judge:** Measures correctness and usefulness to a smallholder farmer of a sample of answers, rated 1–5 by Gemini (an independent model) and normalised to 0–1.

**Human preference:** Measures which of the two answers people prefer in a blind A/B comparison.

## 🧠 What You'll Learn

- Adapt a general language model to a domain with QLoRA fine-tuning
- Compare a general and a domain-adapted model on the same held-out questions
- Evaluate answers with token F1, an independent LLM judge and human preference
- Tell apart better answers from answers that only copy the training style
- Check what a model loses when it specialises

## 🚀 Getting Started

1. Open `notebook.ipynb`.
2. Open the notebook in Google Colab.
3. Enable a GPU if required.
4. Follow the setup instructions in the notebook.
5. Run the notebook from beginning to end.

To see every output before you run it, open the [completed run](../../completed/domain-specific-agriculture.ipynb) of this notebook.

## 🔭 Go Further

- Retrieve relevant passages from extension guides and add them to the prompt (retrieval-augmented generation), then compare with fine-tuning
- Build a Swahili farmer assistant with the Tanzanian [Swahili Question-Answering Dataset for Horticulture](https://doi.org/10.7910/DVN/SORRLR) (CC0)
- Vary the amount of training data (250, 1,000, all questions) and plot answer accuracy
- Prototype an SMS advisory service in which an extension officer reviews every answer before it is sent

## ❓ Frequently Asked Questions

<details>
<summary>When should I use Gemma 3 4B instead of 1B?</summary>

Gemma 3 4B has 4 billion parameters, a 128K-token context window and was trained on 4 trillion tokens. It usually gives better answers, especially in African languages and as an LLM judge, but it is slower and uses more GPU memory. Change `MODEL_ID` in the notebook's Settings cell to switch.

[google/gemma-3-4b-it](https://huggingface.co/google/gemma-3-4b-it)

</details>

<details>
<summary>What is domain adaptation?</summary>

Domain adaptation means further training a general model on text from one field, here farmers' questions and experts' answers, so that it uses the field's knowledge, terms and answer style. It can improve answers in that field, but the model may also copy the style of the training answers or become worse at other tasks, so compare it with the general model on the same held-out questions.

[Gururangan et al. (2020)](https://arxiv.org/abs/2004.10964)

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

<details>
<summary>How can I deploy this project?</summary>

Wrap the final model or pipeline in a small Gradio app and host it on Hugging Face Spaces, or serve it behind an API on your institution's server. Keep a human in the loop for any decision that affects people.

[Gradio quickstart](https://www.gradio.app/guides/quickstart) · [Hugging Face Spaces](https://huggingface.co/docs/hub/spaces-overview)

</details>

## 📚 References

1. Omara et al. (2023). A field-based recommender system for crop disease detection using machine learning. Frontiers in Artificial Intelligence. https://doi.org/10.3389/frai.2023.1010804
2. Gururangan et al. (2020). Don't Stop Pretraining: Adapt Language Models to Domains and Tasks. https://arxiv.org/abs/2004.10964
3. Dettmers et al. (2023). QLoRA: Efficient Finetuning of Quantized LLMs. https://arxiv.org/abs/2305.14314
4. Zheng et al. (2023). Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena. https://arxiv.org/abs/2306.05685
