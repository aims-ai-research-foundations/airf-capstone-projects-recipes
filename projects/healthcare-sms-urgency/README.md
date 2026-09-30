# Classify Healthcare SMS by Urgency

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/healthcare-sms-urgency/notebook.ipynb)
[![View on GitHub](https://img.shields.io/badge/View%20on-GitHub-black?logo=github)](https://github.com/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/healthcare-sms-urgency/notebook.ipynb)
[![Dataset](https://img.shields.io/badge/Dataset-Hugging%20Face-yellow?logo=huggingface)](https://huggingface.co/datasets/Similoluwa/capstone-datasets/viewer/healthcare_sms_urgency)

**Project Category:** Classification

**Model:** Gemma 1B

**Fine-tuning:** QLoRA

**Recommended GPU:** NVIDIA T4, 16 GB VRAM (free Google Colab)

## 🎯 The Problem

Health hotlines and SMS services receive many messages, and urgent ones can wait too long before a person reads them.

## 💡 The Solution

A language model that classifies health messages by urgency so human responders review the most urgent first, compared with triage nurses.

## 👥 Who is this for?

- Health hotline and mHealth teams, such as maternal-health SMS services
- Triage nurses and emergency responders
- Researchers in clinical NLP

## 🔬 Research Question

> When the costliest error is missing an emergency, does fine-tuning a small language model bring it closer to triage nurses than zero-shot prompting?

## 📊 Dataset Card

- **Dataset:** KTAS emergency-department triage records rewritten as SMS-style messages (config `healthcare_sms_urgency`; see [DATASET_CARD.md](DATASET_CARD.md))
- **Distribution:** 879 train / 188 validation / 189 test
- **Task:** Classification
- **Labels / fields:** Message and urgency label (`emergency`, `urgent`, `routine`) from expert triage; the nurse's triage level as a human reference
- **Languages:** English
- **Reference:** https://doi.org/10.1371/journal.pone.0216972

## 📏 Evaluation

**Macro-F1:** Measures overall performance across the three classes with equal weight.

**Emergency recall:** Measures the share of true emergencies the model flags; the headline metric, because a missed emergency is the costliest error.

**Confusion Matrix:** Measures under-triage (emergencies marked routine) against over-triage.

## 🧠 What You'll Learn

- Perform zero-shot urgency classification with a language model
- Fine-tune Gemma with QLoRA on labelled triage messages
- Evaluate a safety-critical classifier by the recall of its most important class
- Compare a model with a human (triage nurse) reference

## 🚀 Getting Started

1. Open `notebook.ipynb`.
2. Open the notebook in Google Colab.
3. Enable a GPU if required.
4. Follow the setup instructions in the notebook.
5. Run the notebook from beginning to end.

To see every output before you run it, open the [completed run](../../completed/healthcare-sms-urgency.ipynb) of this notebook.

## 🔭 Go Further

- Make the model more cautious, for example by weighting emergency examples in training or asking it to choose emergency when unsure
- Compare with Gemma 4B, zero-shot and few-shot
- Translate messages into a local language with native-speaker review and re-evaluate
- Build a triage queue that sorts incoming messages for a nurse, most urgent first, and measure how long emergencies wait

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
<summary>What is QLoRA and why do we use it?</summary>

QLoRA loads the base model in 4-bit precision and trains only small low-rank adapter (LoRA) matrices added to it. Memory use drops enough to fine-tune a language model on a single free T4 GPU, while keeping quality close to full fine-tuning.

[QLoRA paper](https://arxiv.org/abs/2305.14314) · [Hugging Face PEFT](https://huggingface.co/docs/peft/index) · [TRL SFTTrainer](https://huggingface.co/docs/trl/sft_trainer)

</details>

<details>
<summary>How can I deploy this project?</summary>

Only as a decision-support tool: for example, a Gradio app or API that orders incoming messages for a human responder, who reads every message. Before any real use, it needs evaluation on local messages, clinical review and approval from the responsible health authority.

[Gradio quickstart](https://www.gradio.app/guides/quickstart) · [Hugging Face Spaces](https://huggingface.co/docs/hub/spaces-overview)

</details>

## 📚 References

1. Moon et al. (2019). Triage accuracy and causes of mistriage using the Korean Triage and Acuity Scale. PLOS ONE. https://doi.org/10.1371/journal.pone.0216972
2. Gemma Team, Google DeepMind (2025). Gemma 3 Technical Report. https://arxiv.org/abs/2503.19786
3. IDinsight. Enhancing Maternal Healthcare: Training Language Models to Identify Urgent Messages in Real-Time. https://idinsight.github.io/tech-blog/blog/enhancing_maternal_healthcare/
