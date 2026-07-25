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
    Vector spaces: Vector spaces
    Vector spaces::Basic properties: Basic properties
    Vector spaces::Basic properties::Applications in economics: Applications in economics
    Matrix operations: Matrix operations
    Matrix operations::Applications in economics: Applications in economics
    Eigenvalues and eigenvectors: Eigenvalues and eigenvectors
---

# Linear Algebra Foundations

ക്വാണ്ടിറ്റേറ്റീവ് economics-ന് അത്യന്താപേക്ഷിതമായ linear algebra-യിലെ അടിസ്ഥാന concepts ഈ lecture പരിചയപ്പെടുത്തുന്നു. നമ്മൾ vector spaces, matrices, അവ economic പ്രശ്നങ്ങളിൽ എങ്ങനെ പ്രയോഗിക്കാം എന്നിവ പര്യവേക്ഷണം ചെയ്യും.

## Vector spaces

Vector space എന്നത് vectors എന്ന് വിളിക്കപ്പെടുന്ന objects-ന്റെ ഒരു ശേഖരമാണ്, ഇവ പരസ്പരം കൂട്ടിച്ചേർക്കാനും scalars കൊണ്ട് ഗുണിക്കാനും കഴിയും. modern economic analysis-ന് vector spaces മനസ്സിലാക്കുന്നത് നിർണായകമാണ്.

Mathematically, ഒരു vector $\mathbf{v} \in \mathbb{R}^n$ ഇങ്ങനെ represent ചെയ്യാം:

$$
\mathbf{v} = \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix}
$$

നമുക്ക് Python-ൽ ചില vectors സൃഷ്ടിച്ച് visualize ചെയ്യാം:

```{code-cell} python
import numpy as np
import matplotlib.pyplot as plt

# Create two vectors
v1 = np.array([2, 3])
v2 = np.array([1, 4])

# Visualize vectors
fig, ax = plt.subplots(figsize=(8, 6))
ax.quiver(0, 0, v1[0], v1[1], angles='xy', scale_units='xy', scale=1, color='blue', label='v1')
ax.quiver(0, 0, v2[0], v2[1], angles='xy', scale_units='xy', scale=1, color='red', label='v2')
ax.set_xlim(-1, 5)
ax.set_ylim(-1, 5)
ax.set_xlabel('x-axis')
ax.set_ylabel('y-axis')
ax.set_title('Vector Representation in 2D Space')
ax.legend()
ax.grid(True)
plt.show()
```

### Basic properties

Vector spaces പല key properties-യും തൃപ്തിപ്പെടുത്തുന്നു:
- Addition, scalar multiplication എന്നിവയിൽ closure
- Additive identity-യുടെ (zero vector) നിലനിൽപ്പ്
- Additive inverses-ന്റെ നിലനിൽപ്പ്

ഈ properties, mathematical operations-ന് കീഴിൽ vector spaces പ്രവചനാത്മകമായി പെരുമാറുന്നു എന്ന് ഉറപ്പാക്കുന്നു.

#### Applications in economics

Economic modeling-ൽ vector space properties അടിസ്ഥാനപരമാണ്. Closure property, feasible allocations-ന്റെ combinations feasible ആയി തുടരും എന്ന് ഉറപ്പാക്കുന്നു, അതേസമയം inverses-ന്റെ നിലനിൽപ്പ് debts, obligations എന്നിവ model ചെയ്യാൻ നമ്മെ അനുവദിക്കുന്നു.

രണ്ട് vectors $\mathbf{u}$, $\mathbf{v}$ എന്നിവയുടെ sum component-wise ആയി define ചെയ്യപ്പെടുന്നു:

```{math}
\mathbf{u} + \mathbf{v} = \begin{bmatrix} u_1 + v_1 \\ u_2 + v_2 \\ \vdots \\ u_n + v_n \end{bmatrix}
```

## Matrix operations

Matrices എന്നത് linear transformations-നെ represent ചെയ്യുന്ന numbers-ന്റെ rectangular arrays ആണ്. Economic modeling, data analysis എന്നിവയിൽ ഇവ അടിസ്ഥാന tools ആണ്.

ഒരു general $m \times n$ matrix-ന്റെ രൂപം ഇതാണ്:

$$
A = \begin{bmatrix}
a_{11} & a_{12} & \cdots & a_{1n} \\
a_{21} & a_{22} & \cdots & a_{2n} \\
\vdots & \vdots & \ddots & \vdots \\
a_{m1} & a_{m2} & \cdots & a_{mn}
\end{bmatrix}
$$

