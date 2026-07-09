# Seaborn — Complete Reference Notes

## What is Seaborn?

Seaborn is a Python data visualization library built on top of **Matplotlib**. It provides a high-level interface for drawing attractive and informative statistical graphics. It integrates closely with **pandas** DataFrames, making it one of the most popular tools for exploratory data analysis (EDA).

- **Author:** Michael Waskom
- **First Released:** 2012
- **Current Stable Version:** 0.13.x
- **License:** BSD
- **Official Docs:** https://seaborn.pydata.org

---

## Installation

```bash
pip install seaborn
```

Or with conda:
```bash
conda install seaborn
```

Dependencies automatically installed:
- `matplotlib`
- `numpy`
- `pandas`
- `scipy`
- `statsmodels`

---

## Importing Seaborn

```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
```

> **Convention:** `sns` is the standard alias (a nod to the TV character Sam Seaborn from *The West Wing*).

---

## Core Concepts

### 1. Figures vs. Axes-level Functions

Seaborn functions fall into two categories:

| Type | Description | Returns | Example |
|---|---|---|---|
| **Figure-level** | Creates its own figure with `FacetGrid` | `FacetGrid` object | `sns.relplot()`, `sns.displot()`, `sns.catplot()` |
| **Axes-level** | Draws onto an existing Matplotlib axes | `Axes` object | `sns.scatterplot()`, `sns.histplot()`, `sns.boxplot()` |

Figure-level functions support `col=` and `row=` for faceting (subplots by category).

---

### 2. The `data` Parameter

Most Seaborn functions accept a `data=` argument pointing to a **tidy (long-form) pandas DataFrame**:

```python
df = sns.load_dataset("tips")
sns.scatterplot(data=df, x="total_bill", y="tip", hue="sex")
plt.show()
```

---

## Plot Categories & Functions

### A. Relational Plots (relationships between two numeric variables)

| Function | Description |
|---|---|
| `sns.scatterplot()` | Scatter plot |
| `sns.lineplot()` | Line plot with optional confidence interval |
| `sns.relplot()` | Figure-level wrapper for scatter/line |

```python
sns.lineplot(data=df, x="size", y="total_bill", hue="day", style="day", markers=True)
```

---

### B. Distribution Plots (distribution of a single variable or pairwise)

| Function | Description |
|---|---|
| `sns.histplot()` | Histogram (with optional KDE) |
| `sns.kdeplot()` | Kernel Density Estimate |
| `sns.ecdfplot()` | Empirical Cumulative Distribution Function |
| `sns.rugplot()` | Rug plot (tick marks along axis) |
| `sns.displot()` | Figure-level wrapper for all distribution types |

```python
sns.histplot(data=df, x="total_bill", kde=True, bins=30, color="steelblue")
```

---

### C. Categorical Plots (distributions or statistics across categories)

| Function | Description |
|---|---|
| `sns.stripplot()` | Scatter of data points per category |
| `sns.swarmplot()` | Non-overlapping strip plot |
| `sns.boxplot()` | Box-and-whisker plot |
| `sns.violinplot()` | Kernel density on each side |
| `sns.boxenplot()` | Enhanced box plot for large datasets |
| `sns.pointplot()` | Point estimates with error bars |
| `sns.barplot()` | Bar chart with confidence intervals |
| `sns.countplot()` | Count of observations per category |
| `sns.catplot()` | Figure-level wrapper for all the above |

```python
sns.boxplot(data=df, x="day", y="total_bill", hue="sex", palette="Set2")
```

---

### D. Regression Plots (model fitting and visualization)

| Function | Description |
|---|---|
| `sns.regplot()` | Scatter + linear regression line |
| `sns.lmplot()` | Figure-level regplot with faceting |
| `sns.residplot()` | Residuals of a linear regression |

```python
sns.regplot(data=df, x="total_bill", y="tip", ci=95, scatter_kws={"alpha": 0.5})
```

---

### E. Matrix / Heatmap Plots

| Function | Description |
|---|---|
| `sns.heatmap()` | Color-encoded matrix |
| `sns.clustermap()` | Hierarchically clustered heatmap |

```python
corr = df.corr(numeric_only=True)
sns.heatmap(corr, annot=True, fmt=".2f", cmap="coolwarm", linewidths=0.5)
```

---

### F. Multi-plot Grids

| Function | Description |
|---|---|
| `sns.pairplot()` | Pairwise relationships in a dataset |
| `sns.PairGrid()` | Customizable pairplot grid |
| `sns.FacetGrid()` | Grid of plots conditioned on variables |
| `sns.JointGrid()` | Central + marginal plots |
| `sns.jointplot()` | Quick joint + marginal distribution |

```python
sns.pairplot(df, hue="species", diag_kind="kde")
```

```python
g = sns.jointplot(data=df, x="total_bill", y="tip", kind="reg")
```

---

## Key Parameters (Common Across Functions)

