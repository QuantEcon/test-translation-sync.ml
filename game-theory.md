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

ഈ lecture game theory-യിലെ അടിസ്ഥാന concepts-ഉം അവയുടെ economic decision-making-ലേക്കുള്ള applications-ഉം പരിചയപ്പെടുത്തുന്നു. Strategic interactions, Nash equilibrium, common game structures എന്നിവ നമ്മൾ പരിശോധിക്കും.

## Introduction to Strategic Thinking

Multiple decision-makers strategic ആയി interact ചെയ്യുന്ന സാഹചര്യങ്ങൾ analyze ചെയ്യാനുള്ള ഒരു framework ആണ് game theory നൽകുന്നത്. ഓരോ player-ന്റെയും optimal choice, മറ്റുള്ളവർ എന്ത് choose ചെയ്യുന്നു എന്നതിനെ depend ചെയ്യുന്നു. ഇത് സങ്കീർണ്ണമായ interdependencies ഉണ്ടാക്കുന്നു.

Economic markets-ൽ prices-ലോ quantities-ലോ compete ചെയ്യുന്ന firms strategic games-ൽ ഏർപ്പെടുന്നു. ഈ interactions മനസ്സിലാക്കുന്നത് market outcomes predict ചെയ്യാനും മെച്ചപ്പെട്ട mechanisms design ചെയ്യാനും സഹായിക്കുന്നു.

## The Prisoner's Dilemma

Classic ആയ Prisoner's Dilemma, cooperation എല്ലാവർക്കും benefit ചെയ്യുമ്പോൾ പോലും rational individuals എന്തുകൊണ്ട് cooperate ചെയ്യാതിരിക്കാം എന്ന് കാണിക്കുന്നു. രണ്ട് suspects-നെ പ്രത്യേകം interrogate ചെയ്യുന്നു, അവർ confess ചെയ്യണോ silent ആയി തുടരണോ എന്ന് തീരുമാനിക്കണം.

Payoff matrix strategic structure പിടിച്ചെടുക്കുന്നു:

|                | Cooperate    | Defect      |
|----------------|-------------|-------------|
| **Cooperate**  | (-1, -1)    | (-3, 0)     |
| **Defect**     | (0, -3)     | (-2, -2)    |

ഓരോ player-ക്കും defect ചെയ്യാനുള്ള ഒരു dominant strategy ഉണ്ട്, ഇത് ഇരുവർക്കും suboptimal ആയ ഒരു outcome-ലേക്ക് നയിക്കുന്നു.

### Real-World Applications

Prisoner's Dilemma structure പല economic contexts-ലും കാണാം:
- Firms തമ്മിലുള്ള price competition
- Public goods provision
- Environmental protection agreements
- Arms races-ഉം military spending-ഉം

ഈ game മനസ്സിലാക്കുന്നത് market failures-ഉം regulation-ന്റെയോ cooperation mechanisms-ന്റെയോ ആവശ്യകതയും വിശദീകരിക്കാൻ സഹായിക്കുന്നു.

## Nash Equilibrium

Nash equilibrium എന്നത് ഒരു strategy profile ആണ്, അതിൽ ഒരു player-ക്കും unilateral ആയി strategy മാറ്റി അവരുടെ payoff മെച്ചപ്പെടുത്താൻ കഴിയില്ല. മറ്റുള്ളവരുടെ strategies കണക്കിലെടുക്കുമ്പോൾ ഓരോ player-ന്റെയും strategy optimal ആയിരിക്കുന്ന ഒരു stable outcome-നെ ഇത് represent ചെയ്യുന്നു.

Mathematical ആയി, $S_i$ strategy sets-ഉം $u_i$ payoff functions-ഉം ഉള്ള $i = 1, \ldots, n$ players-ക്ക്, $(s_1^*, \ldots, s_n^*)$ എന്ന strategy profile ഒരു Nash equilibrium ആകുന്നത് ഇങ്ങനെയാണ്:

$$
u_i(s_i^*, s_{-i}^*) \geq u_i(s_i, s_{-i}^*) \quad \forall s_i \in S_i, \forall i
$$

ലളിതമായ ഒരു game-ന്റെ Nash equilibria Python-ൽ compute ചെയ്യാം:

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

Pure strategy Nash equilibrium ഒന്നും exist ചെയ്യാത്തപ്പോൾ, players strategies-ൽ randomize ചെയ്യാം. ഈ mixed strategy equilibria-ക്ക് players randomize ചെയ്യാൻ തയ്യാറാകുന്ന indifference conditions ആവശ്യമായിവരുന്നു.

Mixed strategy equilibrium probabilities താഴെ കൊടുത്തിരിക്കുന്നത് solve ചെയ്ത് compute ചെയ്യാം:

