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

ഈ lecture, game theory-യിലെ അടിസ്ഥാന concepts-ഉം economic decision-making-ൽ അവയുടെ applications-ഉം പരിചയപ്പെടുത്തുന്നു. Strategic interactions, Nash equilibrium, സാധാരണ game structures എന്നിവ നമുക്ക് നോക്കാം.

## Introduction to Strategic Thinking

Game theory, ഒന്നിലധികം decision-makers strategic ആയി interact ചെയ്യുന്ന സാഹചര്യങ്ങളെ analyze ചെയ്യാനുള്ള ഒരു framework നൽകുന്നു. ഓരോ player-യുടെയും optimal choice മറ്റുള്ളവർ എന്ത് choose ചെയ്യുന്നു എന്നതിനെ ആശ്രയിച്ചിരിക്കുന്നു, ഇത് സങ്കീർണ്ണമായ interdependencies സൃഷ്ടിക്കുന്നു.

Economic markets-ൽ, prices-ലോ quantities-ലോ compete ചെയ്യുന്ന firms strategic games-ൽ ഏർപ്പെടുന്നു. ഈ interactions മനസ്സിലാക്കുന്നത് market outcomes predict ചെയ്യാനും മികച്ച mechanisms design ചെയ്യാനും സഹായിക്കുന്നു.

## The Prisoner's Dilemma

Classic ആയ Prisoner's Dilemma, cooperation എല്ലാവർക്കും ഗുണകരമാണെങ്കിൽ പോലും rational individuals എന്തുകൊണ്ട് cooperate ചെയ്തെന്നു വരില്ല എന്ന് വിശദീകരിക്കുന്നു. രണ്ട് suspects-നെ വെവ്വേറെ interrogate ചെയ്യുന്നു, അവർ confess ചെയ്യണോ അതോ silent ആയി തുടരണോ എന്ന് തീരുമാനിക്കണം.

Payoff matrix, strategic structure-നെ ഇങ്ങനെ പകർത്തുന്നു:

|                | Cooperate    | Defect      |
|----------------|-------------|-------------|
| **Cooperate**  | (-1, -1)    | (-3, 0)     |
| **Defect**     | (0, -3)     | (-2, -2)    |

ഓരോ player-ക്കും defect ചെയ്യുക എന്നതാണ് dominant strategy, ഇത് രണ്ട് players-ക്കും suboptimal ആയ ഒരു outcome-ലേക്ക് നയിക്കുന്നു.

### Real-World Applications

Prisoner's Dilemma structure പല economic contexts-ലും കാണാം:
- Firms തമ്മിലുള്ള price competition
- Public goods provision
- Environmental protection agreements
- Arms races-ഉം military spending-ഉം

ഈ game മനസ്സിലാക്കുന്നത് market failures-ഉം regulation-ന്റെയോ cooperation mechanisms-ന്റെയോ ആവശ്യകതയും വിശദീകരിക്കാൻ സഹായിക്കുന്നു.

## Nash Equilibrium

Nash equilibrium എന്നത് ഒരു strategy profile ആണ്, ഇതിൽ ഒരു player-ക്കും strategy ഒറ്റയ്ക്ക് മാറ്റിക്കൊണ്ട് തന്റെ payoff improve ചെയ്യാൻ കഴിയില്ല. ഇത് ഒരു stable outcome-നെ represent ചെയ്യുന്നു, ഇവിടെ മറ്റുള്ളവരുടെ strategies കണക്കിലെടുത്താൽ ഓരോ player-യുടെയും strategy optimal ആണ്.

Mathematically, $i = 1, \ldots, n$ players-ക്ക്, strategy sets $S_i$-ഉം payoff functions $u_i$-ഉം ഉള്ളപ്പോൾ, താഴെ പറയുന്ന condition satisfy ചെയ്താൽ $(s_1^*, \ldots, s_n^*)$ എന്ന strategy profile ഒരു Nash equilibrium ആണ്:

$$
u_i(s_i^*, s_{-i}^*) \geq u_i(s_i, s_{-i}^*) \quad \forall s_i \in S_i, \forall i
$$

Python-ൽ ഒരു simple ആയ game-യ്ക്കുള്ള Nash equilibria നമുക്ക് compute ചെയ്യാം:

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

Pure strategy Nash equilibrium ഒന്നും exist ചെയ്യാത്തപ്പോൾ, players strategies-ൽ randomize ചെയ്തേക്കാം. ഈ mixed strategy equilibria-ക്ക് indifference conditions ആവശ്യമായിവരുന്നു, ഇവിടെ players randomize ചെയ്യാൻ തയ്യാറാകുന്നു.

