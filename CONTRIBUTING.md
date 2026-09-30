# Contributing

Thank you for helping improve the AIRF Capstone Project Recipes. We welcome fixes, new recipes, local adaptations and better evaluations.

## Ways to contribute

- **Report a problem:** open an issue describing the recipe, the notebook cell and what went wrong.
- **Fix something:** open a pull request with a short description of the change.
- **Add a recipe:** follow the steps below.
- **Share an adaptation:** tell us how you adapted a recipe to your language, country or institution.

## Adding a recipe

1. Read [docs/recipe-guidelines.md](docs/recipe-guidelines.md), [docs/dataset-guidelines.md](docs/dataset-guidelines.md) and [docs/evaluation-guidelines.md](docs/evaluation-guidelines.md).
2. Create `projects/<recipe-name>/` with `README.md`, `DATASET_CARD.md` and `notebook.ipynb`.
3. Use a publicly available, annotated dataset with a licence that allows redistribution, and add it as a configuration of the [capstone-datasets](https://huggingface.co/datasets/Similoluwa/capstone-datasets) repository.
4. Run the notebook end to end on a free Google Colab T4 GPU before opening a pull request.
5. Add the recipe to the catalogue in the main [README.md](README.md).

## Pull request checklist

- [ ] The notebook runs top to bottom on a Colab T4 GPU.
- [ ] The README follows the recipe template.
- [ ] The dataset card states the source, licence and any adaptation.
- [ ] Results in the notebook come from the test split only, which is not used for training or prompt tuning.
- [ ] Code follows PEP 8, with type annotations and one-sentence docstrings.

## Licence

By contributing, you agree that your contribution is released under [CC0 1.0](LICENSE), like the rest of this repository. Datasets keep their own licences.

## Conduct

Everyone taking part is expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md). Questions: [ai-research-foundations@aims.ac.za](mailto:ai-research-foundations@aims.ac.za).
