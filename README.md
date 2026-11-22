<div align="center">

# DO YOU EVEN LOAF? 🍞
### Does Seoul Bulk or Cut?

**CDS 303 - Scientific Data Mining | Final Team Project**

</div>

---

## Project Overview

**Team Members:** 
Santiago Rodriguez Gomez, Saebom Park

### Research Question
> Can Seoul's 행정동 be grouped based on fitness- and bakery-related commercial activity, and what characteristics distinguish these groups?

**Goal:** Identify groups of 행정동 through explainable cluster analysis and explore their relationship with regional obesity rates.

**Audience:** urban planners, public health policymakers, and public health program planners.

### Data

2025 data from Seoul Open Data Plaza at the 행정동 level (estimated sales, stores, resident and worker population),
plus sports facility licenses and 자치구 obesity rates.
Sources, access links, and processing notes are in [data/README.md](data/README.md).

---

## Reports

| Session | Topic | Report |
|---|---|---|
| 1 | Research methodology, paper summary, variable selection | [Session1.md](Session1.md) |
| 2 | Data integration, refined research question, PCA | [Session2.md](Session2.md) |
| 3 | Clustering and classification | *Coming soon* |

## Notebooks

| Notebook | What it does |
|---|---|
| [01_data_integration.ipynb](notebooks/01_data_integration.ipynb) | Merges the datasets into one row per 행정동 and prepares the PCA input |
| [02_pca.ipynb](notebooks/02_pca.ipynb) | Runs PCA, selects components, and interprets loadings and the biplot |

## How to Reproduce

1. Clone the repository.
2. Install the requirements: `pip install -r requirements.txt`
3. Run `notebooks/01_data_integration.ipynb`, then `notebooks/02_pca.ipynb`. Run them from inside the `notebooks/` folder, since file paths are relative to it.

## Repository Structure

- `Session1.md`, `Session2.md`: reports
- `notebooks/`: analysis notebooks (run in order)
- `data/raw/`: original data (translated to English)
- `data/processed/`: outputs of the notebooks
- `figures/`: saved plots
- `requirements.txt`: Python packages
