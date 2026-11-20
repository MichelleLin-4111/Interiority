# Measuring Interiority: Representations of Inner Life in Fiction at Scale

This repository contains the code, data, evaluation workflows, and analysis materials for the paper **“Measuring Interiority: Representations of Inner Life in Fiction at Scale.”**

The project develops a computational measure of literary interiority, defined as represented access to characters’ thoughts, feelings, beliefs, and perceptions. We annotate 717 passages using a three level ordinal scheme, evaluate supervised language models and prompted large language models, and apply the best performing model to a corpus of 1,348 British novels published between 1704 and 1939.

The repository is organized to support two goals:

1. Reproduce the main analyses and manuscript figures from the released processed data.
2. Preserve the historical research workflow and intermediate artifacts needed to understand how the final data were produced.

## My Contributions (Michelle Lin)

This project was conducted collaboratively at UC Berkeley. My contributions included:

- **Research & Dataset Development:** Contributed to research topic ideation, dataset curation, annotation codebook development, and manual passage annotation.
- **NLP & LLM Evaluation:** Conducted experiments evaluating large language models for three-level interiority classification and analyzed model performance.
- **Code Organization & Reproducibility:** Organized and cleaned the research codebase for publication, including restructuring notebooks, consolidating analysis workflows, archiving legacy code, and improving documentation to support reproducibility.

For the complete research methodology and findings, please refer to the associated paper.

## Quick start

The main paper analyses can be reproduced from the processed data included in this repository. They do not require rerunning BookNLP, model training, or Gemini inference.

```bash
git clone <repository-url>
cd Interiority

python -m venv .venv
source .venv/bin/activate

pip install -r requirements.txt
jupyter lab
```

Python 3.12 is recommended for the main analysis environment.

For additional environment details, including historical model and BookNLP dependencies, see [`docs/environment.md`](docs/environment.md).

## Reproducing the main paper analyses

The primary reproduction path begins with:

[`data/processed/book_summary_analysis.csv`](data/processed/book_summary_analysis.csv)

Run the following notebooks in order:

| Notebook | Purpose |
| --- | --- |
| [`10_book_level_data_and_descriptive_analysis.ipynb`](notebooks/05_analysis/10_book_level_data_and_descriptive_analysis.ipynb) | Book level descriptive analysis, genre summaries, Figure 2, and the appendix corpus count figure |
| [`11_hypothesis_analysis_and_paper_figures.ipynb`](notebooks/05_analysis/11_hypothesis_analysis_and_paper_figures.ipynb) | H1, H2, RQ1, RQ2, robustness analyses, and the corresponding manuscript figures and tables |
| [`12_canonical_author_historical_analysis.ipynb`](notebooks/05_analysis/12_canonical_author_historical_analysis.ipynb) | Historical analysis of novels by canonical authors and the canonical author figures |

Shared analysis functions are located in:

[`src/analysis_utils.py`](src/analysis_utils.py)

Generated figures are written to:

[`results/figures/`](results/figures/)

Generated tables are written to:

[`results/tables/`](results/tables/)

## Repository structure

```text
.
├── data/
│   ├── annotation/
│   │   └── df_annotation_final.csv
│   ├── frozen/
│   │   ├── corpus_inference/
│   │   ├── corpus_passages/
│   │   └── publication_snapshot/
│   └── processed/
│       ├── book_summary_analysis.csv
│       └── book_summary_analysis_schema.csv
├── docs/
│   └── environment.md
├── notebooks/
│   ├── 01_annotation/
│   ├── 02_model_evaluation/
│   ├── 03_corpus_construction/
│   ├── 04_booknlp/
│   └── 05_analysis/
├── workflows/
│   ├── gemini_evaluation/
│   └── gemini_inference/
├── src/
│   └── analysis_utils.py
├── results/
│   ├── figures/
│   └── tables/
├── archive/
│   └── legacy_analysis/
└── requirements.txt
```

## Notebook guide

The numbered notebooks reflect the major stages of the research workflow.

| Stage | Notebook | Role |
| --- | --- | --- |
| Annotation | `01_annotation_sampling.ipynb` | Historical passage sampling workflow for the human annotation dataset |
| Annotation | `02_annotation_agreement_and_classical_baselines.ipynb` | Annotation cleaning, agreement analysis, majority baseline, and TF IDF logistic regression baselines |
| Model evaluation | `03_encoder_model_evaluation.ipynb` | BERT, RoBERTa, DistilBERT, and ModernBERT evaluation |
| Model evaluation | `04_cross_family_llm_evaluation.ipynb` | Historical cross family prompted LLM evaluation |
| Corpus construction | `05_gutenberg_candidate_construction.ipynb` | Initial Project Gutenberg candidate construction |
| Corpus construction | `06_gutenberg_final_corpus_selection.ipynb` | Final corpus selection and filtering |
| Corpus construction | `07_book_metadata_and_publication_year.ipynb` | Publication year and book metadata processing |
| Corpus construction | `08_author_metadata_and_wikidata.ipynb` | Author metadata and Wikidata processing |
| BookNLP | `09a_booknlp_sharded_batch.ipynb` | Historical sharded BookNLP processing workflow |
| BookNLP | `09b_booknlp_additional_42_book_batch.ipynb` | Historical additional BookNLP batch |
| Analysis | `10_book_level_data_and_descriptive_analysis.ipynb` | Descriptive analysis and corpus composition figures |
| Analysis | `11_hypothesis_analysis_and_paper_figures.ipynb` | Main hypothesis, research question, robustness, figure, and table analyses |
| Analysis | `12_canonical_author_historical_analysis.ipynb` | Canonical author historical analysis |