Mixed strategy equilibrium probabilities താഴെ പറയുന്നത് solve ചെയ്താൽ compute ചെയ്യാം:

```{math}
\begin{align}
p \cdot u_1(\text{Strategy 1}) &= p \cdot u_1(\text{Strategy 2}) \\
q \cdot u_2(\text{Strategy 1}) &= q \cdot u_2(\text{Strategy 2})
\end{align}
```

## Sequential Games

Sequential games-ൽ, players ഒരു specific order-ൽ move ചെയ്യുകയും മുൻപുള്ള actions observe ചെയ്യുകയും ചെയ്യുന്നു. ഈ games-നെ backward induction ഉപയോഗിച്ച് analyze ചെയ്യുന്നു, game tree-യുടെ അവസാനത്തിൽ നിന്നും തുടക്കത്തിലേക്ക് പ്രവർത്തിച്ചുകൊണ്ട്.

Subgame perfection ആണ് ഇവിടത്തെ key insight: ഓരോ decision node-ലും, players, future play കണക്കിലെടുത്ത് optimal ആയി choose ചെയ്യുന്നു. ഇത് rational players execute ചെയ്യാത്ത non-credible threats-നെ ഒഴിവാക്കുന്നു.

### Stackelberg Competition

Classic ആയ ഒരു application ആണ് Stackelberg competition, ഇവിടെ ഒരു firm (leader) ആദ്യം quantity choose ചെയ്യുന്നു, മറ്റൊരു firm (follower) ഈ choice observe ചെയ്തതിന് ശേഷം decide ചെയ്യുന്നു.

Leader, follower-യുടെ reaction function anticipate ചെയ്ത് optimal ആയി choose ചെയ്യുന്നു:

$$
\max_{q_L} \pi_L(q_L, R(q_L))
$$

ഇവിടെ $R(q_L)$ എന്നത്, leader-യുടെ quantity $q_L$-ന് follower-യുടെ best response ആണ്.

ഒരു Stackelberg game numerically നമുക്ക് solve ചെയ്യാം:

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

Games repeatedly play ചെയ്യുമ്പോൾ, reputation-ഉം punishment mechanisms-ഉം വഴി പുതിയ equilibria ഉണ്ടാകുന്നു. Players മതിയായ patient ആയിരിക്കുമ്പോൾ പല outcomes-ഉം equilibria ആയി sustain ചെയ്യാമെന്ന് Folk Theorem കാണിക്കുന്നു.

ആരെങ്കിലും cooperative path-ൽ നിന്നും deviate ചെയ്താൽ non-cooperative play-ലേക്ക് തിരിച്ചുപോകും എന്ന threat ആണ് ഇവിടത്തെ key mechanism. ഇതിന് താഴെ പറയുന്നവ ആവശ്യമായിവരുന്നു:

1. Players future payoffs-നെ വിലമതിക്കുന്നു (discount factor $\delta < 1$)
2. Deviations observable ആണ്
3. Punishment threats credible ആണ്

### Trigger Strategies

ഒരു സാധാരണ enforcement mechanism ആണ് grim trigger strategy: ആരെങ്കിലും defect ചെയ്യുന്നത് വരെ cooperate ചെയ്യുക, പിന്നീട് എന്നെന്നേക്കുമായി defect ചെയ്യുക. ഈ strategy, താഴെ പറയുന്ന condition ഉള്ളപ്പോൾ cooperation sustain ചെയ്യാൻ കഴിയും:

$$
\frac{1}{1-\delta} \cdot \pi_{\text{coop}} \geq \pi_{\text{deviate}} + \frac{\delta}{1-\delta} \cdot \pi_{\text{punish}}
$$

ഇടത് വശം, perpetual cooperation-ൽ നിന്നുള്ള payoff ആണ്, വലത് വശമാകട്ടെ, deviate ചെയ്തതിന് ശേഷം perpetual punishment വന്നാലുള്ള gain ആണ്.

## Exercises

1. **Computing Equilibria**: Find all Nash equilibria (pure and mixed) for the following game:
   
   |           | Left  | Right |
   |-----------|-------|-------|
   | **Up**    | (3,2) | (1,3) |
   | **Down**  | (0,1) | (2,4) |

2. **Repeated Games**: Calculate the minimum discount factor needed to sustain cooperation in an infinitely repeated Prisoner's Dilemma using grim trigger strategies.

3. **Sequential Games**: Solve for the subgame perfect equilibrium in a three-stage entry game where firms decide sequentially whether to enter a market.

4. **Mixed Strategies**: Verify that the mixed strategy equilibrium you computed for Exercise 1 makes both players indifferent between their pure strategies.