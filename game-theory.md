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

ഈ lecture, game theory-യിലെ അടിസ്ഥാന concepts-ഉം economic decision-making-ൽ അവയുടെ applications-ഉം പരിചയപ്പെടുത്തുന്നു. Strategic interactions, Nash equilibrium, കൂടാതെ common game structures എന്നിവ നമ്മൾ പരിശോധിക്കും.

## Introduction to Strategic Thinking

ഒന്നിലധികം decision-makers strategic ആയി interact ചെയ്യുന്ന സാഹചര്യങ്ങൾ analyze ചെയ്യാനുള്ള ഒരു framework ആണ് Game theory നൽകുന്നത്. ഓരോ player-യുടെയും optimal choice മറ്റുള്ളവർ എന്ത് choose ചെയ്യുന്നു എന്നതിനെ depend ചെയ്യുന്നു, ഇത് സങ്കീർണ്ണമായ interdependencies ഉണ്ടാക്കുന്നു.

Economic markets-ൽ, prices-ലോ quantities-ലോ compete ചെയ്യുന്ന firms strategic games-ൽ ഏർപ്പെടുന്നു. ഈ interactions മനസ്സിലാക്കുന്നത് market outcomes predict ചെയ്യാനും മെച്ചപ്പെട്ട mechanisms design ചെയ്യാനും സഹായിക്കുന്നു.

## The Prisoner's Dilemma

Cooperation എല്ലാവർക്കും ഗുണകരമാണെങ്കിലും rational individuals എന്തുകൊണ്ട് cooperate ചെയ്യണമെന്നില്ല എന്നത് classic Prisoner's Dilemma വ്യക്തമാക്കുന്നു. രണ്ട് suspects-നെ പ്രത്യേകം interrogate ചെയ്യുന്നു, confess ചെയ്യണോ silent ആയി തുടരണോ എന്ന് അവർ തീരുമാനിക്കണം.

Payoff matrix strategic structure ഇപ്രകാരം പിടിച്ചെടുക്കുന്നു:

|                | Cooperate    | Defect      |
|----------------|-------------|-------------|
| **Cooperate**  | (-1, -1)    | (-3, 0)     |
| **Defect**     | (0, -3)     | (-2, -2)    |

ഓരോ player-ക്കും defect ചെയ്യാൻ ഒരു dominant strategy ഉണ്ട്, ഇത് ഇരുവർക്കും suboptimal ആയ outcome-ലേക്ക് നയിക്കുന്നു.

### Real-World Applications

Prisoner's Dilemma structure പല economic contexts-ലും കാണാം:
- Firms തമ്മിലുള്ള price competition
- Public goods provision
- Environmental protection agreements
- Arms races-ഉം military spending-ഉം

ഈ game മനസ്സിലാക്കുന്നത് market failures-ഉം regulation അല്ലെങ്കിൽ cooperation mechanisms-ന്റെ ആവശ്യകതയും വിശദീകരിക്കാൻ സഹായിക്കുന്നു.

## Nash Equilibrium

ഒരു player-ക്കും unilaterally strategy മാറ്റിക്കൊണ്ട് അവരുടെ payoff മെച്ചപ്പെടുത്താൻ കഴിയാത്ത strategy profile ആണ് Nash equilibrium. മറ്റുള്ളവരുടെ strategies കണക്കിലെടുക്കുമ്പോൾ ഓരോ player-യുടെയും strategy optimal ആയിരിക്കുന്ന stable outcome-നെ ഇത് represent ചെയ്യുന്നു.

Mathematically, $i = 1, \ldots, n$ എന്ന players-ന്, strategy sets $S_i$-ഉം payoff functions $u_i$-ഉം ഉള്ളപ്പോൾ, $(s_1^*, \ldots, s_n^*)$ എന്ന strategy profile ഒരു Nash equilibrium ആകുന്നത് ഇങ്ങനെയാണ്:

$$
u_i(s_i^*, s_{-i}^*) \geq u_i(s_i, s_{-i}^*) \quad \forall s_i \in S_i, \forall i
$$

നമുക്ക് Python-ൽ ഒരു simple game-ന്റെ Nash equilibria compute ചെയ്യാം:

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

Pure strategy Nash equilibrium ഒന്നും exist ചെയ്യാത്തപ്പോൾ, players strategies-നു മേൽ randomize ചെയ്തേക്കാം. ഈ mixed strategy equilibria-യ്ക്ക് players randomize ചെയ്യാൻ തയ്യാറാകുന്ന indifference conditions ആവശ്യമാണ്.

Mixed strategy equilibrium probabilities താഴെ പറയുന്നത് solve ചെയ്തുകൊണ്ട് compute ചെയ്യാം:

