# Bike Sharing Demand Analysis

## PROG8431 – Data Analysis, Mathematics, Modeling and Algorithms

## Group 7 – Data Analysis

### Team Members

- Sukhchain Singh – 9111541
- Preethi – 9125985
- Arya – 9084843

## Project Overview

We study daily Capital Bikeshare rentals in Washington, D.C. We compare seasons, group days with similar demand, and test how accurately weather and calendar information can predict rental counts.

The project includes three related analyses:

1. **Statistical analysis:** Explore rental distributions and compare Winter and Summer using descriptive statistics and statistical tests.
2. **K-means clustering:** Identify groups of days with similar temperatures and rental counts, then examine their characteristics and seasonal patterns.
3. **[Workshop C – Regression](src/Workshop_C/Group_7_Bike_Sharing_Workshop_C.ipynb):** Compare temperature alone, rain alone, and two models that combine weather and calendar information.

Each record represents **one day**, not an individual customer or bicycle.

We evaluate predictions on historical days from 2011–2012. Forecasting a future season would require testing on later dates using weather information available at prediction time.

## Research Questions

### Overall Project

How are temperature and season associated with daily Capital Bikeshare rental demand?

### Statistical Analysis

How do Winter and Summer differ in their rental distributions, variability and average demand?

### K-Means Clustering

Can K-means clustering identify meaningful groups of daily bike rental demand based on temperature and total rentals, and what can these groups tell us about seasonal rental patterns?

### Workshop C – Regression

How accurately can we predict daily rentals from weather and calendar variables, and does combining predictors improve on temperature or rainfall alone?

## Dataset

We use the **Bike Sharing Dataset** from the UCI Machine Learning Repository.

**Source:** Fanaee-T, H. (2013). *Bike Sharing* [Dataset]. UCI Machine Learning Repository.

**Dataset link:** https://doi.org/10.24432/C5W894

The `day.csv` dataset contains **731 daily records from 2011 to 2012**, with 16 original columns.

Important variables include:

| Variable | Description |
|---|---|
| `dteday` | Date |
| `cnt` | Total daily bike rentals |
| `temp` | Normalized temperature |
| `atemp` | Normalized apparent temperature |
| `hum` | Normalized humidity |
| `windspeed` | Normalized wind speed |
| `season` | Season category |
| `workingday` | Working-day indicator |
| `casual` | Rentals by casual users |
| `registered` | Rentals by registered users |

## Data Checks and Cleansing

Before performing the analyses, we:

- Check for missing values.
- Check for duplicate dates and rows.
- Verify valid season codes.
- Verify that total rentals equal casual rentals plus registered rentals.
- Convert dates into datetime format.
- Convert season codes into readable season names.
- Examine relevant numerical ranges.

The statistical notebook also converts temperature to Celsius and humidity to percentages.

The clustering notebook retains temperature on its original normalized scale and standardizes the two clustering features before modelling.

**A normalized temperature value such as 0.685 is not a Celsius measurement.**

The reported checks found no missing values or duplicates requiring row removal. All 731 records were retained for clustering.

## Part A – Statistical Analysis

### Descriptive Statistics

We calculate:

- Mean
- Median
- Mode
- Sample variance
- Sample standard deviation
- Quartiles: Q1, Q2 and Q3
- Maximum, also reported as Q4 or the 100th percentile

We also compare daily rental demand across the four seasons.

### 1. Normality Analysis

We use a **QQ-plot** and the **Shapiro-Wilk test** to assess whether daily rentals follow a normal distribution.

The overall p-value was below 0.05, providing evidence of a departure from normality.

Winter and Summer were also tested separately. Both groups showed evidence of non-normality.

### 2. F-Test

We use the traditional F-test to compare Winter and Summer population variances.

- F-statistic: approximately **1.09**
- P-value: approximately **0.572**

The result did not provide enough evidence that the variances were different.

Failing to reject the null hypothesis does not prove equal variances.

Because both groups showed departures from normality, the traditional F-test result should be interpreted cautiously.

### 3. Levene's Test

We use the median-based version of Levene's test as an additional variance comparison because it is less sensitive to non-normality.

- Test statistic: **2.3327**
- P-value: **0.127543**

