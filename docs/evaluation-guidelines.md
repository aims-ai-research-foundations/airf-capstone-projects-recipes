# Evaluation Guidelines

Evaluation should answer the recipe's research question with **established metrics** on the **test split**.

## Rules

- Tune prompts and hyperparameters on `validation`. Report results on `test` once.
- Compare every approach against a meaningful reference: the same model without the technique being tested (zero-shot vs few-shot, base vs fine-tuned), or a human baseline where one exists.
- Evaluate every approach on the same examples. When generation is slow, use the `in_small_test` subset for all approaches.
- Present one results table that answers the research question.
- Count outputs that cannot be parsed into a valid label as errors, and report how often they occur.

## Metrics by task

| What is evaluated | Primary metrics | Notes |
|---|---|---|
| Classification | Macro-F1, accuracy, confusion matrix | Macro-F1 when classes are imbalanced; recall of the critical class for safety tasks |
| Extractive / short-answer QA | Exact Match, token-level F1 | Normalise case, punctuation and articles |
| Generative QA and responses | ROUGE-1/ROUGE-L F1, BERTScore, LLM-as-a-Judge | Exact Match is usually near zero for long answers |
| Synthetic data | Downstream macro-F1 on real test data, human evaluation, Cohen's kappa | Also report duplicate and filter rates |
| Summaries and headlines | ROUGE-1/ROUGE-L F1, LLM-as-a-Judge | One reference undervalues valid alternatives; judge against the source text |
| Preference alignment | Preference win rate, reward analysis, LLM-as-a-Judge | Check that a higher reward means a better answer, not just a longer one |

## LLM-as-a-Judge

Use a judge only for generated text that lexical metrics cannot assess well.

- **Judge model:** a model should not judge its own outputs. Use a separate, stronger model: the notebooks use Gemini through a free API key. Rate a small sample (about 10 answers) as a demonstration, and add human evaluation on a small held-out subset where practical.
- **Criteria:** name them explicitly, e.g. factual accuracy against the reference, completeness, language appropriateness, usefulness to the intended reader.
- **Scale:** ask for a single integer from 1 to 5, parse it, and normalise with `(score - 1) / 4`. Count unparseable replies separately.
- **Check the judge:** rate 20 examples by hand and compare with the judge before trusting it.
- **Report** the judge model and prompt with the results.

## Non-English text

The default `rouge_score` tokenizer drops non-Latin characters, which gives wrong scores for Amharic and for letters such as ɛ and ɔ in Akan. Pass a whitespace tokenizer instead:

```python
rouge.compute(predictions=preds, references=refs, tokenizer=str.split)
```

Report metrics **per language** as well as overall.
