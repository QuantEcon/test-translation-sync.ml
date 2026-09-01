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
  title: Game Theory Basics
  headings:
    Introduction to Strategic Thinking: Introduction to Strategic Thinking
    The Prisoner's Dilemma: The Prisoner's Dilemma
    The Prisoner's Dilemma::Real-World Applications: Real-World Applications
    Nash Equilibrium: Nash Equilibrium
    Nash Equilibrium::Mixed Strategies: Mixed Strategies
    Sequential Games: Sequential Games
    Sequential Games::Stackelberg Competition: Stackelberg Competition
    Repeated Games: Repeated Games
    Repeated Games::Trigger Strategies: Trigger Strategies
    Exercises: Exercises
---

# Game Theory Basics

ഈ lecture game theory-യിലെ അടിസ്ഥാന concepts-നെയും economic decision-making-ലെ അവയുടെ applications-നെയും പരിചയപ്പെടുത്തുന്നു. Strategic interactions, Nash equilibrium, ഒപ്പം common game structures എന്നിവ നമുക്ക് നോക്കാം.

## Introduction to Strategic Thinking

Multiple decision-makers strategic ആയി interact ചെയ്യുന്ന സാഹചര്യങ്ങളെ analysis ചെയ്യാനുള്ള ഒരു framework ആണ് game theory provide ചെയ്യുന്നത്. ഓരോ player-ന്റെയും optimal choice മറ്റുള്ളവർ എന്ത് choose ചെയ്യുന്നു എന്നതിനെ ആശ്രയിച്ചാണിരിക്കുന്നത്, ഇത് complex ആയ interdependencies സൃഷ്ടിക്കുന്നു.

Economic markets-ൽ prices-ലോ quantities-ലോ compete ചെയ്യുന്ന firms strategic games-ൽ ഏർപ്പെടുന്നു. ഈ interactions മനസ്സിലാക്കുന്നത് market outcomes പ്രവചിക്കാനും മെച്ചപ്പെട്ട mechanisms design ചെയ്യാനും സഹായിക്കുന്നു.

## The Prisoner's Dilemma

Cooperation എല്ലാവർക്കും ഗുണം ചെയ്യുമ്പോൾ പോലും rational individuals എന്തുകൊണ്ട് cooperate ചെയ്യാതിരിക്കാം എന്നത് classic ആയ Prisoner's Dilemma illustrate ചെയ്യുന്നു. രണ്ട് suspects-നെ വെവ്വേറെ interrogate ചെയ്യുകയും confess ചെയ്യണോ silent ആയി തുടരണോ എന്ന് decide ചെയ്യേണ്ടിവരുകയും ചെയ്യുന്നു.

Payoff matrix ഈ strategic structure-നെ capture ചെയ്യുന്നു:

|                | Cooperate    | Defect      |
|----------------|-------------|-------------|
| **Cooperate**  | (-1, -1)    | (-3, 0)     |
| **Defect**     | (0, -3)     | (-2, -2)    |

ഓരോ player-ക്കും defect ചെയ്യാനുള്ള ഒരു dominant strategy ഉണ്ട്, ഇത് രണ്ട് പേർക്കും suboptimal ആയ ഒരു outcome-ലേക്ക് നയിക്കുന്നു.

### Real-World Applications

Prisoner's Dilemma structure പല economic contexts-ലും കാണാം:
- Firms തമ്മിലുള്ള price competition
- Public goods provision
- Environmental protection agreements
- Arms races-ഉം military spending-ഉം

ഈ game മനസ്സിലാക്കുന്നത് market failures-നെയും regulation-ന്റെയോ cooperation mechanisms-ന്റെയോ ആവശ്യകതയെയും വിശദീകരിക്കാൻ സഹായിക്കുന്നു.

## Nash Equilibrium

Unilateral ആയി strategy മാറ്റുന്നതിലൂടെ ഒരു player-ക്കും തന്റെ payoff improve ചെയ്യാൻ കഴിയാത്ത ഒരു strategy profile ആണ് Nash equilibrium. മറ്റുള്ളവരുടെ strategies കണക്കിലെടുത്ത് ഓരോ player-ന്റെയും strategy optimal ആയിരിക്കുന്ന ഒരു stable outcome-നെ ഇത് represent ചെയ്യുന്നു.

Mathematically, strategy sets $S_i$-ഉം payoff functions $u_i$-ഉം ഉള്ള players $i = 1, \ldots, n$-ന്, ഒരു strategy profile $(s_1^*, \ldots, s_n^*)$ Nash equilibrium ആകുന്നത് ഇങ്ങനെയാണ്:

$$
u_i(s_i^*, s_{-i}^*) \geq u_i(s_i, s_{-i}^*) \quad \forall s_i \in S_i, \forall i
$$

Python-ൽ ഒരു simple game-ന്റെ Nash equilibria നമുക്ക് compute ചെയ്യാം:

```{code-cell} python
import numpy as np
import nashpy as nash

# Define a simple 2x2 game
# Player 1's payoff matrix
A = np.array([[3, 0],
              [5, 1]])

# Player 2's payoff matrix  
B = np.array([[3, 5],
              [0, 1]])

# Create game
game = nash.Game(A, B)

# Find Nash equilibria
equilibria = list(game.support_enumeration())

print("Nash Equilibria:")
for eq in equilibria:
    print(f"Player 1: {eq[0]}, Player 2: {eq[1]}")
    print(f"Payoffs: ({np.dot(eq[0], A @ eq[1])}, {np.dot(eq[0], B.T @ eq[1])})")
    print()
```

### Mixed Strategies

