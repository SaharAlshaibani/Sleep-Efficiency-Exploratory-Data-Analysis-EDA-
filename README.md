## Project Overview

- **Objective:**
    
    To analyze and understand the patterns and relationships that influence **sleep efficiency**.
    
- **Focus Areas:**
    - Cleaning missing values and handling outliers.
    - Exploring correlations between sleep efficiency and other factors (age, exercise, caffeine, alcohol, etc).
    - Visualizing patterns, trends, and relationships using Python libraries.

**Dataset Source:** [Kaggle — Sleep Efficiency Dataset](https://www.kaggle.com/datasets/equilibriumm/sleep-efficiency)

---

## Dataset Information

- **Rows:** 452
- **Columns:** 15
- **Key Features:**
    - `sleep_duration`, `sleep_efficiency`, `deep_sleep_percentage`, `light_sleep_percentage`,
        
        `rem_sleep_percentage`, `awakenings`, `alcohol_consumption`,
        
        `caffeine_consumption`, `exercise_frequency`, `age`, etc.
        

### Missing Values:

| Column | Missing | Treatment |
| --- | --- | --- |
| Caffeine Consumption | 25 | Filled with most frequent value |
| Alcohol Consumption | 14 | Filled with most frequent value |
| Awakenings | 20 | Filled with median |
| Exercise Frequency | 6 | Filled with median |

**Rationale:**

Mode used for categorical habits (like alcohol/caffeine), and median used for numeric, skewed data such as awakenings or exercise frequency.

## Data Cleaning & Outlier Detection

### Outlier Detection (IQR Method)

```python
q1 = df[col].quantile(0.25)
q3 = df[col].quantile(0.75)
iqr = q3 - q1
lower = q1 - 1.5 * iqr
upper = q3 + 1.5 * iqr
outliers = df[(df[col] < lower) | (df[col] > upper)]

```

| Column | Outliers | % | Recommendation |
| --- | --- | --- | --- |
| sleep_duration | 16 | 3.5% | Review |
| deep_sleep_percentage | 60 | 13.3% | Keep (natural variation) |
| light_sleep_percentage | 2 | 0.4% | Keep |
| caffeine_consumption | 4 | 0.9% | Remove |
| awakenings, alcohol, exercise | 0 | 0% | Clean |

**Interpretation:**

Outliers were mostly natural variations (especially deep sleep %). Mild ones (like caffeine) were trimmed for better visualization.

---

## Correlation Analysis

We computed **Pearson correlations** between all numeric features to identify the main influencers of **sleep efficiency**.

```python
corr = df.corr(numeric_only=True)
plt.figure(figsize=(10,8))
sns.heatmap(corr, annot=True, cmap='coolwarm', fmt=".2f")
plt.title("Correlation Heatmap (Numeric Features)")
plt.show()

```

### Key Findings:

| Feature | Correlation (r) | Interpretation |
| --- | --- | --- |
| `deep_sleep_percentage` | **+0.79** | Strong positive — higher deep sleep = better efficiency |
| `light_sleep_percentage` | **–0.82** | Strong negative — more light sleep lowers efficiency |
| `awakenings` | **–0.55** | Strong negative — frequent awakenings reduce quality |
| `alcohol_consumption` | **–0.37** | Moderate negative — alcohol disturbs deep sleep |
| `exercise_frequency` | **+0.27** | Moderate positive — exercise improves sleep |
| `caffeine_consumption` | **+0.04** | No clear impact |
| `age` | **+0.09** | Minimal effect |

---

## Key Visualizations

### Correlation Heatmap

Shows relationships among all numeric variables.

---

### Deep Sleep Percentage vs Sleep Efficiency

Strong positive relationship — higher deep sleep leads to better sleep efficiency.

---

### Light Sleep Percentage vs Sleep Efficiency

Strong negative relationship — excessive light sleep reduces overall efficiency.

---

### Awakenings vs Sleep Efficiency

More awakenings significantly decrease efficiency.

---

### Alcohol Consumption vs Sleep Efficiency

Alcohol consumption disrupts deep and REM cycles, reducing efficiency.

---

## Insights & Takeaways

**To improve sleep efficiency:**

- Increase deep sleep duration (through consistent bedtime and calm environment).
- Limit light sleep by avoiding alcohol or screens before bed.
- Maintain regular exercise (moderate intensity).
- Reduce nighttime awakenings (avoid caffeine late in the day).

**Technical Summary:**

- Libraries used: `pandas`, `numpy`, `matplotlib`, `seaborn`
- Environment: Jupyter Notebook
- Dataset size: 452 × 15
- Outliers handled using IQR
- Missing data filled with median/mode