The result did not provide enough evidence of different Winter and Summer variances.

Daily observations may be related over time, so independence remains a limitation.

### 4. Welch's T-Test

We use Welch's t-test to compare average daily rentals without requiring equal population variances.

- Winter average: approximately **2,604 rentals per day**
- Summer average: approximately **5,644 rentals per day**
- Observed difference: approximately **3,040 rentals per day**
- T-statistic: approximately **20.42**
- P-value: approximately **1.828 × 10⁻⁶²**

The results provide evidence of different population means under the test assumptions.

However, consecutive daily observations may be related, which can affect the interpretation of the p-value.

### 5. Z-Score Analysis

We calculate Z-scores to identify days with rental counts unusually far from the overall average.

```text
Z-score = (daily rentals − mean rentals) / sample standard deviation
```

Results:

- Mean rentals: approximately **4,504.35**
- Sample standard deviation: approximately **1,937.21**
- Days with absolute Z-scores greater than 2: **10**
- Unusually low rental days: **7**
- Unusually high rental days: **3**

The threshold is an exploratory rule, not proof of incorrect records. These observations were retained.

Because the distribution departs from normality, we do not assume that approximately 95% of observations must fall within two standard deviations.

## Statistical Visualizations

The four main visualizations are:

1. **Scatterplot:** Compares temperature and daily rentals.
2. **Histogram:** Shows the distribution of daily rental totals.
3. **Boxplot:** Compares rental demand across seasons.
4. **Venn diagram:** Shows the overlap between warm days above 20°C and high-demand days with more than 5,000 rentals.

Additional charts include:

- Average daily rentals by season
- Separate Winter and Summer histograms
- Normal QQ-plot

## Part B – K-Means Clustering

### Purpose

K-means is an unsupervised learning method that groups similar observations without predefined group labels.

We use it to identify different types of bike-rental days and examine how these groups relate to seasons and other characteristics.

### Selected Clustering Features

We use two numerical features:

1. **Normalized temperature (`temp`)**
2. **Total daily rentals (`cnt`)**

Temperature describes daily conditions, while rental counts describe demand.

Season, humidity and working-day status are used afterwards to describe the groups. They are not used to form the clusters.

### Outlier Analysis

We examine temperature and rental counts using boxplots and the 1.5 × IQR rule.

```text
IQR = Q3 − Q1

Lower boundary = Q1 − 1.5 × IQR
Upper boundary = Q3 + 1.5 × IQR
```

No observations were flagged by this rule in the two selected features.

This does not prove that the dataset contains no unusual observations.

### Feature Standardization

We compare two scaling methods:

**Min-Max scaling**

```text
Scaled value = (value − minimum) / (maximum − minimum)
```

**Z-score standardization**

```text
Z-score = (value − feature mean) / feature sample standard deviation
```

We select **Z-score standardization** for clustering.

Rental counts are measured in thousands, while normalized temperatures are below one. Without standardization, rental counts could dominate distance calculations.

The final standardized input contains:

- **731 observations**
- **2 features**
- No missing or infinite values

### K-Means Implementation

We fit models for K values from 1 through 7 using:

```python
KMeans(
    n_clusters=k,
    max_iter=10,
    n_init=10,
    random_state=123
)
```

- `n_clusters`: Number of groups.
- `max_iter`: Maximum iterations per initialization.
- `n_init`: Number of initializations.
- `random_state`: Controls random choices for reproducibility in the same environment.

### Selecting the Number of Clusters

We compare models using:

- Within-cluster sum of squares
- Elbow chart
- Silhouette scores
- Cluster scatterplots
- Cluster centres
- Cluster sizes
- Total and between-cluster sums of squares

The silhouette results were approximately:

| Number of clusters | Silhouette score |
|---|---:|
| 2 | 0.5048 |
| 4 | 0.4297 |

K = 2 has the highest tested silhouette score.

We select **K = 4** because it provides more detailed and interpretable groups, while acknowledging the lower silhouette score.

This is a trade-off between separation and descriptive detail. Four clusters are not the only possible solution.

For K = 4:

- WCSS: approximately **270.484**
- BCSS as a percentage of TSS: approximately **81.47%**

The BCSS percentage describes variation in the standardized feature space. **It is not prediction accuracy.**

