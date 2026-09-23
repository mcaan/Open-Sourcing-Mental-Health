# Mental Health in Tech: Treatment-Seeking Behavior

An end-to-end data science analysis of Open Sourcing Mental Health (OSMH) survey data examining how individual mental health experiences and workplace environments relate to treatment-seeking behavior.

## Project Overview

Mental health resources may exist within an organization without necessarily translating into treatment-seeking behavior. This project examines the factors associated with whether respondents seek mental health treatment and asks two related questions:

1. What individual and workplace factors distinguish people who seek treatment?
2. How does workplace environment affect treatment-seeking among people with different levels of personal readiness to seek care?

The analysis combines nine years of OSMH Mental Health in Tech survey data (2014 and 2016–2023) and develops a workflow spanning data harmonization, exploratory analysis, dimensionality reduction, behavioral segmentation, propensity modeling, and interpretation.

The resulting framework separates two dimensions of treatment-seeking behavior:

* **Personal readiness** — whether mental health feels salient enough for an individual to act.
* **Environment** — whether workplace resources and information make that action easier.

The analysis suggests that personal readiness is the primary driver of treatment-seeking, while workplace environment becomes especially important among individuals with lower intrinsic readiness.

## Key Findings

### Personal readiness is the strongest treatment-related signal

Principal component analysis identified **PC4** as the component most strongly associated with treatment status.

PC4 primarily captures factors related to:

* family history of mental illness;
* interference of mental health with work;
* observed workplace consequences; and
* comfort taking leave for mental health reasons.

