# Exploratory Data Analysis of Participants

This exploratory analysis asks whether resting blood pressure differs across chest pain types and resting ECG results, and whether cholesterol differs across resting ECG results, using the UCI Heart Disease dataset (n = 297 after removing 6 incomplete records). Supporting checks examine whether sex is associated with resting blood pressure, cholesterol, and maximum heart rate achieved during exercise, and whether age is associated with maximum heart rate. The aim is to describe patterns and identify relationships worth further investigation, not to establish causes or provide clinical guidance.

## Source

Data was taken from the UCI Machine Learning Repository. The dataset was published on 30 June 1988 (Janosi et al., 1988).

## DISCLAIMER

This project was not done by or reviewed by a medical professional. This project was developed with the assistance of Claude, Gemini, and ChatGPT. Portions of the code and README, such as the syntax of the final p-value correction and the results table in the README, were generated or refactored using AI. AI also helped provide feedback and advice during the creation of the draft and README. All code was reviewed, tested, and modified by the project author.

## Key Findings

- Age was negatively associated with maximum heart rate. In the simple linear regression, maximum heart rate decreased by an estimated 1.00 beat per minute per additional year of age (95% confidence interval: 0.73 to 1.27 beats per minute lower). Age explained approximately 15.6% of the variation in maximum heart rate (R² = 0.156; FDR-adjusted p < 0.001).
- Participants with resting ECG type 2 tended to have higher resting blood pressure than those with ECG type 0 (FDR-adjusted p = 0.012). The rank-biserial correlation was approximately -0.17 when comparing type 0 with type 2, indicating a small rank-based difference.
- The female group had higher mean cholesterol than the male group (FDR-adjusted p = 0.006; Cohen's d = 0.43, a small standardized difference).
- Among women, cholesterol tended to be higher for ECG type 2 than type 0 (FDR-adjusted p = 0.012). The corresponding test among men was not statistically significant (FDR-adjusted p = 0.107). **These two results do not, on their own, establish that the ECG–cholesterol relationship differs by sex.**

## Data

Data description of selected fields (not all fields were used in hypothesis testing):

- **Age:** Age (years).
- **Sex:** 0 = Female, 1 = Male.
- **CP:** Chest pain type: 1 = Typical angina; 2 = Atypical angina; 3 = Non-anginal pain; 4 = Asymptomatic.
- **Trestbps:** Resting blood pressure at admission (mmHg).
- **Chol:** Serum cholesterol (mg/dL).
- **Fbs:** Fasting blood sugar: 1 = above 120 mg/dL; 0 = 120 mg/dL or below.
- **Restecg:** Resting ECG result: 0 = Normal; 1 = ST–T wave abnormality; 2 = Probable or definite left ventricular hypertrophy by Estes' criteria.
- **Thalach:** Maximum heart rate achieved (beats per minute).
- **Exang:** Exercise-induced angina: 1 = Yes; 0 = No.
- **Ca:** Number of major vessels (0–3) colored by fluoroscopy.

Variable definitions follow the UCI dataset documentation (Janosi et al., 1988).

## Methodology

**1. Cleaning:** Rows containing null values were dropped from the dataset, leaving 297 of the original 303 records. Missing values occurred in `ca` (4 records) and `thal` (2 records), neither of which was used in the reported hypothesis tests.

**2. Descriptive analysis:** Counts broken down by male and female participants were printed, and a pie chart was plotted to show the breakdown by chest pain type. Descriptive statistics, a correlation heatmap, bar charts, and boxplots were generated to explore differences between groups before statistical testing. Upon seeing the visualised differences between groups, hypotheses were generated, which were tested in steps 3 & 4.

**3. Exploring differences by sex:** One-sided t-tests were conducted to compare maximum heart rate, resting blood pressure, and cholesterol between men and women. Mean cholesterol was higher among women after p-value adjustment, so the ECG–cholesterol comparisons were explored separately by sex. **This grouping was exploratory; a significant difference in cholesterol between sexes does not by itself establish that sex is a confounding factor or that the ECG–cholesterol relationship differs by sex.**

**4. Testing:** Kruskal–Wallis was used to compare resting blood pressure across chest pain types. A one-sided Mann–Whitney U test compared resting blood pressure between ECG types 0 and 2. For cholesterol, a one-sided t-test was used for ECG types 0 and 2 among men, and a one-sided Mann–Whitney U test was used among women. Simple linear regression was used to assess the association between age and maximum heart rate, reporting the slope, R², and p-value. The Benjamini–Hochberg false discovery rate (FDR) procedure was applied to the eight reported p-values. Because several tests used one-sided alternatives in an exploratory analysis, their results should be interpreted cautiously.

## Results

### Statistical Test Results

| Test | p_raw | sig_fdr | p_fdr |
|---|---:|:---:|---:|
| t-test: max HR, F > M | 1.319145e-01 | False | 1.395706e-01 |
| t-test: resting BP, F > M | 1.395706e-01 | False | 1.395706e-01 |
| t-test: cholesterol, F > M | 1.541476e-03 | True | 6.165902e-03 |
| Kruskal–Wallis: resting BP by chest pain type | 5.485338e-02 | False | 8.776541e-02 |
| Mann–Whitney U: resting BP, ECG 0 < 2 | 5.996833e-03 | True | 1.199367e-02 |
| t-test: cholesterol, ECG 0 < 2 (male) | 8.018521e-02 | False | 1.069136e-01 |
| Mann–Whitney U: cholesterol, ECG 0 < 2 (female) | 4.342867e-03 | True | 1.158098e-02 |
| Regression: max HR ~ age | 1.670125e-12 | True | 1.336100e-11 |

**Significance threshold:** α = 0.05. `p_raw` is the unadjusted p-value, `p_fdr` is the Benjamini–Hochberg-adjusted p-value, and `sig_fdr` indicates whether the adjusted p-value is below 0.05. In directional tests, `F > M` means female greater than male, and `ECG 0 < 2` means ECG type 0 lower than type 2.

**Summary:** 4 out of 8 statistical tests were significant after p-value adjustment. The strongest quantified relationship was the negative association between age and maximum heart rate, although age accounted for only about 15.6% of its variation. The resting blood pressure difference between ECG types 0 and 2 had a small rank-based effect. These findings describe associations in this sample, not causal or clinical effects. A nonsignificant test is not proof that no difference exists.

## Limitations

- Data was published more than 30 years ago and may no longer be representative of current populations.
- Male and female sample sizes were unequal (201 male and 96 female participants). Only four participants had ECG type 1, so ECG comparisons focused on types 0 and 2.
- Removing all rows with missing values reduced the sample size by six, although missingness was limited to fields not used in the hypothesis tests. Rerunning the analysis with these records retained would be a useful sensitivity check.
- The analysis is exploratory and observational. One-sided tests and multiple comparisons warrant caution, even after FDR adjustment. Statistical significance does not necessarily imply a large or clinically meaningful difference.
- Dividing the cholesterol analysis by sex does not establish confounding or a sex-specific effect. An adjusted model and an interaction test would be needed to investigate those questions more directly.

## Requirements for replication

- Python version: 3.14.6
- NumPy version: 2.4.6
- pandas version: 3.0.3
- pingouin version: 0.7.0
- Matplotlib version: 3.11.0
- Seaborn version: 0.13.2
- ucimlrepo version: 0.0.7
- Jupyter Notebook or JupyterLab

Install the Python packages with:

`pip install numpy pandas pingouin matplotlib seaborn ucimlrepo`

Run `HeartDisease.ipynb` in Jupyter. An internet connection is needed to retrieve the dataset using `ucimlrepo`.

## Contact

estherpig999@gmail.com

## Citations

1. Janosi, A. et al. (1988) *UCI Machine Learning Repository*. https://archive.ics.uci.edu/dataset/45/heart+disease (Accessed: October 29, 2026).
2. Prabhavathi, K. et al. (2014) *Role of Biological Sex in Normal Cardiac Function and in its Disease Outcome – A Review*. https://pmc.ncbi.nlm.nih.gov/articles/PMC4190707/ (Accessed: October 29, 2026).
3. AlBadri, A. et al. (2017) 'Typical angina is associated with greater coronary endothelial dysfunction but not abnormal vasodilatory reserve,' *Clinical Cardiology*, 40(10), pp. 886–891. https://doi.org/10.1002/clc.22740.
4. Cleveland Clinic (2026) *Atypical chest pain*. https://my.clevelandclinic.org/health/symptoms/24935-atypical-chest-pain (Accessed: October 29, 2026).
5. Cleveland Clinic (2025) *Noncardiac chest pain*. https://my.clevelandclinic.org/health/diseases/15851-gerd-non-cardiac-chest-pain (Accessed: October 29, 2026).
6. McMahon, K. and Ahmed, I. (2025) *ECG T Wave*. https://www.ncbi.nlm.nih.gov/books/NBK538264/ (Accessed: October 29, 2026).
7. SUNY OER Services (no date) *Cardiac Cycle | Anatomy and Physiology II*. https://courses.lumenlearning.com/suny-ap2/chapter/cardiac-cycle/ (Accessed: October 29, 2026).
8. Kashou, A. H., Basit, H. and Malik, A. (2023) *ST Segment*. https://www.ncbi.nlm.nih.gov/books/NBK459364/ (Accessed: October 29, 2026).
9. Cleveland Clinic (2025a) *Left ventricular hypertrophy*. https://my.clevelandclinic.org/health/diseases/21883-left-ventricular-hypertrophy (Accessed: October 29, 2026).