### Cluster Interpretation

We attach the final cluster labels to the complete cleaned dataset.

We then:

- Calculate cluster sizes and percentages.
- Calculate descriptive statistics.
- Convert standardized centres back to original feature scales.
- Compare humidity and working days.
- Examine seasonal composition.
- Assign descriptive names.
- Explain practical insights and limitations.

The inverse transformation is:

```text
Original centre =
standardized centre × original sample standard deviation
+ original mean
```

Rental centres return to rental counts. Temperature centres return to the original normalized scale, not Celsius.

## Final Cluster Results

| Cluster | Descriptive name | Days | Share of dataset | Mean normalized temperature | Mean daily rentals |
|---|---|---:|---:|---:|---:|
| 0 | Warm Days, Moderate Demand | 169 | 23.12% | 0.685 | 4,522 |
| 1 | Cool Days, Moderate Demand | 205 | 28.04% | 0.407 | 4,470 |
| 2 | Warm Days, High Demand | 179 | 24.49% | 0.634 | 7,040 |
| 3 | Cold Days, Low Demand | 178 | 24.35% | 0.279 | 1,978 |

Values are rounded for readability.

The names describe relative differences within this dataset. Individual days can differ from their cluster averages.

Cluster numbers are identifiers, not rankings. Labels may change when model settings or environments change.

### Humidity and Working Days

| Cluster | Average humidity | Working-day percentage |
|---|---:|---:|
| 0 | 67.3% | 71.6% |
| 1 | 62.1% | 70.7% |
| 2 | 61.3% | 69.3% |
| 3 | 60.8% | 61.8% |

Working-day percentage describes the share of days in each cluster classified as working days. It does not represent the percentage of customers who commute.

## Seasonal Composition of Clusters

| Cluster | Winter | Spring | Summer | Fall |
|---|---:|---:|---:|---:|
| 0 | 0.00% | 31.36% | 60.36% | 8.28% |
| 1 | 22.44% | 24.39% | 1.95% | 51.22% |
| 2 | 3.35% | 32.40% | 45.81% | 18.44% |
| 3 | 72.47% | 12.92% | 0.00% | 14.61% |

Percentages describe seasons **within each cluster**. Rounding may cause small differences from 100%.

For example, 72.47% of Cluster 3's days occurred in Winter. This does not mean that 72.47% of all Winter days belong to Cluster 3.

A **100% stacked bar chart** visualizes these patterns.

Summer days appear in both moderate-demand and high-demand clusters, showing differences beyond season labels alone.

## Part C – Predicting Daily Rentals

We matched Capital Bikeshare records with daily Open-Meteo weather observations by date. All 731 days matched. We kept 25 predictors, including rainfall, sunshine, holidays and weekdays. We excluded casual and registered rental counts because they add up to the target, `cnt`. [Workshop C notebook](src/Workshop_C/Group_7_Bike_Sharing_Workshop_C.ipynb)

We trained each model on the same 511 days and tested it on the remaining 220 days. For the smaller model, we started with all 25 predictors and used backward elimination on the training data. Ten predictors remained. The table reports training R² and prediction error in rentals per day. A lower test RMSE means closer predictions on the held-out days.

| Model | R² | Adjusted R² | Training RMSE | Test RMSE |
|---|---:|---:|---:|---:|
| Temperature only | 0.3980 | 0.3968 | 1,480.05 | 1,569.03 |
| Rainfall only | 0.0487 | 0.0468 | 1,860.56 | 1,891.61 |
| Full model (25 predictors) | 0.8791 | 0.8662 | 663.40 | **651.19** |
| Smaller model (10 predictors) | 0.8735 | 0.8664 | 678.56 | 676.98 |

The full model gave the lowest test error. Its test RMSE was **58.5% lower** than temperature alone. Removing 15 predictors increased test RMSE by only **25.79 rentals per day**, so the smaller model came close with fewer inputs.

## Practical Business Insights

### 1. Investigate Capacity for High-Demand Periods

The high-demand cluster averages approximately 7,040 rentals per day.

These historical observations suggest periods worth investigating when planning staffing and serviceable bike availability.

Station-level information would be needed to determine where bikes should be placed.

### 2. Consider Maintenance During Quieter Periods