Matrix multiplication, linear transformations compose ചെയ്യാൻ നമ്മെ അനുവദിക്കുന്നു. $A$, $B$ എന്നീ matrices-ന്, product $AB$ transformation $B$ apply ചെയ്ത ശേഷം transformation $A$ apply ചെയ്യുന്നതിനെ represent ചെയ്യുന്നു.

ഒരു economic application ഉപയോഗിച്ച് matrix operations നമുക്ക് കാണിക്കാം:

```{code-cell} python
# Create a simple input-output matrix for a 3-sector economy
# Sectors: Agriculture, Manufacturing, Services
input_output = np.array([
    [0.2, 0.3, 0.1],  # Agriculture inputs
    [0.3, 0.2, 0.2],  # Manufacturing inputs
    [0.1, 0.2, 0.3]   # Services inputs
])

# Final demand vector (in billions)
final_demand = np.array([100, 150, 200])

# Calculate total output using Leontief inverse: x = (I - A)^{-1} * d
I = np.eye(3)
leontief_inverse = np.linalg.inv(I - input_output)
total_output = leontief_inverse @ final_demand

print("Input-Output Matrix:")
print(input_output)
print("\nLeontief Inverse:")
print(np.round(leontief_inverse, 3))
print("\nTotal Output Required (billions):")
print(np.round(total_output, 2))
```

### Applications in economics

Economic models പലപ്പോഴും ഇവയെ represent ചെയ്യാൻ matrices ഉപയോഗിക്കുന്നു:
- Production-ലെ input-output relationships
- Markov chains-ലെ transition probabilities
- Linear equation systems-ലെ coefficient matrices

Leontief inverse $(I - A)^{-1}$ പ്രത്യേകിച്ച് പ്രധാനമാണ്, ഇവിടെ $I$ identity matrix ആണ്, $A$ input-output coefficient matrix ആണ്.

## Eigenvalues and eigenvectors

Eigenvalues, eigenvectors എന്നിവ linear transformations-ന്റെ പ്രധാന properties വെളിപ്പെടുത്തുന്നു. Matrix $A$-യുടെ ഒരു eigenvector $v$ ഇത് തൃപ്തിപ്പെടുത്തുന്നു:

```{math}
:label: eigenvalue-equation
Av = \lambda v
```

ഇവിടെ $\lambda$ eigenvalue ആണ്. ഈ അടിസ്ഥാന equation growth theory മുതൽ dynamic systems-ന്റെ stability analysis വരെ economics-ൽ എല്ലായിടത്തും കാണപ്പെടുന്നു.

ഒരു $n \times n$ matrix $A$-ന്, characteristic polynomial ഇതാണ്:

$$
\det(A - \lambda I) = 0
$$

ഈ equation solve ചെയ്യുന്നത് eigenvalues നൽകുന്നു. നമുക്ക് ഒരു transition matrix-ന് eigenvalues compute ചെയ്യാം:

```{code-cell} python
# Create a transition matrix for a simple Markov chain
# States: Employed, Unemployed
transition_matrix = np.array([
    [0.95, 0.05],  # Employed -> (Employed, Unemployed)
    [0.20, 0.80]   # Unemployed -> (Employed, Unemployed)
])

# Calculate eigenvalues and eigenvectors
eigenvalues, eigenvectors = np.linalg.eig(transition_matrix)

print("Transition Matrix:")
print(transition_matrix)
print("\nEigenvalues:")
print(np.round(eigenvalues, 4))
print("\nEigenvectors:")
print(np.round(eigenvectors, 4))

# The eigenvector corresponding to eigenvalue 1 gives steady-state distribution
steady_state_index = np.argmax(eigenvalues)
steady_state = eigenvectors[:, steady_state_index]
steady_state = steady_state / steady_state.sum()  # Normalize

print("\nSteady-State Distribution:")
print(f"Employed: {steady_state[0]:.2%}")
print(f"Unemployed: {steady_state[1]:.2%}")
```

growth models, stability analysis പോലുള്ള dynamic economic systems analyze ചെയ്യാൻ ഈ concepts അത്യന്താപേക്ഷിതമാണ്.

Dominant eigenvalue കണ്ടെത്താൻ power iteration method ഉപയോഗിക്കാം:

$$
\lambda_1 = \lim_{k \to \infty} \frac{\|A^k \mathbf{v}_0\|}{\|A^{k-1} \mathbf{v}_0\|}
$$