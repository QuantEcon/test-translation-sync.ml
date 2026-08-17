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
  title: Linear Algebra Foundations
  headings:
    Vector Spaces: Vector Spaces
    Vector Spaces::Basic Properties: Basic Properties
    Vector Spaces::Basic Properties::Applications in Economics: Applications in Economics
    Matrix Operations: Matrix Operations
    Matrix Operations::Eigenvalues and Eigenvectors: Eigenvalues and Eigenvectors
---

# Linear Algebra Foundations

ഈ lecture, quantitative economics-ഇനും data science-ഇനും അത്യന്താപേക്ഷിതമായ linear algebra-യിലെ അടിസ്ഥാന ആശയങ്ങൾ പരിചയപ്പെടുത്തുന്നു. നമ്മൾ vector spaces, matrices, eigenvalues, അവയുടെ economic പ്രശ്നങ്ങളിലുള്ള applications എന്നിവ പരിശോധിക്കും.

## Vector Spaces

vector space എന്നത് vectors എന്ന് വിളിക്കപ്പെടുന്ന objects-ന്റെ ഒരു collection ആണ്, ഇവ പരസ്പരം add ചെയ്യാനും scalars കൊണ്ട് multiply ചെയ്യാനും കഴിയും. modern economic analysis-ഇനും machine learning applications-ഇനും vector spaces മനസ്സിലാക്കുന്നത് അത്യന്താപേക്ഷിതമാണ്.

Mathematically, ഒരു vector $\mathbf{v} \in \mathbb{R}^n$ ഇങ്ങനെ represent ചെയ്യാം:

$$
\mathbf{v} = \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix}
$$

നമുക്ക് Python-ൽ ചില vectors സൃഷ്ടിച്ച് visualize ചെയ്യാം:

```{code-cell} python
import numpy as np
import matplotlib.pyplot as plt

# രണ്ട് vectors സൃഷ്ടിക്കുക
v1 = np.array([2, 3])
v2 = np.array([1, 4])

# Vectors visualize ചെയ്യുക
fig, ax = plt.subplots(figsize=(8, 6))
ax.quiver(0, 0, v1[0], v1[1], angles='xy', scale_units='xy', scale=1, color='blue', label='v1')
ax.quiver(0, 0, v2[0], v2[1], angles='xy', scale_units='xy', scale=1, color='red', label='v2')
ax.set_xlim(-1, 5)
ax.set_ylim(-1, 5)
ax.set_xlabel('x-axis')
ax.set_ylabel('y-axis')
ax.set_title('2D Space-ലെ Vector Representation')
ax.legend()
ax.grid(True)
plt.show()
```

### Basic Properties

vector spaces നിരവധി പ്രധാന properties satisfy ചെയ്യുന്നു:
- addition, scalar multiplication എന്നിവയ്ക്ക് കീഴിലുള്ള closure
- additive identity-യുടെ (zero vector) നിലനിൽപ്പ്
- additive inverses-ന്റെ നിലനിൽപ്പ്
- addition-ന്റെ associativity-ഉം commutativity-ഉം

ഈ properties, mathematical operations-ന് കീഴിൽ vector spaces പ്രവചനീയമായി പെരുമാറുന്നു എന്ന് ഉറപ്പാക്കുന്നു, കൂടാതെ linear transformations-ന്റെ അടിസ്ഥാനവും ഇവ രൂപപ്പെടുത്തുന്നു.

#### Applications in Economics

economic modeling-ൽ vector space properties അടിസ്ഥാനപരമാണ്. closure property, feasible allocations-ന്റെ combinations feasible ആയി തുടരുന്നു എന്ന് ഉറപ്പാക്കുന്നു, inverses-ന്റെ നിലനിൽപ്പ് debts, obligations എന്നിവ model ചെയ്യാൻ നമ്മെ അനുവദിക്കുന്നു.

രണ്ട് vectors $\mathbf{u}$, $\mathbf{v}$ എന്നിവയുടെ sum component-wise ആയി define ചെയ്യുന്നു:

```{math}
\mathbf{u} + \mathbf{v} = \begin{bmatrix} u_1 + v_1 \\ u_2 + v_2 \\ \vdots \\ u_n + v_n \end{bmatrix}
```

## Matrix Operations

Matrices എന്നത് linear transformations represent ചെയ്യുന്ന numbers-ന്റെ rectangular arrays ആണ്. economic modeling, data analysis, optimization problems എന്നിവയിൽ ഇവ അടിസ്ഥാന tools ആണ്.

ഒരു general $m \times n$ matrix-ന്റെ രൂപം ഇങ്ങനെയാണ്:

$$
A = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{bmatrix}
$$

### Eigenvalues and Eigenvectors

Eigenvalues, eigenvectors എന്നിവ linear transformations-ന്റെ പ്രധാനപ്പെട്ട structural properties വെളിപ്പെടുത്തുന്നു. ഒരു square matrix $A$-ക്ക്, ഒരു eigenvector $\mathbf{v}$-ഉം അതിനോട് അനുബന്ധിച്ചുള്ള eigenvalue $\lambda$-ഉം ഇത് തൃപ്തിപ്പെടുത്തുന്നു:

$$
A\mathbf{v} = \lambda \mathbf{v}
$$

dynamic systems, stability analysis, PCA പോലുള്ള dimensionality reduction techniques എന്നിവ മനസ്സിലാക്കാൻ ഈ concepts നിർണായകമാണ്.
