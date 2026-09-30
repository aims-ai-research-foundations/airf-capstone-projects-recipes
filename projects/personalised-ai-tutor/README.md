# Build Your Own Personalised AI Tutor

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/personalised-ai-tutor/notebook.ipynb)
[![View on GitHub](https://img.shields.io/badge/View%20on-GitHub-black?logo=github)](https://github.com/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/personalised-ai-tutor/notebook.ipynb)
[![Dataset](https://img.shields.io/badge/Dataset-Hugging%20Face-yellow?logo=huggingface)](https://huggingface.co/datasets/Similoluwa/capstone-datasets/viewer/tutor_preferences)

**Project Category:** Conversational Assistant

**Model:** Gemma 1B, with a small ModernBERT reward model

**Fine-tuning:** Reward model, then reward-guided LoRA fine-tuning

**Recommended GPU:** NVIDIA T4, 16 GB VRAM (free Google Colab)

## 🎯 The Problem

A language model can answer a learner's question correctly and still teach it the wrong way for that learner: too technical for a beginner, too slow for an advanced student, or without the examples a visual learner needs.

## 💡 The Solution

A personalised tutor that learns how one learner wants to be taught: you define what "better" means, collect preferences over tutor responses, train a reward model on them, and use the reward to align the tutor.

## 👥 Who is this for?

- Educators designing AI teaching assistants
- Students learning how language models are aligned
- Developers building personalised learning tools

## 🔬 Research Question

> Can a small reward model learn one learner's preferences from a few hundred comparisons, and does aligning a tutor to that reward make it teach the way the learner wants?

## 📊 Dataset Card

- **Dataset:** Tutor Preference Dataset: teaching questions, learner profiles and pairs of tutor responses, built for this library (config `tutor_preferences`; see [DATASET_CARD.md](DATASET_CARD.md))
- **Distribution:** 392 train / 56 validation / 112 test
- **Task:** Conversational Assistant
- **Labels / fields:** Question, learner profile, response A, response B, preferred response and reason; plus a set of questions for aligning and testing the tutor
- **Languages:** English
- **Reference:** https://huggingface.co/datasets/Similoluwa/capstone-datasets/viewer/tutor_preferences

## 📏 Evaluation

**Reward-model agreement:** Measures how often the reward model prefers the same response as the learner on new, unseen pairs.

**Preference win rate:** Measures how often the reward model prefers the personalised tutor's answer over the base tutor's on test questions.

**LLM-as-a-Judge:** Measures how well each tutor follows each of the learner's preferences, rated 1–5 by Gemini (an independent model) on a sample and normalised to 0–1.

**Human evaluation:** Measures helpfulness, clarity, personalisation and pedagogical quality (1–5), and which tutor people would rather learn from.

## 🧠 What You'll Learn

- Decide what makes one tutor response better than another for a given learner
- Represent preferences as (prompt, preferred response, rejected response)
- Train a small reward model with the Bradley–Terry objective and test it on new pairs
- Align a tutor with reward-guided fine-tuning: generate, score, select, fine-tune
- Evaluate whether the tutor improved, and why a higher reward does not guarantee a better tutor

## 🚀 Getting Started

1. Open `notebook.ipynb`.
2. Open the notebook in Google Colab.
3. Enable a GPU if required.
4. Follow the setup instructions in the notebook.
5. Run the notebook from beginning to end.

To see every output before you run it, open the [completed run](../../completed/personalised-ai-tutor.ipynb) of this notebook.

## 🔭 Go Further

- Collect real preferences from your own learners and compare them with the profile-based labels
- Align the tutor directly on the preference pairs with DPO, and compare with reward-guided fine-tuning
- Repeat generate, score, select and fine-tune for a second round and measure whether the tutor keeps improving
- Build a tutor for a subject you teach, in a local language, with your own learner profile

## ❓ Frequently Asked Questions

<details>
<summary>What is the Gemma 3 1B model?</summary>

Gemma 3 1B is a small open-weight language model from Google with 1 billion parameters and a 32K-token context window, trained on 2 trillion tokens covering more than 140 languages. It is small enough to run and fine-tune on a free Colab T4 GPU, which makes it a good starting point for experiments.

[Gemma 3 model card](https://ai.google.dev/gemma/docs/core/model_card_3) · [google/gemma-3-1b-it](https://huggingface.co/google/gemma-3-1b-it)

</details>

<details>
<summary>Is this RLHF?</summary>

It follows the same idea as reinforcement learning from human feedback (RLHF): preference data, a reward model and optimisation of the model against the reward. Full RLHF optimises with reinforcement learning (for example PPO), which is complex and expensive. This recipe uses reward-guided fine-tuning instead: the tutor writes several answers, the reward model picks the best, and the tutor is fine-tuned on those. We call the whole process preference alignment.

[Ouyang et al. (2022)](https://arxiv.org/abs/2203.02155) · [RAFT (Dong et al., 2023)](https://arxiv.org/abs/2304.06767)

</details>

<details>
<summary>What is a reward model?</summary>

A reward model is a model with a single-number output. It is trained on pairs of responses to the same prompt so that the response people preferred gets the higher score. Once trained, it can score any new response, for example to pick the best of several candidates. Here it is a small encoder (ModernBERT), which trains in minutes on a T4.

[TRL Reward Modeling](https://huggingface.co/docs/trl/reward_trainer) · [InstructGPT paper](https://arxiv.org/abs/2203.02155)

</details>

<details>
<summary>What is Direct Preference Optimization (DPO)?</summary>

DPO fine-tunes a language model directly on preference pairs: it raises the probability of the chosen response and lowers that of the rejected one, relative to a frozen copy of the original model. It gives results similar to reinforcement learning from human feedback without training a separate policy with RL, which makes it practical on a single GPU.

[DPO paper](https://arxiv.org/abs/2305.18290) · [TRL DPO Trainer](https://huggingface.co/docs/trl/dpo_trainer)

</details>

<details>
<summary>What is LLM-as-a-Judge, and why Gemini?</summary>

A second, stronger language model rates generated answers against clear criteria (here on a 1–5 scale, normalised to 0–1). It captures qualities such as correctness and helpfulness that word-overlap metrics miss. A model should not judge its own outputs, so an independent model (Gemini) is used, on a small sample, as a demonstration; check some ratings yourself.

[Judging LLM-as-a-Judge](https://arxiv.org/abs/2306.05685) · [Gemini API](https://ai.google.dev/gemini-api/docs)

</details>

## 📚 References

1. Bradley, R. A. and Terry, M. E. (1952). Rank Analysis of Incomplete Block Designs: I. The Method of Paired Comparisons. Biometrika. https://doi.org/10.2307/2334029
2. Ouyang et al. (2022). Training language models to follow instructions with human feedback. https://arxiv.org/abs/2203.02155
3. Dong et al. (2023). RAFT: Reward rAnked FineTuning for Generative Foundation Model Alignment. https://arxiv.org/abs/2304.06767
4. Rafailov et al. (2023). Direct Preference Optimization: Your Language Model is Secretly a Reward Model. https://arxiv.org/abs/2305.18290
