# Module Summary: Titanic Data Workflow Project

## Overview

This project implements a complete, reproducible data workflow analyzing the Titanic passenger dataset. Using Python, Pandas, and visualization libraries, I built a pipeline that ingests raw data, cleans missing values, performs exploratory analysis, and generates visualizations to understand factors influencing passenger survival rates.

**Project GitHub repo**: https://github.com/sam-andaluri/ai-programming-foundations-project

## Dataset Description

**Dataset:** [Titanic - Machine Learning from Disaster](https://www.kaggle.com/c/titanic)

The analysis uses the Titanic passenger dataset from Kaggle's *Titanic: Machine Learning from Disaster* competition (Kaggle, n.d.). The dataset contains information about 891 passengers aboard the RMS Titanic, which sank on April 15, 1912. The dataset includes 12 columns capturing passenger demographics (age, sex), travel details (passenger class, fare, cabin), family relationships (siblings/spouses, parents/children), and survival outcome. Key variables analyzed include `Survived` (target), `Pclass` (socioeconomic status proxy), `Sex`, `Age`, and `Fare`. The dataset presents realistic data quality challenges including missing values in Age (19.9%), Cabin (77.1%), and Embarked (0.2%) columns.

## Workflow Description

The data workflow follows a structured five-stage pipeline:

1. **Ingestion:** Load the CSV file using Pandas and perform initial inspection of shape, data types, and missing values.

2. **Cleaning:** Apply two custom functions to handle missing data. `fill_missing_age()` uses group-wise median imputation, while `clean_categorical_columns()` processes the Embarked and Cabin columns.

3. **Exploratory Analysis:** Execute the `perform_eda()` function to generate summary statistics, calculate survival rates by demographic groups, and compute correlations between numeric features.

4. **Visualizations:** Create three plots, a grouped bar chart for survival by class and sex, a histogram with KDE for age distribution, and a correlation heatmap, each with proper titles and labeled axes.

5. **Summary:** Document findings, patterns, limitations, and surprising observations in narrative form.

## Key Decisions and Assumptions

### Cleaning Choices

**Age Imputation Strategy:** Rather than using a single global median to fill missing ages, I implemented group-wise median imputation based on passenger class and sex. This decision preserves the relationship between age and demographic factors observed in the dataset. Using a global median would have introduced artificial homogeneity and potentially masked real patterns in the survival analysis.

**Cabin Column Handling:** With 77% of cabin values missing, meaningful imputation was not feasible. I converted this column to a binary `HasCabin` feature indicating whether a passenger had a recorded cabin number. This transformation retains predictive signal while acknowledging the limitation that specific cabin locations cannot be analyzed.

**Embarked Imputation:** The two missing Embarked values were filled using mode imputation (Southampton), which is appropriate given the small number of missing values and the categorical nature of the variable.

### EDA Focus

The exploratory analysis prioritized survival rate comparisons across demographic segments rather than exhaustive statistical testing. This choice aligns with the project's goal of building intuition about the data before future ML modeling, where these features would become predictors.

### Visualization Design

- **Figure 1 (Bar Chart):** Designed to show the interaction between class and sex on survival, revealing that both factors independently and jointly influenced outcomes.
- **Figure 2 (Histogram):** Compares age distributions between survivors and non-survivors to identify whether age affected survival chances.
- **Figure 3 (Heatmap):** Provides a comprehensive view of numeric feature correlations to identify multicollinearity and potential predictors for future modeling.

## Results and Interpretation

The analysis revealed three primary findings:

1. **Gender was the dominant survival factor.** Women survived at 74% compared to 19% for men (Figure 1), reflecting the "women and children first" evacuation protocol.

2. **Socioeconomic status strongly influenced survival.** First-class passengers had 63% survival versus 24% for third-class (Figure 1). The correlation heatmap (Figure 3) confirms this relationship, showing negative correlation between Pclass and Survived (-0.34) and positive correlation between Fare and Survived (0.26).

3. **Age effects were secondary but notable.** Figure 2 suggests that younger passengers had better survival outcomes overall, and grouped age summaries show that children under 10 had elevated survival rates while young adult males (20-30) comprised the largest group of casualties.

The `HasCabin` feature showed a 0.32 correlation with survival (Figure 3), suggesting that cabin assignment likely acted as a proxy for wealth and location within the ship.

## Responsible Practice (Bias and Data Quality)

Several aspects of data handling could introduce bias:

**Survivorship Bias in Records:** The dataset only includes passengers with recorded information. If record-keeping varied by class, the dataset may underrepresent certain groups.

**Imputation Bias:** Group-wise median imputation for age assumes missing values are missing at random within each class-sex group. If that assumption is false, the imputed ages could distort age-related survival conclusions.

**Feature Engineering Choices:** Converting Cabin to binary `HasCabin` loses location detail that could have affected survival outcomes.

To mitigate these risks, I documented all transformations with clear docstrings, avoided over-interpreting heavily imputed variables, and acknowledged the main limitations in the written summary.

## Reproducibility

This project follows reproducibility best practices consistent with guidance discussed by Danchev (2022):

1. **Dependency Management:** All required packages are captured in `requirements.txt` from the working environment.

2. **Version Control:** The project uses Git with multiple commits tracking incremental progress. A separate development branch isolates work-in-progress changes from the stable main branch.

3. **Self-Contained Workflow:** The Jupyter notebook executes top-to-bottom without external dependencies beyond the included CSV file. All functions are defined within the notebook with comprehensive docstrings.

4. **Clear Documentation:** The README provides step-by-step instructions for running the analysis, and this report explains the main methodological decisions.

Following these practices helps another researcher reproduce the same workflow and outputs (Peng, 2011).

## References

Peng, R. D. (2011). Reproducible research in computational science. *Science, 334*(6060), 1226-1227. https://doi.org/10.1126/science.1213847

Danchev, V. (2022). Reproducible data science with Python: An open learning resource. *Journal of Open Source Education, 5*(56), 156. https://doi.org/10.21105/jose.00156

Kaggle. (n.d.). *Titanic: Machine learning from disaster* [Data set]. Kaggle. Retrieved April 9, 2026, from https://www.kaggle.com/c/titanic
