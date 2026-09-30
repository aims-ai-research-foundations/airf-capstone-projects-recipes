# Build a Customer Support Intent Classifier

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/customer-support-assistant/notebook.ipynb)
[![View on GitHub](https://img.shields.io/badge/View%20on-GitHub-black?logo=github)](https://github.com/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/customer-support-assistant/notebook.ipynb)
[![Dataset](https://img.shields.io/badge/Dataset-Hugging%20Face-yellow?logo=huggingface)](https://huggingface.co/datasets/Similoluwa/capstone-datasets/viewer/customer_support)

**Project Category:** Classification

**Model:** Gemma 1B

**Fine-tuning:** QLoRA

**Recommended GPU:** NVIDIA T4, 16 GB VRAM (free Google Colab)

## 🎯 The Problem

Support teams receive thousands of customer queries a day, and each one must be routed to the right team quickly and correctly.

## 💡 The Solution

A small language model that predicts the intent of a customer support message, compared across zero-shot prompting, few-shot prompting and QLoRA fine-tuning.

## 👥 Who is this for?

- Customer-support and contact-centre teams
- Banks, fintechs, mobile-money and telecom providers
- Students learning prompting and fine-tuning

## 🔬 Research Question

> How much does a small language model gain from examples in its prompt, and from fine-tuning, when classifying customer intents?

## 📊 Dataset Card

- **Dataset:** BANKING77 (config `customer_support`; see [DATASET_CARD.md](DATASET_CARD.md))
- **Distribution:** 8,951 train / 995 validation / 3,075 test
- **Task:** Classification
- **Labels / fields:** Customer query (`text`) and one of 77 banking intents (`label`)
- **Languages:** English
- **Reference:** https://huggingface.co/datasets/legacy-datasets/banking77

## 📏 Evaluation

**Accuracy:** Measures the proportion of test queries assigned the correct intent.

**Macro-F1:** Measures performance across all 77 intents with equal weight per intent, so rare intents count as much as common ones.

**Confusion analysis:** Measures which similar intents (e.g. `card_arrival` vs `card_delivery_estimate`) each approach confuses most often.

## 🧠 What You'll Learn

- Prompt a language model for zero-shot classification over a fixed label set
- Add labelled examples to the prompt (few-shot prompting)
- Fine-tune Gemma with QLoRA on a single T4 GPU
- Compare approaches overall and per intent

## 🚀 Getting Started

1. Open `notebook.ipynb`.
2. Open the notebook in Google Colab.
3. Enable a GPU if required.
4. Follow the setup instructions in the notebook.
5. Run the notebook from beginning to end.

To see every output before you run it, open the [completed run](../../completed/customer-support-assistant.ipynb) of this notebook.

## 🔭 Go Further

- Build a Swahili banking intent classifier with the INJONGO intent data, using the same zero-shot, few-shot and QLoRA comparison
- Choose few-shot examples by similarity to the query with EmbeddingGemma instead of at random
- Let the model answer "unsure" and pass those queries to a human agent; measure how many errors this catches
- Connect the classifier to a reply generator to build a full support pipeline (see [Build a Customer Support Response Generator](../customer-support-response-generation/))

## ❓ Frequently Asked Questions

<details>
<summary>What is the Gemma 3 1B model?</summary>

Gemma 3 1B is a small open-weight language model from Google with 1 billion parameters and a 32K-token context window, trained on 2 trillion tokens covering more than 140 languages. It is small enough to run and fine-tune on a free Colab T4 GPU, which makes it a good starting point for experiments.

[Gemma 3 model card](https://ai.google.dev/gemma/docs/core/model_card_3) · [google/gemma-3-1b-it](https://huggingface.co/google/gemma-3-1b-it)

</details>

<details>
<summary>What is zero-shot prompting?</summary>

Zero-shot prompting asks a model to do a task from instructions alone, without any labelled training examples. Here we describe the task and the allowed answers in the prompt, then map the model's reply to a label.

[Prompt Engineering Guide: zero-shot prompting](https://www.promptingguide.ai/techniques/zeroshot)

</details>

<details>
<summary>What is few-shot prompting?</summary>

Few-shot prompting adds a handful of labelled examples to the prompt before the real input. The model is not trained; it simply sees what the task looks like. It often helps small models follow a label format and understand a definition.

[Prompt Engineering Guide: few-shot prompting](https://www.promptingguide.ai/techniques/fewshot)

</details>

<details>
<summary>What is QLoRA and why do we use it?</summary>

QLoRA loads the base model in 4-bit precision and trains only small low-rank adapter (LoRA) matrices added to it. Memory use drops enough to fine-tune a language model on a single free T4 GPU, while keeping quality close to full fine-tuning.

[QLoRA paper](https://arxiv.org/abs/2305.14314) · [Hugging Face PEFT](https://huggingface.co/docs/peft/index) · [TRL SFTTrainer](https://huggingface.co/docs/trl/sft_trainer)

</details>

<details>
<summary>How can I deploy this project?</summary>

Wrap the final model or pipeline in a small Gradio app and host it on Hugging Face Spaces, or serve it behind an API on your institution's server. Keep a human in the loop for any decision that affects people.

[Gradio quickstart](https://www.gradio.app/guides/quickstart) · [Hugging Face Spaces](https://huggingface.co/docs/hub/spaces-overview)

</details>

## 📚 References

1. BANKING77: Casanueva et al. (2020). Efficient Intent Detection with Dual Sentence Encoders. https://arxiv.org/abs/2003.04807
2. Gemma Team, Google DeepMind (2025). Gemma 3 Technical Report. https://arxiv.org/abs/2503.19786
3. Dettmers et al. (2023). QLoRA: Efficient Finetuning of Quantized LLMs. https://arxiv.org/abs/2305.14314
4. Brown et al. (2020). Language Models are Few-Shot Learners. https://arxiv.org/abs/2005.14165
