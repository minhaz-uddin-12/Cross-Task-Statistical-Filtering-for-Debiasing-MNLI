# Cross-Task Statistical Filtering for Debiasing MNLI

Minhaz Uddin, Istieaque Chowdhury, Antu Chowdhury |
Dept. of Computer Science and Engineering, East Delta University

## What this is

NLP benchmarks like GLUE reward models that find shortcuts in the training data — surface
patterns like lexical overlap — instead of learning to actually reason about entailment. This
project prunes MNLI's training data using a generalized, non-uniform-prior z-statistic and a
cross-task stability check (replacing the paid human annotation used in prior work), then trains
a real BERT-base model on the pruned data and reports what happens: a genuine robustness gain on
HANS, alongside real costs on MNLI's own validation set and on SNLI-hard.

Full method, results, and the honest trade-offs are in `paper/main.pdf`.

## Repository contents

```
paper/
  main.tex              LaTeX source (IEEE conference format)
  main.pdf              Compiled paper
  references.bib        Bibliography
  figures/               All figures used in the paper
notebook/
  glue-paper-final.ipynb  Full pipeline: data → pruning → training → evaluation → error analysis
slides/
  GLUE_Paper_Presentation.pptx
```

## Key results

| | Original | Pruned |
|---|---|---|
| MNLI dev accuracy | 84.41% | 80.77% |
| HANS accuracy | 52.81% | **57.45%** |
| SNLI-hard accuracy | **71.63%** | 66.57% |

The HANS gain isn't uniform — it comes from a large improvement on the subsequence and
constituent heuristics that's partly offset by a drop on lexical overlap, the exact heuristic the
pruning targeted. Section VI of the paper (Error Analysis) traces this "overshoot" down to the
sentence level using the models' actual saved predictions.

## Reproducing this

The notebook is a single, continuous pipeline — running it top to bottom on a Kaggle GPU session
reproduces every number and figure in the paper, including the final error-analysis tables and
confusion matrices. Expect roughly 3 hours of GPU time (BERT-base fine-tuning, two conditions,
three epochs each).

## Citation

If you use this work, please cite the paper (full BibTeX entry in `paper/references.bib`, key
entries for the two closest prior works are `wang2022identifying` and `wu2022generating`).
