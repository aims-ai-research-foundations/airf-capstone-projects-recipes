# Build a Customer Support Response Generator

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/customer-support-response-generation/notebook.ipynb)
[![View on GitHub](https://img.shields.io/badge/View%20on-GitHub-black?logo=github)](https://github.com/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/customer-support-response-generation/notebook.ipynb)
[![Dataset](https://img.shields.io/badge/Dataset-Hugging%20Face-yellow?logo=huggingface)](https://huggingface.co/datasets/Similoluwa/capstone-datasets/viewer/customer_support_responses)

**Project Category:** Conversational Assistant

**Model:** Gemma 4B

**Fine-tuning:** QLoRA

**Recommended GPU:** NVIDIA T4, 16 GB VRAM (free Google Colab)

## 🎯 The Problem

Writing consistent, helpful replies to routine customer queries takes up much of a support agent's day.

## 💡 The Solution

A small language model that generates helpful responses to customer support messages, compared before and after QLoRA fine-tuning.

## 👥 Who is this for?

- Customer-support agents who review and send drafted replies
- Small businesses and e-commerce teams
- Students learning parameter-efficient fine-tuning

## 🔬 Research Question

> Does fine-tuning make a language model's replies more useful, or merely more similar to the training data?

## 📊 Dataset Card

- **Dataset:** Bitext Customer Support LLM Chatbot Training Dataset (config `customer_support_responses`; see [DATASET_CARD.md](DATASET_CARD.md))
- **Distribution:** 3,240 train / 405 validation / 405 test
- **Task:** Conversational Assistant
- **Labels / fields:** Customer query (`instruction`), reference reply (`response`), intent (27) and category (11)
- **Languages:** English
- **Reference:** https://huggingface.co/datasets/bitext/Bitext-customer-support-llm-chatbot-training-dataset

## 📏 Evaluation

**ROUGE-L F1:** Measures overlap between the generated and reference reply based on their longest common subsequence.

**LLM-as-a-Judge:** Measures correctness, helpfulness and tone of a sample of replies, rated 1–5 by Gemini (an independent model) and normalised to 0–1.

**Human preference:** Measures which of the two replies people prefer in a blind A/B comparison.

## 🧠 What You'll Learn

- Format support conversations as prompt-completion chat examples
- Fine-tune Gemma 4B with QLoRA (4-bit base model with LoRA adapters)
- Evaluate generated text with ROUGE, an independent LLM judge and human preference
- Tell apart 'more useful' from 'more similar to the training data'

## 🚀 Getting Started

1. Open `notebook.ipynb`.
2. Open the notebook in Google Colab.
3. Enable a GPU if required.
4. Follow the setup instructions in the notebook.
5. Run the notebook from beginning to end.

To see every output before you run it, open the [completed run](../../completed/customer-support-response-generation.ipynb) of this notebook.

## 🔭 Go Further

- Adapt the assistant to a local domain such as mobile money or telecoms, with a few hundred example replies you write or collect
- Retrieve the relevant help article for each query and add it to the prompt, then compare with fine-tuning
- Route each query through the intent classifier first (see [Build a Customer Support Intent Classifier](../customer-support-assistant/)) and fine-tune one reply style per intent group
- Study how the number of training replies (100, 500, 2,000) affects reply quality

## ❓ Frequently Asked Questions

<details>
<summary>When should I use Gemma 3 4B instead of 1B?</summary>

Gemma 3 4B has 4 billion parameters, a 128K-token context window and was trained on 4 trillion tokens. It usually gives better answers, especially in African languages and as an LLM judge, but it is slower and uses more GPU memory. Change `MODEL_ID` in the notebook's Settings cell to switch.

[google/gemma-3-4b-it](https://huggingface.co/google/gemma-3-4b-it)

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

1. Bitext (2023). Customer Support LLM Chatbot Training Dataset. https://huggingface.co/datasets/bitext/Bitext-customer-support-llm-chatbot-training-dataset
2. Thakur, P. (2026). Fine-Tuning Gemma 4 with QLoRA for Customer Support. PyImageSearch. https://pyimagesearch.com/2026/09/21/fine-tuning-gemma-4-with-qlora-for-customer-support/
3. Dettmers et al. (2023). QLoRA: Efficient Finetuning of Quantized LLMs. https://arxiv.org/abs/2305.14314
4. Zheng et al. (2023). Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena. https://arxiv.org/abs/2306.05685