```{math}
\begin{align}
p \cdot u_1(\text{Strategy 1}) &= p \cdot u_1(\text{Strategy 2}) \\
q \cdot u_2(\text{Strategy 1}) &= q \cdot u_2(\text{Strategy 2})
\end{align}
```

## Sequential Games

Sequential games-ൽ, players ഒരു specific order-ൽ move ചെയ്യുകയും മുൻ actions observe ചെയ്യുകയും ചെയ്യുന്നു. ഈ games backward induction ഉപയോഗിച്ചാണ് analyze ചെയ്യുന്നത്, game tree-യുടെ അവസാനത്തിൽ നിന്ന് തുടക്കത്തിലേക്ക് പ്രവർത്തിക്കുന്നു.

Subgame perfection ആണ് key insight: ഓരോ decision node-ലും, future play കണക്കിലെടുത്ത് players optimal ആയി choose ചെയ്യുന്നു. Rational players execute ചെയ്യാത്ത non-credible threats ഇത് ഒഴിവാക്കുന്നു.

### Stackelberg Competition

Stackelberg competition ഒരു classic application ആണ്, ഇവിടെ ഒരു firm (leader) ആദ്യം quantity choose ചെയ്യുന്നു, മറ്റൊരു firm (follower) decide ചെയ്യുന്നതിന് മുമ്പ് ഈ choice observe ചെയ്യുന്നു.

Leader, follower-യുടെ reaction function anticipate ചെയ്ത് optimal ആയി choose ചെയ്യുന്നു:

$$
\max_{q_L} \pi_L(q_L, R(q_L))
$$

ഇവിടെ $R(q_L)$ എന്നത് leader-യുടെ quantity $q_L$-ന് follower-യുടെ best response ആണ്.

നമുക്ക് ഒരു Stackelberg game numerically solve ചെയ്യാം:

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

Games ആവർത്തിച്ച് play ചെയ്യുമ്പോൾ, reputation-ഉം punishment mechanisms-ഉം വഴി പുതിയ equilibria ഉയർന്നുവരുന്നു. Players മതിയായ patient ആയിരിക്കുമ്പോൾ പല outcomes-ഉം equilibria ആയി sustain ചെയ്യാൻ കഴിയുമെന്ന് Folk Theorem കാണിക്കുന്നു.

Cooperative path-ൽ നിന്ന് ആരെങ്കിലും deviate ചെയ്താൽ non-cooperative play-ലേക്ക് reversion ഉണ്ടാകുമെന്ന threat ആണ് key mechanism. ഇതിന് ഇവ ആവശ്യമാണ്:

1. Players future payoffs value ചെയ്യുന്നു (discount factor $\delta < 1$)
2. Deviations observable ആണ്
3. Punishment threats credible ആണ്

### Trigger Strategies

Grim trigger strategy ആണ് ഒരു common enforcement mechanism: ആരെങ്കിലും defect ചെയ്യുന്നത് വരെ cooperate ചെയ്യുക, പിന്നീട് എപ്പോഴും defect ചെയ്യുക. ഈ strategy-ക്ക് cooperation sustain ചെയ്യാൻ കഴിയുന്നത് ഇങ്ങനെയാണ്:

$$
\frac{1}{1-\delta} \cdot \pi_{\text{coop}} \geq \pi_{\text{deviate}} + \frac{\delta}{1-\delta} \cdot \pi_{\text{punish}}
$$

ഇടതു വശം perpetual cooperation-ൽ നിന്നുള്ള payoff ആണ്, വലതു വശമാകട്ടെ deviate ചെയ്തതിനെ തുടർന്ന് perpetual punishment ഉണ്ടാകുമ്പോൾ ലഭിക്കുന്ന gain ആണ്.

## Exercises

1. **Computing Equilibria**: താഴെ പറയുന്ന game-ന് എല്ലാ Nash equilibria-യും (pure ഉം mixed ഉം) കണ്ടെത്തുക:
   
   |           | Left  | Right |
   |-----------|-------|-------|
   | **Up**    | (3,2) | (1,3) |
   | **Down**  | (0,1) | (2,4) |

2. **Repeated Games**: Grim trigger strategies ഉപയോഗിച്ച് infinitely repeated Prisoner's Dilemma-യിൽ cooperation sustain ചെയ്യാൻ ആവശ്യമായ minimum discount factor calculate ചെയ്യുക.

3. **Sequential Games**: Firms ഒരു market-ൽ enter ചെയ്യണോ എന്ന് sequentially decide ചെയ്യുന്ന three-stage entry game-ലെ subgame perfect equilibrium solve ചെയ്യുക.

4. **Mixed Strategies**: Exercise 1-ന് നിങ്ങൾ compute ചെയ്ത mixed strategy equilibrium, ഇരു players-നെയും അവരുടെ pure strategies-ക്കിടയിൽ indifferent ആക്കുന്നു എന്ന് verify ചെയ്യുക.