# Build a News Headline Generator

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/news-headline-generation/notebook.ipynb)
[![View on GitHub](https://img.shields.io/badge/View%20on-GitHub-black?logo=github)](https://github.com/aims-ai-research-foundations/airf-capstone-project-recipes/blob/main/projects/news-headline-generation/notebook.ipynb)
[![Dataset](https://img.shields.io/badge/Dataset-Hugging%20Face-yellow?logo=huggingface)](https://huggingface.co/datasets/Similoluwa/capstone-datasets/viewer/news_headlines)

**Project Category:** Summarization

**Model:** Gemma 4B

**Fine-tuning:** QLoRA

**Recommended GPU:** NVIDIA T4, 16 GB VRAM (free Google Colab)

## 🎯 The Problem

Newsrooms and community radio stations across Africa publish in local languages and must write clear, accurate headlines for every story, often under time pressure.

## 💡 The Solution

A language model that generates concise and informative headlines from news articles in African languages (Swahili by default), compared before and after QLoRA fine-tuning on published headlines.

## 👥 Who is this for?

- Journalists and editors in African-language newsrooms and community radio
- Researchers working on natural language generation for African languages
- Students learning summarization and its evaluation

## 🔬 Research Question

> Does fine-tuning improve a small language model's ability to generate concise and informative news headlines?

## 📊 Dataset Card

- **Dataset:** [AfriHG](https://arxiv.org/abs/2412.20223): news articles and their published headlines in African languages (Ogunremi et al., 2024) (config `news_headlines`; see [DATASET_CARD.md](DATASET_CARD.md))
- **Distribution:** 25,000 train / 5,444 validation / 5,431 test
- **Task:** Summarization
- **Labels / fields:** Article text, published headline (reference) and language
- **Languages:** Swahili (default), Yoruba, isiZulu, isiXhosa, Tigrinya
- **Reference:** https://github.com/dadelani/AfriHG

## 📏 Evaluation

**ROUGE-1 F1:** Measures word overlap between the generated and the published headline.

**ROUGE-L F1:** Measures similarity based on the longest sequence of words the generated and published headlines share in order.

**LLM-as-a-Judge:** Measures accuracy, informativeness and concision of a sample of headlines, judged against the article by Gemini (an independent model), rated 1–5 and normalised to 0–1.

## 🧠 What You'll Learn

- Summarize news articles into headlines with a language model
- Fine-tune Gemma 4B with QLoRA on published headlines
- Evaluate generated text with ROUGE and an independent LLM judge
- Inspect which information a headline keeps or drops, and when it becomes vague, long or misleading
- Tell apart a headline that matches the reference from a headline that is good
- Compare your results with published baselines from the [AfriHG paper](https://arxiv.org/abs/2412.20223) (mT5, AfriTeVa V2 and Aya-101)

## 🚀 Getting Started

1. Open `notebook.ipynb`.
2. Open the notebook in Google Colab.
3. Enable a GPU if required.
4. Follow the setup instructions in the notebook.
5. Run the notebook from beginning to end.

To see every output before you run it, open the [completed run](../../completed/news-headline-generation.ipynb) of this notebook.

## 🔭 Go Further

- Repeat the experiment in Yoruba, isiZulu, isiXhosa or Tigrinya (all in this dataset) and compare how much fine-tuning helps in each language
- Ask a few readers to choose between headlines in a blind test, and compare their choices with ROUGE and the Gemini judge
- Add constraints to the prompt, such as a word limit or naming the main person or place, and measure how often each model follows them
- Check factual consistency: ask the judge whether each headline makes a claim the article does not support, and count how often it happens

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
<summary>What do ROUGE-1 and ROUGE-L measure, and what do they miss?</summary>

ROUGE compares a generated text with a reference: ROUGE-1 counts shared words and ROUGE-L the longest sequence of words they share in order, both as an F1 score from 0 to 1. A headline can be accurate and informative but use different words from the reference and score low, or copy the reference's words and still be vague or misleading, so read examples and use a judge as well.

[Lin (2004)](https://aclanthology.org/W04-1013/) · [Hugging Face Evaluate: ROUGE](https://huggingface.co/spaces/evaluate-metric/rouge)

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

1. Ogunremi, T., Akojenu, S., Soronnadi, A., Adekanmbi, O. and Adelani, D. I. (2024). AfriHG: News headline generation for African Languages. AfricaNLP Workshop at ICLR 2024. https://arxiv.org/abs/2412.20223
2. Hasan, T. et al. (2021). XL-Sum: Large-Scale Multilingual Abstractive Summarization for 44 Languages. Findings of ACL 2021. https://aclanthology.org/2021.findings-acl.413/
3. Adelani, D. I. et al. (2023). MasakhaNEWS: News Topic Classification for African languages. IJCNLP-AACL 2023. https://aclanthology.org/2023.ijcnlp-main.10/
4. Lin, C.-Y. (2004). ROUGE: A Package for Automatic Evaluation of Summaries. https://aclanthology.org/W04-1013/
5. Dettmers et al. (2023). QLoRA: Efficient Finetuning of Quantized LLMs. https://arxiv.org/abs/2305.14314
6. Zheng et al. (2023). Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena. https://arxiv.org/abs/2306.05685