The low-demand cluster averages approximately 1,978 rentals per day and mainly contains Winter days.

Historically quieter periods may provide maintenance opportunities, subject to current demand.

### 3. Consider More Than Temperature

The warmest cluster does not have the highest average demand.

Planning should therefore investigate other information, such as season, weather and year.

These insights suggest opportunities for further analysis rather than automatic operational decisions.

## Technologies Used

- Python
- Jupyter Notebook
- Visual Studio Code
- Pandas
- NumPy
- Matplotlib
- Matplotlib-Venn
- SciPy
- Scikit-learn
- Statsmodels
- Git/GitHub

## How to Run the Project

1. Clone or download the repository.
2. Open the project in Visual Studio Code or a Jupyter-compatible editor.
3. Install the required libraries.
4. Keep the supplied CSV files in the repository’s `data` folder.
5. Open the statistical, K-means, or Workshop C notebook.
6. Select the correct Python kernel.
7. Restart the kernel and run all cells from top to bottom.

### Install Dependencies

```bash
python -m pip install pandas numpy matplotlib matplotlib-venn scipy scikit-learn statsmodels jinja2
```

For standalone JupyterLab:

```bash
python -m pip install jupyterlab
jupyter lab
```

If the repository contains a requirements file:

```bash
python -m pip install -r requirements.txt
```

### Dataset Loading

**Statistical notebook**

Checks `data/day.csv` and `../data/day.csv`. If neither exists, the code downloads a copy from the team's repository. This requires internet access.

**K-means notebook**

Searches the current folder and its parent folders for `data/day.csv`. It does not download the dataset automatically.

**Workshop C notebook**

Searches the current folder and its parents for `data/day.csv` and `data/open_meteo_weather.csv`. Both files are included in the repository.

## Troubleshooting

### Missing Package

Install the package in the same Python environment selected as the notebook kernel.

### Dataset Not Found

Check that `day.csv` is inside the project's `data` folder.

### Variable Not Defined

Run the earlier cells first, or restart the kernel and run all cells in order.

## Project Limitations

- The dataset covers 2011–2012 and does not represent current demand.
- Daily observations may be related over time, affecting statistical-test assumptions.
- The traditional F-test's normality assumption is not supported by the seasonal checks.
- Historical associations do not establish causation.
- Only temperature and rentals define the clusters.
- Differences in rentals between clusters are partly expected because rentals helped form the groups.
- K-means results depend on scaling, initialization and the choice of K.
- Daily totals hide hourly peaks and station-level differences.
- The clustering model requires a day's rental total and does not directly predict future demand using the same inputs.

## Future Improvements

- Use more recent data.
- Compare patterns across years.
- Investigate weather, wind speed and customer-type differences.
- Check cluster stability with different random seeds and iteration limits.
- Compare clustering results under different scaling methods.
- Use hourly and station-level data.
- Consider time-aware statistical methods.
- Develop a separate forecasting model if future demand prediction is required.

These are proposed improvements unless their implementation is shown in the notebooks.

## Presentation

The statistical and K-means notebooks include five-minute presentation guides. For Workshop C, we present from the submitted regression notebook within the seven-minute limit. The presentations cover:

1. Research question and project context
2. Dataset and preparation
3. Methods and justification
4. Main results
5. Practical insights
6. Limitations and conclusion

All team members should understand the complete analysis and be prepared to answer questions.

## Conclusion

The statistical analysis shows higher observed average rentals in Summer than Winter, with statistical inference subject to the stated assumptions.

The K-means analysis identifies four understandable types of rental days. Their seasonal composition provides additional detail beyond a comparison of seasons alone.

In Workshop C, the combined weather and calendar model predicted historical daily rentals more accurately than either single-predictor model.

## References

Fanaee-T, H. (2013). *Bike Sharing* [Dataset]. UCI Machine Learning Repository.  
https://doi.org/10.24432/C5W894

Open-Meteo. [Historical Weather API documentation](https://open-meteo.com/en/docs/historical-weather-api)

PROG8431 course materials:

- Problem Analysis Workshop B – K-Means Clustering
- Week 7 clustering lecture and demonstration materials
- Problem Analysis Workshop C – Transformation/Linear Regression