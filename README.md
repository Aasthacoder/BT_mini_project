# Drug Repurposing Using Biomedical Knowledge Graphs

Artificial intelligence is increasingly being explored as a tool for drug discovery, but its practical value depends on the quality of the underlying biological data and the reliability of model evaluation. Bender et al. (2026) highlight the need to assess AI models based on their relevance to real drug-discovery decisions rather than relying solely on computational performance metrics. Motivated by these considerations, this project investigates drug–disease link prediction using the Hetionet biomedical knowledge graph and TransE embeddings. The predicted associations are treated as computational hypotheses that require further biological validation, rather than as confirmed therapeutic recommendations.

## Overview

This project explores the use of biomedical knowledge graph embeddings for predicting potential drug–disease treatment relationships that are not explicitly recorded in an existing knowledge graph.

The project uses **Hetionet v1.0**, a frequency-based baseline, and the **TransE** knowledge graph embedding model to evaluate treatment link prediction and rank candidate drug–disease pairs.

## Objectives

- Explore and preprocess the Hetionet biomedical knowledge graph.
- Split recorded drug–disease treatment relationships into training, validation, and test sets.
- Implement a frequency-based baseline.
- Train a TransE embedding model.
- Evaluate model performance using AUROC, AUPRC, MRR, and Hits@K.
- Rank unrecorded drug–disease pairs and inspect their graph connections.

## Dataset

**Dataset:** Hetionet v1.0

| Characteristic | Value |
|---|---:|
| Nodes | 47,031 |
| Edges | 2,250,197 |
| Unique entities in processed triples | 45,158 |
| Relation types | 16 |
| Recorded treatment relationships | 755 |
| Unique drugs in treatment relationships | 387 |
| Unique diseases in treatment relationships | 77 |

The graph contains compounds, diseases, genes, anatomical structures, and other biomedical entities connected through different relationship types.

## Project Workflow

1. **Graph exploration:** Load Hetionet and inspect its nodes, edges, and relationship types.
2. **Preprocessing:** Convert the graph into head–relation–tail triples and prepare the treatment relationship splits.
3. **Baseline evaluation:** Implement a frequency-based scoring method using drug and disease treatment frequencies.
4. **TransE training:** Train the embedding model using the training graph.
5. **Model evaluation:** Compare the baseline and TransE on validation data and evaluate TransE on held-out treatment links.
6. **Candidate ranking:** Score unrecorded drug–disease pairs and inspect selected graph paths.

## Model Configuration

| Parameter | Value |
|---|---|
| Model | TransE |
| Embedding dimension | 50 |
| Training epochs | 5 |
| Batch size | 1,024 |
| Random seed | 42 |
| Training device | CPU |

## Results

### Validation Performance

| Metric | Frequency baseline | TransE |
|---|---:|---:|
| AUROC | 0.9226 | 0.6113 |
| AUPRC | 0.9196 | 0.5901 |

The frequency-based baseline outperformed TransE on the reported validation metrics.

### Held-Out Test Ranking

| Metric | TransE |
|---|---:|
| Mean Reciprocal Rank (MRR) | 0.0777 |
| Hits@10 | 0.1974 |
| Hits@50 | 0.6842 |

These ranking metrics are reported separately from the validation results.

### Top Five Candidate Pairs

| Rank | Drug | Disease | TransE score |
|---:|---|---|---:|
| 1 | Dexamethasone | Coronary artery disease | -3.7468 |
| 2 | Amiodarone | Hypertension | -3.7539 |
| 3 | Maprotiline | Coronary artery disease | -3.9108 |
| 4 | Dexamethasone | Autistic disorder | -3.9433 |
| 5 | Dasatinib | Hypertension | -3.9452 |

Higher scores indicate higher rankings according to the model's scoring function. These scores are not probabilities of treatment success.

## Repository Structure

```text
mini_project/
├── data/
│   ├── raw/
│   │   ├── hetionet-v1.0.json.bz2
│   │   └── hetionet-v1.0-metagraph.json
│   └── processed/
│       ├── hetionet_triples.csv
│       ├── treats_triples.csv
│       └── splits/
├── Notebooks/
│   ├── 01_hetionet_exploration.ipynb
│   ├── 02_graph_preprocessing.ipynb
│   ├── 03_baseline_and_splits.ipynb
│   ├── 04_transe_training.ipynb
│   ├── 05_evaluation_and_candidates.ipynb
│   └── 06_candidate_ranking.ipynb
├── models/
│   └── transe_model/
├── results/
│   └── tables/
├── src/
├── final_project_report.md
└── README.md
```

*Note: The structure shows the intended organization of the project. Include the generated files that are actually present in your final submission.*

## Requirements

The project was developed using Python and the following main libraries:

- PyKEEN
- PyTorch
- pandas
- NumPy
- scikit-learn
- Matplotlib
- Jupyter Notebook

## Running the Project

1. Clone or download the project repository.
2. Ensure Python and the required libraries are installed.
3. Open the project directory in a terminal.
4. Start Jupyter Notebook.
5. Run the notebooks in the following order:

   1. `01_hetionet_exploration.ipynb`
   2. `02_graph_preprocessing.ipynb`
   3. `03_baseline_and_splits.ipynb`
   4. `04_transe_training.ipynb`
   5. `05_evaluation_and_candidates.ipynb`
   6. `06_candidate_ranking.ipynb`

The notebooks use relative paths to load input data and save processed data, models, and results. Run them from the expected project structure and ensure the required raw dataset files are available.

## Limitations

- The frequency-based baseline performed better than TransE on the reported validation metrics.
- Sampled unrecorded pairs are not confirmed negative treatment examples.
- Indirect graph connections may represent associations or side effects rather than therapeutic mechanisms.
- Candidate scores are relative model scores, not treatment probabilities.
- The predicted relationships have not been experimentally or clinically validated.

## Conclusion

This project demonstrates a workflow for biomedical link prediction using a knowledge graph, a frequency-based baseline, and TransE embeddings. The resulting candidate rankings provide exploratory hypotheses for further investigation, not validated drug-repurposing recommendations.

## Reference article

- Huang, K., Chandak, P., Wang, Q., et al. (2024). *A foundation model for clinician-centered drug repurposing*. **Nature Medicine**. https://doi.org/10.1038/s41591-024-03233-x

My reference paper is a 2024 Nature Medicine study introducing TxGNN for drug repurposing using biomedical knowledge graphs. My project addresses the same drug–disease link prediction problem, but uses Hetionet and TransE embeddings instead of a graph neural network. I implemented a frequency baseline, evaluated the model, ranked unrecorded drug–disease pairs, and inspected graph paths. The paper provides the research motivation, while my project demonstrates a simpler approach without claiming to reproduce its results.

## References

- Himmelstein, D. S., et al. (2017). *Systematic integration of biomedical knowledge prioritizes drugs for repurposing*. eLife, 6, e26726. https://doi.org/10.7554/eLife.26726
- Hetionet: https://het.io/
- PyKEEN documentation: https://pykeen.readthedocs.io/
