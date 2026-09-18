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

ഈ lecture, game theory-യിലെ അടിസ്ഥാന concepts-ഉം economic decision-making-ൽ അവയുടെ applications-ഉം introduce ചെയ്യുന്നു. Strategic interactions, Nash equilibrium, common game structures എന്നിവയെക്കുറിച്ച് നമുക്ക് നോക്കാം.

## Introduction to Strategic Thinking

Game theory എന്നത്, ഒന്നിലധികം decision-makers strategic ആയി interact ചെയ്യുന്ന situations-നെ analyze ചെയ്യുന്നതിനുള്ള ഒരു framework provide ചെയ്യുന്നു. ഓരോ player-ന്റെയും optimal choice, മറ്റുള്ളവർ എന്ത് തിരഞ്ഞെടുക്കുന്നു എന്നതിനെ ആശ്രയിച്ചിരിക്കുന്നു, ഇത് complex ആയ interdependencies സൃഷ്ടിക്കുന്നു.

Economic markets-ൽ, prices-ലോ quantities-ലോ compete ചെയ്യുന്ന firms strategic games-ൽ ഏർപ്പെടുന്നു. ഈ interactions മനസ്സിലാക്കുന്നത്, market outcomes predict ചെയ്യാനും മെച്ചപ്പെട്ട mechanisms design ചെയ്യാനും സഹായിക്കുന്നു.

## The Prisoner's Dilemma

Cooperation എല്ലാവർക്കും പ്രയോജനകരമാണെങ്കിൽ പോലും rational individuals എന്തുകൊണ്ട് cooperate ചെയ്യണമെന്നില്ല എന്നത്, classic ആയ Prisoner's Dilemma illustrate ചെയ്യുന്നു. രണ്ട് suspects-നെ വെവ്വേറെ interrogate ചെയ്യുന്നു, അവർ confess ചെയ്യണോ അതോ silent ആയി തുടരണോ എന്ന് തീരുമാനിക്കണം.

Payoff matrix, strategic structure-നെ capture ചെയ്യുന്നു:

|                | Cooperate    | Defect      |
|----------------|-------------|-------------|
| **Cooperate**  | (-1, -1)    | (-3, 0)     |
| **Defect**     | (0, -3)     | (-2, -2)    |

ഓരോ player-നും defect ചെയ്യാനുള്ള ഒരു dominant strategy ഉണ്ട്, ഇത് ഇരുവർക്കും suboptimal ആയ ഒരു outcome-ലേക്ക് നയിക്കുന്നു.

### Real-World Applications

Prisoner's Dilemma structure ഒന്നിലധികം economic contexts-ൽ കാണാം:
- Firms തമ്മിലുള്ള price competition
- Public goods provision
- Environmental protection agreements
- Arms races-ഉം military spending-ഉം

ഈ game മനസ്സിലാക്കുന്നത്, market failures-ഉം regulation-ന്റെയോ cooperation mechanisms-ന്റെയോ ആവശ്യകതയും വിശദീകരിക്കാൻ സഹായിക്കുന്നു.

## Nash Equilibrium

ഒരു Nash equilibrium എന്നത്, ഒരു player-ക്കും unilateral ആയി strategy മാറ്റിക്കൊണ്ട് തന്റെ payoff improve ചെയ്യാൻ കഴിയാത്ത ഒരു strategy profile ആണ്. ഇത്, മറ്റുള്ളവരുടെ strategies കണക്കിലെടുക്കുമ്പോൾ ഓരോ player-ന്റെയും strategy optimal ആയിരിക്കുന്ന ഒരു stable outcome-നെ represent ചെയ്യുന്നു.

Mathematical ആയി, $i = 1, \ldots, n$ എന്ന players-ക്ക് strategy sets $S_i$-ഉം payoff functions $u_i$-ഉം ഉള്ളപ്പോൾ, $(s_1^*, \ldots, s_n^*)$ എന്ന strategy profile ഒരു Nash equilibrium ആകുന്നത് താഴെ പറയുന്ന condition satisfy ചെയ്യുമ്പോഴാണ്:

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

Pure strategy Nash equilibrium ഒന്നും exist ചെയ്യാത്തപ്പോൾ, players strategies-ന് മേൽ randomize ചെയ്തേക്കാം. ഈ mixed strategy equilibria-ക്ക്, players randomize ചെയ്യാൻ തയ്യാറാകുന്ന indifference conditions ആവശ്യമായിവരുന്നു.

Mixed strategy equilibrium probabilities താഴെ കൊടുത്തിരിക്കുന്നത് solve ചെയ്തുകൊണ്ട് compute ചെയ്യാം:

