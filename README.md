# Bike Sharing Demand Analysis

## PROG8431 – Data Analysis, Mathematics, Modeling and Algorithms

**Group 7 – Data Analysis**

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
