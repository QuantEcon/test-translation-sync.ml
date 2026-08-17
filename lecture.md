---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
translation:
  title: Programming for Economics
  headings:
    Using `numpy` Arrays: Using `numpy` Arrays
    Using `numpy` Arrays::Creating Arrays with `np.arange()` and `np.linspace()`: Creating Arrays with `np.arange()` and `np.linspace()`
    Working with **Pandas DataFrames**: Working with **Pandas DataFrames**
    'Working with **Pandas DataFrames**::Reading Data: `pd.read_csv()` vs `pd.read_excel()`': 'Reading Data: `pd.read_csv()` vs `pd.read_excel()`'
    '*Matplotlib* Visualization Basics': '*Matplotlib* Visualization Basics'
    '*Matplotlib* Visualization Basics::The `plt.plot()` Function': The `plt.plot()` Function
    Using [QuantEcon](https://quantecon.org) Libraries: Using [QuantEcon](https://quantecon.org) Libraries
    Using [QuantEcon](https://quantecon.org) Libraries::Installing `quantecon` Package: Installing `quantecon` Package
    'Special Cases: $\LaTeX$ in Headings': 'Special Cases: $\LaTeX$ in Headings'
    'Special Cases: $\LaTeX$ in Headings::The $\beta$-Coefficient in Regression': The $\beta$-Coefficient in Regression
    'Special Cases: $\LaTeX$ in Headings::Computing $\mathbb{E}[X]$ (Expected Values)': Computing $\mathbb{E}[X]$ (Expected Values)
    Questions & Answers: Questions & Answers
    '"Edge Cases" and [Special] {Characters}': '"Edge Cases" and [Special] {Characters}'
    '"Edge Cases" and [Special] {Characters}::Using `matplotlib`''s `plt.subplot()` for Multiple Plots': Using `matplotlib`'s `plt.subplot()` for Multiple Plots
    Summary: Summary
---

# Programming for Economics

ഈ lecture, modern economic analysis-ന് അത്യന്താപേക്ഷിതമായ programming concepts-ഉം tools-ഉം cover ചെയ്യുന്നു.

## Using `numpy` Arrays

`numpy` library, numerical computing-നുള്ള efficient array operations നൽകുന്നു. Python-ലെ scientific computing-ന്റെ foundation ആണ് arrays.

NumPy arrays vectorized operations support ചെയ്യുന്നു, ഇത് slow Python loops ഒഴിവാക്കാൻ സഹായിക്കുന്നു:

```python
import numpy as np
arr = np.array([1, 2, 3, 4, 5])
result = arr ** 2  # Vectorized squaring
```

### Creating Arrays with `np.arange()` and `np.linspace()`

വ്യത്യസ്ത array creation methods വ്യത്യസ്ത purposes-നു വേണ്ടിയാണ്. `np.arange()` function specified step sizes ഉള്ള arrays create ചെയ്യുന്നു:

```python
x = np.arange(0, 10, 0.5)  # From 0 to 10, step 0.5
```

അതേസമയം, `np.linspace()` specified number of points ഉള്ള arrays create ചെയ്യുന്നു:

```python
y = np.linspace(0, 1, 100)  # 100 points from 0 to 1
```

## Working with **Pandas DataFrames**

**pandas** library ശക്തമായ data manipulation capabilities നൽകുന്നു. DataFrames എന്നത് two-dimensional labeled data structures ആണ്.

### Reading Data: `pd.read_csv()` vs `pd.read_excel()`

വ്യത്യസ്ത file formats-ന് വ്യത്യസ്ത reading functions ആവശ്യമായിവരുന്നു:

- **CSV files**: `pd.read_csv('data.csv')` ഉപയോഗിക്കുക
- **Excel files**: `pd.read_excel('data.xlsx')` ഉപയോഗിക്കുക
- **JSON files**: `pd.read_json('data.json')` ഉപയോഗിക്കുക

## *Matplotlib* Visualization Basics

*Matplotlib*, Python-ന്റെ foundational plotting library ആണ്. ഇത് plot appearance-ന് മേൽ fine-grained control നൽകുന്നു.

### The `plt.plot()` Function

Basic line plots, `plt.plot(x, y)` ഉപയോഗിച്ച് create ചെയ്യാം:

```python
import matplotlib.pyplot as plt
plt.plot([1, 2, 3], [4, 5, 6])
plt.xlabel('x-axis')
plt.ylabel('y-axis')
plt.title('Simple Plot')
plt.show()
```

## Using [QuantEcon](https://quantecon.org) Libraries

[QuantEcon](https://quantecon.org) project economic modeling-നു വേണ്ടിയുള്ള specialized tools നൽകുന്നു. വിശദാംശങ്ങൾക്കായി [documentation](https://quanteconpy.readthedocs.io/) സന്ദർശിക്കുക.

### Installing `quantecon` Package

pip വഴി install ചെയ്യുക:

```bash
pip install quantecon
```

അല്ലെങ്കിൽ conda ഉപയോഗിച്ച്:

```bash
conda install -c conda-forge quantecon
```

## Special Cases: $\LaTeX$ in Headings

Mathematical notation ഇൻലൈൻ ആയി കാണാം: $f(x) = x^2 + 2x + 1$.

### The $\beta$-Coefficient in Regression

regression coefficient $\beta$, variables-കൾ തമ്മിലുള്ള ബന്ധം (relationship) അളക്കുന്നു:

$$
y = \alpha + \beta x + \epsilon
$$

### Computing $\mathbb{E}[X]$ (Expected Values)

Expected values $\mathbb{E}[X]$ ശരാശരി outcomes-നെ പ്രതിനിധീകരിക്കുന്നു:

$$
\mathbb{E}[X] = \sum_{i} x_i P(X = x_i)
$$

## Questions & Answers

Economics-നുള്ള Python-നെക്കുറിച്ചുള്ള common questions:

- **Q**: Which is faster: `numpy` or plain Python?
- **A**: Numerical operations-ന് `numpy` സാധാരണയായി 10-100x വേഗതയുള്ളതാണ്

- **Q**: Can I use `pandas` with large datasets?
- **A**: Yes, but RAM-നേക്കാൾ വലിയ datasets-ന് `dask` പരിഗണിക്കുക

## "Edge Cases" and [Special] {Characters}

ഈ section headings-ലെ വിവിധ special characters-നെ test ചെയ്യുന്നു.

### Using `matplotlib`'s `plt.subplot()` for Multiple Plots

subplot function ഒരു figure-ൽ multiple plots create ചെയ്യുന്നു:

```python
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))
ax1.plot(x, y1)
ax2.plot(x, y2)
```

## Summary

**numpy**, *pandas*, `matplotlib` എന്നിവ പോലുള്ള programming tools ആധുനിക economic research-ന് അത്യാവശ്യമാണ്. data efficiently ആയി analyze ചെയ്യാനും results visualize ചെയ്യാനും ഈ libraries-നെ master ചെയ്യുക.