| Parameter | Purpose |
|---|---|
| `data` | Pandas DataFrame |
| `x`, `y` | Column names for axes |
| `hue` | Variable for color grouping |
| `style` | Variable for marker style |
| `size` | Variable for marker size |
| `col`, `row` | Faceting variables (figure-level only) |
| `palette` | Color palette name or dict |
| `order` | Order of categorical levels |
| `orient` | `"v"` (vertical) or `"h"` (horizontal) |
| `ax` | Matplotlib Axes to draw on |
| `ci` | Confidence interval width (deprecated → `errorbar`) |
| `errorbar` | Error representation (`"ci"`, `"sd"`, `"se"`, `"pi"`) |

---

## Themes & Aesthetics

### Setting a Theme

```python
sns.set_theme(style="whitegrid", palette="pastel", font_scale=1.2)
```

### Available Styles

| Style | Description |
|---|---|
| `darkgrid` | Dark background with grid |
| `whitegrid` | White background with grid |
| `dark` | Dark background, no grid |
| `white` | White background, no grid |
| `ticks` | White background with axis ticks |

### Color Palettes

```python
sns.set_palette("husl")         # Qualitative
sns.set_palette("Blues")        # Sequential
sns.set_palette("coolwarm")     # Diverging
sns.color_palette("tab10")      # Named palette
```

Generate custom palettes:
```python
custom = sns.color_palette(["#FF6B6B", "#4ECDC4", "#45B7D1"])
```

### Temporary Style Context

```python
with sns.axes_style("darkgrid"):
    sns.lineplot(data=df, x="x", y="y")
```

---

## Working with Matplotlib

Since Seaborn builds on Matplotlib, you can always use `plt` for fine-tuning:

```python
fig, axes = plt.subplots(1, 2, figsize=(12, 5))

sns.boxplot(data=df, x="day", y="total_bill", ax=axes[0])
axes[0].set_title("Box Plot")

sns.histplot(data=df, x="tip", ax=axes[1], bins=20)
axes[1].set_title("Tip Distribution")

plt.tight_layout()
plt.savefig("plot.png", dpi=150)
plt.show()
```

---

## Built-in Example Datasets

```python
# List all datasets
print(sns.get_dataset_names())

# Load a dataset
tips = sns.load_dataset("tips")
iris = sns.load_dataset("iris")
titanic = sns.load_dataset("titanic")
penguins = sns.load_dataset("penguins")
flights = sns.load_dataset("flights")  # Good for heatmaps
diamonds = sns.load_dataset("diamonds")
```

---

## Statistical Estimations

Seaborn automatically computes:
- **Confidence Intervals** via bootstrapping (default: 95% CI)
- **Kernel Density Estimates** for distribution plots
- **Linear Regression** in regplot / lmplot
- **Aggregations** (mean, median, etc.) in barplot / pointplot

```python
# Change aggregation function
sns.barplot(data=df, x="day", y="total_bill", estimator="median", errorbar="sd")
```

---

## FacetGrid (Custom Multi-panel Plots)

```python
g = sns.FacetGrid(df, col="time", row="sex", height=4, aspect=1.2)
g.map_dataframe(sns.scatterplot, x="total_bill", y="tip", hue="smoker")
g.add_legend()
g.set_axis_labels("Total Bill ($)", "Tip ($)")
g.set_titles("{col_name} | {row_name}")
```

---

## Object-Oriented API (Seaborn v0.12+)

Seaborn introduced a new declarative object-oriented interface:

```python
from seaborn.objects import Plot, Dot, Line, Bar

(
    Plot(df, x="total_bill", y="tip", color="sex")
    .add(Dot(alpha=0.5))
    .add(Line(), stat="smooth")
    .label(title="Tips by Total Bill", x="Total Bill ($)", y="Tip ($)")
    .show()
)
```

This new API allows method chaining and composing layers cleanly.

---

## Common Patterns & Tips

1. **Always use tidy (long-form) data** — one observation per row, one variable per column.
2. **Use `hue_order`, `order` params** to control sort order of categories.
3. **Use `despine()`** to remove top/right spines for a cleaner look:
   ```python
   sns.despine(top=True, right=True)
   ```
4. **Combine plots** by passing `ax=` from Matplotlib subplots.
5. **Save figures** using `plt.savefig()` after Seaborn calls.
6. **Use `tight_layout()`** to prevent label clipping in complex figures.

---

## Seaborn vs. Matplotlib vs. Plotly

| Feature | Seaborn | Matplotlib | Plotly |
|---|---|---|---|
| Ease of use | ✅ High | ⚠️ Medium | ✅ High |
| Statistical built-ins | ✅ Yes | ❌ No | ⚠️ Limited |
| Interactivity | ❌ Static | ❌ Static | ✅ Interactive |
| Customization | ⚠️ Medium | ✅ Full | ✅ High |
| Best for | EDA & stats | Publication plots | Dashboards/web apps |

---

## Summary

Seaborn is the go-to library for **statistical data visualization** in Python. Its tight integration with pandas, automatic statistical estimation, clean default themes, and concise syntax make it ideal for:

- Exploratory Data Analysis (EDA)
- Visualizing distributions and correlations
- Comparing groups and categories
- Communicating insights from data quickly

> **Rule of thumb:** Use Seaborn for fast, beautiful, statistically-rich plots. Drop down to Matplotlib when you need fine-grained control.