```{math}
\begin{align}
p \cdot u_1(\text{Strategy 1}) &= p \cdot u_1(\text{Strategy 2}) \\
q \cdot u_2(\text{Strategy 1}) &= q \cdot u_2(\text{Strategy 2})
\end{align}
```

## Sequential Games

Sequential games-ൽ, players ഒരു specific order-ൽ move ചെയ്യുകയും മുൻപത്തെ actions observe ചെയ്യുകയും ചെയ്യുന്നു. ഈ games, game tree-യുടെ അവസാനത്തിൽ നിന്നും തുടക്കത്തിലേക്ക് work ചെയ്യുന്ന backward induction ഉപയോഗിച്ചാണ് analyze ചെയ്യുന്നത്.

പ്രധാന insight subgame perfection ആണ്: ഓരോ decision node-ലും, future play കണക്കിലെടുത്ത് players optimal ആയി choose ചെയ്യുന്നു. Rational players execute ചെയ്യാത്ത non-credible threats ഇത് ഒഴിവാക്കുന്നു.

### Stackelberg Competition

Classic ആയ ഒരു application Stackelberg competition ആണ്. ഇതിൽ ഒരു firm (leader) ആദ്യം quantity choose ചെയ്യുന്നു, മറ്റൊരു firm (follower) ഈ choice observe ചെയ്ത ശേഷം തീരുമാനിക്കുന്നു.

Leader, follower-ന്റെ reaction function മുൻകൂട്ടി കണ്ട് optimal ആയി choose ചെയ്യുന്നു:

$$
\max_{q_L} \pi_L(q_L, R(q_L))
$$

ഇവിടെ $R(q_L)$ എന്നത് leader-ന്റെ quantity $q_L$-ന് follower നൽകുന്ന best response ആണ്.

ഒരു Stackelberg game numerically solve ചെയ്യാം:

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

Games repeatedly play ചെയ്യുമ്പോൾ, reputation-ഉം punishment mechanisms-ഉം വഴി പുതിയ equilibria ഉടലെടുക്കുന്നു. Players മതിയായി patient ആണെങ്കിൽ പല outcomes-ഉം equilibria ആയി sustain ചെയ്യാം എന്ന് Folk Theorem കാണിക്കുന്നു.

ആരെങ്കിലും cooperative path-ൽ നിന്നും deviate ചെയ്താൽ non-cooperative play-ലേക്ക് തിരിച്ചുപോകും എന്ന threat ആണ് പ്രധാന mechanism. ഇതിന് ഇവ ആവശ്യമായിവരുന്നു:

1. Players future payoffs-നെ value ചെയ്യണം (discount factor $\delta < 1$)
2. Deviations observable ആയിരിക്കണം
3. Punishment threats credible ആയിരിക്കണം

### Trigger Strategies

സാധാരണമായ ഒരു enforcement mechanism grim trigger strategy ആണ്: ആരെങ്കിലും defect ചെയ്യുന്നത് വരെ cooperate ചെയ്യുക, പിന്നീട് എപ്പോഴും defect ചെയ്യുക. ഈ strategy cooperation sustain ചെയ്യുന്നത് ഇങ്ങനെയാണ്:

$$
\frac{1}{1-\delta} \cdot \pi_{\text{coop}} \geq \pi_{\text{deviate}} + \frac{\delta}{1-\delta} \cdot \pi_{\text{punish}}
$$

ഇടത് വശം perpetual cooperation-ൽ നിന്നുള്ള payoff ആണ്, വലത് വശമാകട്ടെ deviate ചെയ്തതിന് ശേഷം perpetual punishment വരുന്നതിൽ നിന്നുള്ള gain ആണ്.

## Exercises

1. **Computing Equilibria**: താഴെ കൊടുത്തിരിക്കുന്ന game-ന് എല്ലാ Nash equilibria-യും (pure ഉം mixed ഉം) കണ്ടെത്തുക:
   
   |           | Left  | Right |
   |-----------|-------|-------|
   | **Up**    | (3,2) | (1,3) |
   | **Down**  | (0,1) | (2,4) |

2. **Repeated Games**: Grim trigger strategies ഉപയോഗിച്ച് infinitely repeated Prisoner's Dilemma-യിൽ cooperation sustain ചെയ്യാൻ ആവശ്യമായ minimum discount factor കണക്കാക്കുക.

3. **Sequential Games**: Firms ഒരു market-ൽ enter ചെയ്യണോ എന്ന് sequential ആയി തീരുമാനിക്കുന്ന ഒരു three-stage entry game-ൽ subgame perfect equilibrium-ന് വേണ്ടി solve ചെയ്യുക.

4. **Mixed Strategies**: Exercise 1-ന് വേണ്ടി നിങ്ങൾ compute ചെയ്ത mixed strategy equilibrium, ഇരു players-നെയും അവരുടെ pure strategies-ക്കിടയിൽ indifferent ആക്കുന്നു എന്ന് verify ചെയ്യുക.