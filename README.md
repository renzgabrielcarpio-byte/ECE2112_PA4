# ECE2112_PA4
## EXPERIMENT 4: DATA WRANGLING AND DATA VISUALIZATION
CARPIO, Renz Gabriel P.  |  2ECE-A


I. OBJECTIVES
At the end of this laboratory activity, the student should be able to:
1. Filter tabular data using several categorical and numerical conditions;
2. Construct focused DataFrames by selecting relevant features;
3. Summarize the relationship between categorical features and a numerical variable; and
4. Communicate a data comparison using clear and correctly labeled plots.

II. INSTRUCTIONS

Use the same ECE Board Exam 2 dataset supplied for Experiment 4. Work in a Jupyter Notebook
using Pandas and a Python plotting library used in class. Use the dataset’s existing column labels,
including Name, Gender, Track, Hometown, Math, GEAS, Electronics, and Average.

• Derive all tables and plot values from the dataset. Do not manually type rows, category means, or
plotted values.

• When applying more than one condition, make every condition explicit in the filtering expression.

• Keep the original DataFrame unchanged.

• Every graph must have a title, axis labels, readable category labels, and a consistent scale
appropriate to the data.

# **A. Visayas Communication DataFrame**

Create a DataFrame named `VisComm` containing students whose `Hometown` is "Visayas" and whose `Track` is "Communication". Retain only the columns `Name`, `Gender`, `Math`, `Electronics`, and `Average` in the stated order, and display the resulting DataFrame along with its row count.

The following functions and methods were used in this problem:

• Bitwise AND Operator (`&`) - combines multiple Boolean conditions element-wise, ensuring that both categorical criteria are satisfied simultaneously.

Example: `(df['Hometown'] == 'Visayas') & (df['Track'] == 'Communication')`

• Column Subsetting (`[['Name', 'Gender', ...]]`) - selects and rearranges the specified columns in the designated order after filtering.

• `len()` / `.shape[0]` - used to determine and display the total number of rows matching the criteria.

Combining these operations, the final implementation for this problem is as follows:

```python
import pandas as pd

# Load dataset
df = pd.read_csv("board2.csv")

# Filter Hometown and Track, then select required columns
vis_comm_filter = (df["Hometown"] == "Visayas") & (df["Track"] == "Communication")
VisComm = df[vis_comm_filter][["Name", "Gender", "Math", "Electronics", "Average"]]

# Required checks
print("VisComm DataFrame:")
print(VisComm)
print("\nNumber of rows:", len(VisComm))
```

# **B. Visayas Female DataFrame**

Create a DataFrame named `VisFemale` containing students whose `Hometown` is "Visayas" and whose `Gender` is "Female", retaining only `Name`, `Track`, `GEAS`, `Electronics`, and `Average`. Display `VisFemale`, then display only the subset of rows whose `Average` is at least 60 without overwriting `VisFemale`.   

The following functions and methods were used in this problem:

• Multi-Condition Boolean Filtering - isolates records where `Hometown == 'Visayas'` and `Gender == 'Female'` before extracting the target column subset.  

• Numerical Comparison (`>= 60`) - applies a numerical threshold filter on the `Average` column to evaluate which students scored 60 or higher.   

• Non-Destructive Filtering - evaluates and displays the filtered view directly without reassigning or modifying the underlying `VisFemale` DataFrame.   

Combining these methods, the final implementation for this problem is as follows:

```python
import pandas as pd

# Filter Hometown and Gender, then select required columns
vis_fem_filter = (df["Hometown"] == "Visayas") & (df["Gender"] == "Female")
VisFemale = df[vis_fem_filter][["Name", "Track", "GEAS", "Electronics", "Average"]]

# Display complete VisFemale DataFrame
print("VisFemale DataFrame:")
print(VisFemale)

# Display rows where Average is at least 60 without overwriting VisFemale
vis_female_passed = VisFemale[VisFemale["Average"] >= 60]
print("\nVisFemale with Average >= 60:")
print(vis_female_passed)
```

# **C.   Category-Average Visualization**
Examine how the recorded `Average` differs across the three categorical features `Track`, `Gender`, and `Hometown` [cite: 4]. Compute the mean of `Average` for each category, display the summary tables, generate a single figure containing three bar charts, and identify the highest category mean for each feature[cite: 4].

The following functions and methods were used in this problem:

• `.groupby()` & `.mean()` - groups the dataset by unique values in a categorical feature and computes the arithmetic mean of the numerical `Average` column[cite: 4].

Example: `df.groupby('Track')['Average'].mean()`

[cite: 4]

• `plt.subplots(1, 3, ...)` - initializes a single figure layout containing three side-by-side subplots to display bar charts for each categorical variable[cite: 4].

• `.plot(kind='bar', ...)` - renders a bar graph for each summary series with appropriate titles, axis labels, and custom styling[cite: 4].

• `.idxmax()` - extracts the category label holding the highest sample mean value for interpretation[cite: 4].

Combining these techniques, the final implementation for this problem is as follows:

```python
import pandas as pd
import matplotlib.pyplot as plt

# a. Compute category means for Average
mean_track = df.groupby("Track")["Average"].mean()
mean_gender = df.groupby("Gender")["Average"].mean()
mean_hometown = df.groupby("Hometown")["Average"].mean()

# b. Display summary tables
print("Mean Average by Track:\n", mean_track, "\n")
print("Mean Average by Gender:\n", mean_gender, "\n")
print("Mean Average by Hometown:\n", mean_hometown, "\n")

# c. Create one figure containing three bar charts
fig, axes = plt.subplots(1, 3, figsize=(18, 5))

mean_track.plot(kind="bar", ax=axes[0], color="skyblue", edgecolor="black")
axes[0].set_title("Mean Average by Track")
axes[0].set_xlabel("Track")
axes[0].set_ylabel("Mean Average Score")
axes[0].tick_params(axis="x", rotation=0)

mean_gender.plot(kind="bar", ax=axes[1], color="salmon", edgecolor="black")
axes[1].set_title("Mean Average by Gender")
axes[1].set_xlabel("Gender")
axes[1].set_ylabel("Mean Average Score")
axes[1].tick_params(axis="x", rotation=0)

mean_hometown.plot(kind="bar", ax=axes[2], color="mediumseagreen", edgecolor="black")
axes[2].set_title("Mean Average by Hometown")
axes[2].set_xlabel("Hometown")
axes[2].set_ylabel("Mean Average Score")
axes[2].tick_params(axis="x", rotation=0)

plt.tight_layout()
plt.show()

# d. Statements identifying the highest sample mean for each feature
print(f"1. Track with highest sample mean: {mean_track.idxmax()} ({mean_track.max():.2f})")
print(f"2. Gender with highest sample mean: {mean_gender.idxmax()} ({mean_gender.max():.2f})")
print(f"3. Hometown with highest sample mean: {mean_hometown.idxmax()} ({mean_hometown.max():.2f})")
```