Pure strategy Nash equilibrium exist ചെയ്യാത്തപ്പോൾ, players strategies-ന് മേൽ randomize ചെയ്യാം. ഈ mixed strategy equilibria-ക്ക് players randomize ചെയ്യാൻ തയ്യാറാകുന്ന indifference conditions ആവശ്യമായിവരുന്നു.

Mixed strategy equilibrium probabilities താഴെ കൊടുത്തിരിക്കുന്നത് solve ചെയ്ത് compute ചെയ്യാം:

```{math}
\begin{align}
p \cdot u_1(\text{Strategy 1}) &= p \cdot u_1(\text{Strategy 2}) \\
q \cdot u_2(\text{Strategy 1}) &= q \cdot u_2(\text{Strategy 2})
\end{align}
```

## Sequential Games

Sequential games-ൽ players ഒരു specific order-ൽ move ചെയ്യുകയും മുൻപത്തെ actions observe ചെയ്യുകയും ചെയ്യുന്നു. Game tree-യുടെ അവസാനത്തിൽ നിന്നും തുടക്കത്തിലേക്ക് പോയി backward induction ഉപയോഗിച്ചാണ് ഈ games analysis ചെയ്യുന്നത്.

Subgame perfection ആണ് ഇവിടത്തെ key insight: ഓരോ decision node-ലും, future play കണക്കിലെടുത്ത് players optimal ആയി choose ചെയ്യുന്നു. Rational players execute ചെയ്യാത്ത non-credible threats ഇത് ഒഴിവാക്കുന്നു.

### Stackelberg Competition

Stackelberg competition ആണ് ഒരു classic application, ഇവിടെ ഒരു firm (leader) ആദ്യം quantity choose ചെയ്യുന്നു, മറ്റൊരു firm (follower) decide ചെയ്യുന്നതിന് മുമ്പ് ഈ choice observe ചെയ്യുന്നു.

Leader, follower-ന്റെ reaction function മുൻകൂട്ടി കണ്ട് optimal ആയി choose ചെയ്യുന്നു:

$$
\max_{q_L} \pi_L(q_L, R(q_L))
$$

ഇവിടെ $R(q_L)$ എന്നത് leader-ന്റെ quantity $q_L$-ന് follower-ന്റെ best response ആണ്.

നമുക്ക് ഒരു Stackelberg game numerically ആയി solve ചെയ്യാം:

```{code-cell} python
from scipy.optimize import minimize_scalar

# Market parameters
a = 100  # Demand intercept
c = 10   # Marginal cost

# Follower's reaction function: q_F = (a - c - q_L) / 2
def follower_response(q_L):
    return (a - c - q_L) / 2

# Leader's profit given follower's response
def leader_profit(q_L):
    q_F = follower_response(q_L)
    price = a - q_L - q_F
    return -(price - c) * q_L  # Negative for minimization

# Find leader's optimal quantity
result = minimize_scalar(leader_profit, bounds=(0, a-c), method='bounded')
q_L_optimal = result.x
q_F_optimal = follower_response(q_L_optimal)

print(f"Leader quantity: {q_L_optimal:.2f}")
print(f"Follower quantity: {q_F_optimal:.2f}")
print(f"Market price: {a - q_L_optimal - q_F_optimal:.2f}")
print(f"Leader profit: {-result.fun:.2f}")
```

## Repeated Games

Games repeatedly play ചെയ്യുമ്പോൾ, reputation-ഉം punishment mechanisms-ഉം വഴി പുതിയ equilibria ഉണ്ടാകുന്നു. Players മതിയായി patient ആയിരിക്കുമ്പോൾ പല outcomes-നെയും equilibria ആയി sustain ചെയ്യാൻ കഴിയുമെന്ന് Folk Theorem കാണിക്കുന്നു.

ആരെങ്കിലും cooperative path-ൽ നിന്നും deviate ചെയ്താൽ non-cooperative play-യിലേക്ക് മടങ്ങുമെന്ന threat ആണ് ഇവിടത്തെ key mechanism. ഇതിന് ഇവ ആവശ്യമായിവരുന്നു:

1. Players future payoffs-ന് വില കൽപ്പിക്കുന്നു (discount factor $\delta < 1$)
2. Deviations observable ആണ്
3. Punishment threats credible ആണ്

### Trigger Strategies

Grim trigger strategy ആണ് ഒരു common enforcement mechanism: ആരെങ്കിലും defect ചെയ്യുന്നത് വരെ cooperate ചെയ്യുക, പിന്നീട് എന്നെന്നേക്കും defect ചെയ്യുക. ഈ strategy താഴെ പറയുന്ന condition-ൽ cooperation sustain ചെയ്യാൻ കഴിയും:

$$
\frac{1}{1-\delta} \cdot \pi_{\text{coop}} \geq \pi_{\text{deviate}} + \frac{\delta}{1-\delta} \cdot \pi_{\text{punish}}
$$

ഇടത് വശം perpetual cooperation-ൽ നിന്നുള്ള payoff ആണ്, വലത് വശം deviate ചെയ്തതിന് ശേഷം perpetual punishment വരുന്നതിൽ നിന്നുള്ള gain ആണ്.

## Exercises

1. **Computing Equilibria**: Find all Nash equilibria (pure and mixed) for the following game:
   
   |           | Left  | Right |
   |-----------|-------|-------|
   | **Up**    | (3,2) | (1,3) |
   | **Down**  | (0,1) | (2,4) |

2. **Repeated Games**: Calculate the minimum discount factor needed to sustain cooperation in an infinitely repeated Prisoner's Dilemma using grim trigger strategies.

3. **Sequential Games**: Solve for the subgame perfect equilibrium in a three-stage entry game where firms decide sequentially whether to enter a market.

4. **Mixed Strategies**: Verify that the mixed strategy equilibrium you computed for Exercise 1 makes both players indifferent between their pure strategies.