```{math}
\begin{align}
p \cdot u_1(\text{Strategy 1}) &= p \cdot u_1(\text{Strategy 2}) \\
q \cdot u_2(\text{Strategy 1}) &= q \cdot u_2(\text{Strategy 2})
\end{align}
```

## Sequential Games

Sequential games-ൽ, players ഒരു specific order-ൽ move ചെയ്യുകയും മുമ്പത്തെ actions observe ചെയ്യുകയും ചെയ്യുന്നു. Game tree-യുടെ അവസാനത്തിൽ നിന്നും തുടക്കത്തിലേക്ക് work ചെയ്തുകൊണ്ട്, backward induction ഉപയോഗിച്ചാണ് ഈ games analyze ചെയ്യുന്നത്.

Key insight, subgame perfection ആണ്: ഓരോ decision node-ലും, players future play കണക്കിലെടുത്ത് optimal ആയി choose ചെയ്യുന്നു. Rational players execute ചെയ്യാത്ത non-credible threats-നെ ഇത് ഒഴിവാക്കുന്നു.

### Stackelberg Competition

ഒരു firm (leader) ആദ്യം quantity choose ചെയ്യുകയും, മറ്റൊരു firm (follower) ഈ choice observe ചെയ്ത ശേഷം തീരുമാനിക്കുകയും ചെയ്യുന്ന Stackelberg competition ഒരു classic application ആണ്.

Leader, follower-ന്റെ reaction function മുൻകൂട്ടി കണ്ട് optimal ആയി choose ചെയ്യുന്നു:

$$
\max_{q_L} \pi_L(q_L, R(q_L))
$$

ഇവിടെ $R(q_L)$ എന്നത്, leader-ന്റെ quantity $q_L$-നോടുള്ള follower-ന്റെ best response ആണ്.

നമുക്ക് ഒരു Stackelberg game numerical ആയി solve ചെയ്യാം:

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

Games repeatedly play ചെയ്യുമ്പോൾ, reputation-ഉം punishment mechanisms-ഉം വഴി പുതിയ equilibria ഉണ്ടാകുന്നു. Players മതിയായത്ര patient ആയിരിക്കുമ്പോൾ ഒന്നിലധികം outcomes equilibria ആയി sustain ചെയ്യാൻ കഴിയുമെന്ന് Folk Theorem കാണിക്കുന്നു.

Key mechanism, ആരെങ്കിലും cooperative path-ൽ നിന്നും deviate ചെയ്താൽ non-cooperative play-ലേക്ക് reversion ചെയ്യുമെന്ന threat ആണ്. ഇതിന് താഴെപ്പറയുന്നവ ആവശ്യമായിവരുന്നു:

1. Players future payoffs-ന് value നൽകുന്നു (discount factor $\delta < 1$)
2. Deviations observable ആണ്
3. Punishment threats credible ആണ്

### Trigger Strategies

ഒരു സാധാരണ enforcement mechanism, grim trigger strategy ആണ്: ആരെങ്കിലും defect ചെയ്യുന്നത് വരെ cooperate ചെയ്യുക, അതിനുശേഷം എന്നേക്കും defect ചെയ്യുക. ഈ strategy-ക്ക് താഴെ പറയുന്ന condition-ൽ cooperation sustain ചെയ്യാൻ കഴിയും:

$$
\frac{1}{1-\delta} \cdot \pi_{\text{coop}} \geq \pi_{\text{deviate}} + \frac{\delta}{1-\delta} \cdot \pi_{\text{punish}}
$$

ഇടത് വശം, perpetual cooperation-ൽ നിന്നുള്ള payoff ആണ്, വലത് വശം, deviate ചെയ്തതിനു ശേഷം perpetual punishment ഉണ്ടാകുന്നതിൽ നിന്നുള്ള gain ആണ്.

## Exercises

1. **Computing Equilibria**: Find all Nash equilibria (pure and mixed) for the following game:
   
   |           | Left  | Right |
   |-----------|-------|-------|
   | **Up**    | (3,2) | (1,3) |
   | **Down**  | (0,1) | (2,4) |

2. **Repeated Games**: Calculate the minimum discount factor needed to sustain cooperation in an infinitely repeated Prisoner's Dilemma using grim trigger strategies.

3. **Sequential Games**: Solve for the subgame perfect equilibrium in a three-stage entry game where firms decide sequentially whether to enter a market.

4. **Mixed Strategies**: Verify that the mixed strategy equilibrium you computed for Exercise 1 makes both players indifferent between their pure strategies.