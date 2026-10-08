# Bike Sharing Demand Analysis

## PROG8431 – Data Analysis, Mathematics, Modeling and Algorithms

## Group 7 – Data Analysis

### Team Members

* Sukhchain Singh – 9111541
* Preethi – 9125985
* Arya – 9084843

## Project Overview

Bike rental demand can change depending on different conditions. This project analyzes historical Capital Bikeshare data to understand how **temperature and season are associated with daily bike rental demand**.

## Research Question

**How are temperature and season associated with daily Capital Bikeshare rental demand?**

## Dataset

We use the **Bike Sharing Dataset** from the UCI Machine Learning Repository.

**Source:** Fanaee-T, H. (2013). *Bike Sharing* [Dataset]. UCI Machine Learning Repository.

The `day.csv` dataset contains **731 daily records from 2011 to 2012**.

Important variables used in our analysis include:

* `cnt` – total daily bike rentals
* `temp` – normalized temperature
* `hum` – normalized humidity
* `season` – season of the year
* `casual` – rentals by casual users
* `registered` – rentals by registered users

## Data Checks and Cleansing

Before performing the analysis, we:

* Check for missing values.
* Check for duplicate dates and rows.
* Verify valid season codes.
* Verify that total rentals equal casual rentals + registered rentals.
* Convert temperature to Celsius.
* Convert humidity to percentage.
* Convert season codes to readable season names.

## Statistical Analysis

We calculate descriptive statistics including:

* Mean
* Median
* Mode
* Variance
* Standard deviation
* Quartiles (Q1–Q4)

We also compare bike rental demand across different seasons.

## Statistical Tests

We performed statistical tests to understand daily bike rental patterns.

### 1. Normality Test (Shapiro-Wilk)

We used a QQ-plot and the Shapiro-Wilk test to check whether daily bike rentals follow a normal distribution.

The overall p-value was below 0.05, showing evidence of non-normality.

We also tested Winter and Summer separately. Both groups showed evidence of non-normality.

### 2. F-Test

We used the F-test to compare the variances of daily bike rentals in Winter and Summer.

* F-statistic: 1.09
* P-value: 0.572

We did not find enough evidence that the two variances were different.

However, the data did not satisfy the normality assumption of the traditional F-test.

### 3. Levene's Test

We performed Levene's test as an additional variance comparison because it is less sensitive to non-normal data.

* Test statistic: 2.3327
* P-value: 0.127543

We did not find enough evidence that Winter and Summer variances were different.

### 4. Welch's T-Test

We used Welch's t-test to compare average daily bike rentals between Winter and Summer.

* Winter average: 2,604 rentals
* Summer average: 5,644 rentals
* T-score: 20.42
* P-value: Below 0.001

The results showed a statistically significant difference between the seasonal averages under the test assumptions.

### Limitations

Our dataset contains historical observations from 2011 to 2012.

Daily observations may be related over time, which can affect statistical test results.

Our findings show historical associations, not proof of causation.

## Visualizations

The project includes four visualizations:

1. **Scatter Plot** – compares temperature and daily rentals.
2. **Histogram** – shows the distribution of daily rental totals.
3. **Box Plot** – compares rental demand across seasons.
4. **Venn Diagram** – shows the overlap between warm days (above 20°C) and high-demand days (more than 5,000 rentals).

## Technologies Used

* Python
* Jupyter Notebook
* Visual Studio Code
* Pandas
* NumPy
* Matplotlib
* Matplotlib-Venn
* Git/GitHub
* SciPy – Statistical testing and analysis

## How to Run the Project

1. Clone or download the project repository.
2. Open the project in Visual Studio Code.
3. Make sure Python and the required libraries are installed.
4. Make sure `day.csv` is available in the project's `data` folder.
5. Open the Jupyter Notebook.
6. Select the correct Python kernel.
7. Run all notebook cells from top to bottom.

## Project Limitation

The dataset contains historical records from 2011–2012. Therefore, the analysis demonstrates historical relationships between temperature, season, and rental demand and should not be interpreted as representing current Capital Bikeshare demand.

## Reference

Fanaee-T, H. (2013). *Bike Sharing* [Dataset]. UCI Machine Learning Repository.