The historical notebooks are retained to document the research process. Some were originally run in Google Colab, Google Drive, or environment specific workflows and should not be treated as the shortest route for reproducing the final paper results.

## Data

### Human annotation data

[`data/annotation/df_annotation_final.csv`](data/annotation/df_annotation_final.csv) contains the final human annotation benchmark.

The benchmark contains 717 passages from 18 books. The book disjoint split contains 357 training passages, 120 development passages, and 240 test passages.

Final label counts are:

| Label | Count |
| --- | ---: |
| HIGH | 327 |
| LOW | 274 |
| NONE | 116 |

### Final book level analysis data

[`data/processed/book_summary_analysis.csv`](data/processed/book_summary_analysis.csv) is the authoritative processed dataset for the current manuscript analyses.

It contains 1,348 books by 435 authors published between 1704 and 1939, together with book level interiority measures and metadata used in the paper.

[`data/processed/book_summary_analysis_schema.csv`](data/processed/book_summary_analysis_schema.csv) documents the fields in this table.

### Frozen research artifacts

The [`data/frozen/`](data/frozen/) directory preserves selected intermediate artifacts that define reproducibility boundaries for historical production stages.

`corpus_passages/` contains the final sampled passage batches used in large scale corpus processing.

`corpus_inference/` contains preserved inference inputs and outputs for the Armstrong related subset.

`publication_snapshot/` contains a historical publication era snapshot of the book level data. It is retained for provenance and comparison. It is not the source of truth for the current manuscript analyses.

## Reproducibility scope

The repository is designed so that the main statistical analyses and manuscript figures can be reproduced from the included processed data without rerunning the full historical production pipeline.

Several upstream stages were created through research workflows that combined BookNLP outputs, manual review, corpus specific postprocessing, and external model APIs. For these stages, the repository preserves the final derived data or frozen intermediate artifacts used by downstream analysis.

In particular, the following historical production stages are documented through final or frozen artifacts rather than reconstructed as a single fresh clone pipeline:

1. BookNLP postprocessing, paragraph reconstruction, passage splitting and merging, narrative position normalization, and 100 bin sampling.
2. Protagonist identification and manual review.
3. Quote mapping and quote share construction.

The authoritative downstream values from these stages are embedded in the released processed data. This separation is intentional. It distinguishes the analyses that can be rerun directly from the research production steps that are preserved primarily for provenance.

## Model evaluation

The human annotated benchmark was used to evaluate classical baselines, encoder models, and prompted language models.

The supervised encoder evaluation includes BERT, RoBERTa, DistilBERT, and ModernBERT. Model selection was performed on the development set, followed by evaluation on the held out test set with book level bootstrap confidence intervals.

The prompted model evaluation compares several model families. Gemini 3.5 Flash with minimal thinking achieved the strongest final macro F1 in the reported comparison and was selected for large scale corpus annotation.

The complete human-readable classification prompt and codebook is available at [`resources/full_interiority_prompt.md`](resources/full_interiority_prompt.md).

Historical model evaluation notebooks are included for transparency, but rerunning all model evaluations is not required to reproduce the paper’s downstream statistical analyses.

## Gemini workflows

Reusable Gemini evaluation and inference code is located in:

[`workflows/gemini_evaluation/`](workflows/gemini_evaluation/)

[`workflows/gemini_inference/`](workflows/gemini_inference/)

These workflows use separate dependency specifications because the Gemini client versions used during evaluation and large scale inference differed.

Running these workflows may require external credentials, API access, and associated model usage costs. They are not required for reproducing the main manuscript figures from the released processed data.

See the workflow documentation and [`docs/environment.md`](docs/environment.md) before attempting to rerun API based experiments.

## Environment

The root [`requirements.txt`](requirements.txt) provides the lightweight environment needed for the main analysis notebooks.

The primary analysis dependencies include NumPy, pandas, Matplotlib, seaborn, IPython, ipykernel, and JupyterLab.

Historical workflows used additional environments for transformer training, BookNLP, and Gemini API access. Exact known requirements and environment notes are documented in [`docs/environment.md`](docs/environment.md).

## Results

Manuscript facing outputs are organized under [`results/`](results/).

`results/figures/` contains generated figure assets.

`results/tables/` contains generated analysis tables and supporting CSV files.

Some figure filenames preserve historical internal names even when manuscript numbering changed during revision. The analysis notebooks and manuscript references should be treated as the authoritative mapping between outputs and paper figure numbers.

## Archived materials

[`archive/legacy_analysis/`](archive/legacy_analysis/) contains superseded analysis notebooks retained for research history.

These files are not part of the recommended reproduction path and should not be used in place of the canonical numbered notebooks.

## Citation

If you use this code, annotation resource, or processed dataset, please cite:

**Chelsea Chen, Michelle Lin, and David Bamman. “Measuring Interiority: Representations of Inner Life in Fiction at Scale.”**

Final proceedings metadata should be used once available.

## Authors

Chelsea Chen<br>
Michelle Lin<br>
David Bamman

School of Information, University of California, Berkeley

## AI usage

AI assisted coding tools were used during parts of code development, repository organization, and code cleanup. All research decisions, analyses, interpretations, and final manuscript decisions were reviewed and made by the authors. Human annotation was conducted manually.