Treatment groups differed substantially on PC4 (Cohen's *d* ≈ 0.58), making it the strongest treatment-related PCA dimension in the analysis.

Rather than representing employer support directly, PC4 is interpreted as a latent measure of **mental health salience and readiness to act**.

### Workplace environment matters most when readiness is lower

Clustering identified three respondent segments with different combinations of personal readiness, workplace support, and awareness of available resources.

Among respondents with high PC4, treatment rates were approximately 68–71% across all three clusters. This indicates that individuals with strong treatment-seeking readiness often seek treatment regardless of cluster membership.

The differences became larger among respondents with low PC4:

| Cluster   | Treatment rate among Low-PC4 respondents |
| --------- | ---------------------------------------: |
| Cluster 0 |                                    32.2% |
| Cluster 1 |                                    32.4% |
| Cluster 2 |                                    48.8% |

Cluster 2 therefore showed substantially higher treatment uptake among respondents who were otherwise less predisposed toward treatment.

This supports the central interpretation of the project:

> **Needs drive action; environment shapes accessibility.**

### Predictive modeling supports the same pattern

Three classification approaches were evaluated.

| Model                           | Accuracy | ROC AUC | Brier Score |
| ------------------------------- | -------: | ------: | ----------: |
| Logistic Regression             |    0.676 |   0.723 |       0.213 |
| Elastic-Net Logistic Regression |    0.678 |   0.723 |       0.213 |
| Decision Tree                   |    0.657 |   0.692 |           — |

Regularization produced little change in predictive performance or coefficient structure, while the shallow decision tree performed somewhat worse.

Across the models, **PC4 remained the strongest treatment-related latent factor**. The decision tree also showed that when PC4 was lower, workplace support and secondary latent factors became more relevant to treatment classification.

The models are used primarily to understand treatment propensity and relationships in the survey data rather than to propose a production prediction system.

## Organizational Implications

The analysis suggests two complementary intervention strategies.

**Reduce friction for people who are less inclined to seek treatment.** Clear care options, accessible benefits, simplified pathways to care, and normalization of help-seeking may have the greatest opportunity to influence employees whose personal readiness is relatively low.

**Prevent access failures among people with high need.** Respondents with strong treatment-related signals but uncertainty about workplace resources represent a different problem: intent may already exist, but employees may not know how to translate that intent into action.

Potential organizational actions include:

* centralized and easy-to-understand mental health resource hubs;
* clear instructions for accessing treatment;
* manager training for directing employees to appropriate resources;
* proactive communication about available benefits; and
* reducing unnecessary friction in initiating care.

The broader implication is that interventions should consider both **uptake** and **accessibility** rather than assuming resource availability alone is sufficient.

## Data

Survey data was obtained from **Open Sourcing Mental Health (OSMH)**:

https://osmhhelp.org/research.html

The project uses surveys from:

`2014, 2016, 2017, 2018, 2019, 2020, 2021, 2022, 2023`

Survey instruments changed across years, so the ETL process harmonizes selected questions into a common analytical schema.

Raw survey files are not included in this repository. See [`data/raw/README.md`](data/raw/README.md) for acquisition and placement instructions.

## Analytical Workflow

```text
Raw OSMH survey files
        ↓
1. ETL
        ↓
cleaned_data.csv
   ├──────────────→ 2. Exploratory Data Analysis
   ↓
3. Dimensionality Reduction
   ├──→ PC4_loadings.csv
   ↓
full_data.csv
   ↓
4. Clustering & Segmentation
   ↓
clustered_data.csv
   ↓
5. Propensity Modeling
   ↓
Results, Interpretation & Recommendations
```

Generated datasets are stored locally under `data/processed/` and excluded from Git.

## Methodology

### 1. ETL

Nine annual survey datasets are combined and harmonized into a common schema.

The process includes:

* aligning differently worded survey questions across years;
* cleaning demographic and workplace variables;
* normalizing categorical responses;
* encoding analytical features; and
* constructing derived measures used downstream.

### 2. Exploratory Data Analysis

EDA examines:

* missingness across survey years;
* respondent demographics;
* workplace characteristics;
* mental health experiences;
* employer support measures;
* treatment-seeking behavior; and
* relationships between candidate predictors and treatment.

Missingness varies substantially by survey year because the OSMH questionnaire changed over time.

### 3. Dimensionality Reduction

**PCA** is used to reduce correlated survey variables into interpretable latent dimensions. Ten principal components retain approximately 90% of the variance in the selected feature set.

**UMAP** provides a complementary nonlinear two-dimensional representation for visualization and assessment of respondent structure.

PCA ultimately provided stronger treatment-related quantitative signals, while UMAP was useful for visualizing nonlinear respondent structure.

### 4. Clustering & Segmentation

K-Means clustering is applied to the PCA representation.

Elbow and silhouette analysis support a three-cluster solution, which is interpreted using the original survey features and treatment behavior.

The resulting segments distinguish respondents with different combinations of treatment readiness, workplace support, and awareness of available resources.

### 5. Propensity Modeling

Treatment-seeking propensity is evaluated using:

* logistic regression;
* elastic-net logistic regression; and
* a shallow decision tree.

Evaluation focuses on discrimination, classification performance, probability quality where applicable, coefficient stability, and interpretability.

### 6. Interpretation

The final analysis combines dimensionality reduction, segmentation, and propensity modeling into a two-axis framework:

**Personal readiness**

> Do I feel this matters, and am I ready to act?

**Environment**

> Can I act easily, and do I understand how?

This framework is used to translate the statistical findings into organizational recommendations.

## Repository Structure

```text
Open-Sourcing-Mental-Health/
│
├── data/
│   ├── raw/
│   │   └── README.md
│   ├── processed/
│   │   └── README.md
│   └── README.md
│
├── notebooks/
│   ├── 1. OSMH ETL.ipynb
│   ├── 2. OSMH EDA.ipynb
│   ├── 3. OSMH Dimensionality Reduction.ipynb
│   ├── 4. OSMH Clustering & Segmentation.ipynb
│   ├── 5. OSMH Propensity Modeling.ipynb
│
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

## Reproducing the Analysis

Clone the repository and install the required Python dependencies:

```bash
pip install -r requirements.txt
```

Download the required OSMH survey files using the instructions in `data/raw/README.md` and place them in `data/raw/`.

Run the notebooks sequentially:

```text
1. OSMH ETL.ipynb
2. OSMH EDA.ipynb
3. OSMH Dimensionality Reduction.ipynb
4. OSMH Clustering & Segmentation.ipynb
5. OSMH Propensity Modeling.ipynb
```

The notebooks use repository-relative paths, so no machine-specific directory configuration should be necessary.

## Limitations

Several limitations should be considered when interpreting the analysis.

* **Survey questions changed over time.** Variables were harmonized across survey years, but similarly themed questions are not always identical.
* **Missingness is year-dependent.** Changes in questionnaire design produce systematic differences in feature availability.
* **Later survey years contain smaller samples.**
* **The data is observational and self-reported.** Relationships identified here should not be interpreted as causal effects of workplace policies or benefits.
* **Latent dimensions are analytical constructs.** Interpretations such as "personal readiness" summarize patterns in PCA loadings and downstream behavior rather than directly measured psychological constructs.
* **Clusters are descriptive segments.** They provide a useful framework for understanding respondent heterogeneity but should not be treated as fixed population categories.
* **Predictive performance is moderate.** The models are intended primarily for interpretation and propensity analysis rather than deployment as clinical or individual-level decision systems.

## Acknowledgments

Survey data was provided by Open Sourcing Mental Health (OSMH).

The initial ETL approach was informed by Mariam Raafat Mohamed's *Mental Health Tech: 9 Years of Insights & Prediction* analysis. The ETL methodology was modified for this project, including retention of additional variables and changes to response encoding. Subsequent analytical notebooks represent the project's own analysis.

## License

Original code in this repository is available under the MIT License. See [`LICENSE`](LICENSE).

The OSMH survey datasets are externally sourced and are not distributed as part of this repository.