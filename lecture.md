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
    Eigenvalues and Eigenvectors: Eigenvalues and Eigenvectors
---

# Linear Algebra Foundations

ഈ lecture, quantitative economics-ന് അത്യന്താപേക്ഷിതമായ linear algebra-യിലെ അടിസ്ഥാന ആശയങ്ങൾ പരിചയപ്പെടുത്തുന്നു. നമ്മൾ vector spaces, matrices, അവയുടെ economic പ്രശ്നങ്ങളിലുള്ള applications എന്നിവ പരിശോധിക്കും.

## Vector Spaces

vector space എന്നത് vectors എന്ന് വിളിക്കപ്പെടുന്ന objects-ന്റെ ഒരു collection ആണ്, ഇവ പരസ്പരം add ചെയ്യാനും scalars കൊണ്ട് multiply ചെയ്യാനും കഴിയും. modern economic analysis-ന് vector spaces മനസ്സിലാക്കുന്നത് അത്യന്താപേക്ഷിതമാണ്.

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

ഈ properties, mathematical operations-ന് കീഴിൽ vector spaces പ്രവചനീയമായി പെരുമാറുന്നു എന്ന് ഉറപ്പാക്കുന്നു.

#### Applications in Economics

economic modeling-ൽ vector space properties അടിസ്ഥാനപരമാണ്. closure property, feasible allocations-ന്റെ combinations feasible ആയി തുടരുന്നു എന്ന് ഉറപ്പാക്കുന്നു, inverses-ന്റെ നിലനിൽപ്പ് debts, obligations എന്നിവ model ചെയ്യാൻ നമ്മെ അനുവദിക്കുന്നു.

രണ്ട് vectors $\mathbf{u}$, $\mathbf{v}$ എന്നിവയുടെ sum component-wise ആയി define ചെയ്യുന്നു:

```{math}
\mathbf{u} + \mathbf{v} = \begin{bmatrix} u_1 + v_1 \\ u_2 + v_2 \\ \vdots \\ u_n + v_n \end{bmatrix}
```

## Eigenvalues and Eigenvectors

Eigenvalues, eigenvectors എന്നിവ linear transformations-ന്റെ പ്രധാന properties വെളിപ്പെടുത്തുന്നു. matrix $A$-യുടെ ഒരു eigenvector $v$ ഇത് satisfy ചെയ്യുന്നു:

```{math}
:label: eigenvalue-equation
Av = \lambda v
```

ഇവിടെ $\lambda$ എന്നത് eigenvalue ആണ്. growth theory മുതൽ stability analysis വരെ, economics-ൽ ഉടനീളം ഈ fundamental equation കാണപ്പെടുന്നു.

ഒരു $n \times n$ matrix $A$-ക്ക്, characteristic polynomial ഇതാണ്:

$$
\det(A - \lambda I) = 0
$$

ഈ equation പരിഹരിക്കുന്നത് eigenvalues നൽകുന്നു. നമുക്ക് ഒരു transition matrix-ന്റെ eigenvalues കണക്കാക്കാം:

```{code-cell} python
# ഒരു simple Markov chain-ന് വേണ്ടി ഒരു transition matrix സൃഷ്ടിക്കുക
# States: Employed, Unemployed
transition_matrix = np.array([
    [0.95, 0.05],  # Employed -> (Employed, Unemployed)
    [0.20, 0.80]   # Unemployed -> (Employed, Unemployed)
])

# Eigenvalues, eigenvectors കണക്കാക്കുക
eigenvalues, eigenvectors = np.linalg.eig(transition_matrix)

print("Transition Matrix:")
print(transition_matrix)
print("\nEigenvalues:")
print(np.round(eigenvalues, 4))
print("\nEigenvectors:")
print(np.round(eigenvectors, 4))

# eigenvalue 1-നോട് അനുയോജ്യമായ eigenvector, steady-state distribution നൽകുന്നു
steady_state_index = np.argmax(eigenvalues)
steady_state = eigenvectors[:, steady_state_index]
steady_state = steady_state / steady_state.sum()  # Normalize ചെയ്യുക

print("\nSteady-State Distribution:")
print(f"Employed: {steady_state[0]:.2%}")
print(f"Unemployed: {steady_state[1]:.2%}")
```

growth models, stability analysis പോലുള്ള dynamic economic systems analyze ചെയ്യാൻ ഈ concepts അത്യന്താപേക്ഷിതമാണ്.

dominant eigenvalue കണ്ടെത്താൻ power iteration method ഉപയോഗിക്കാം:

$$
\lambda_1 = \lim_{k \to \infty} \frac{\|A^k \mathbf{v}_0\|}{\|A^{k-1} \mathbf{v}_0\|}
$$
