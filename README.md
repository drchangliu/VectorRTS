# The VectorRTS game

VectorRTS is a simple (likely the simplest) turn-based strategy game with a very small action space (4) and an observation space that is easy to visualize. It is small enough for a human to work out a good strategy or to apply a reinforcement learning algorithm by hand.

#### Definitions

```
M = 64 (map size, or magnitude of the vector)
S = 2 (attacker speed advantage factor)
E2M = 20% (Economy to military conversion rate)
B = 20% (Economy booming rate)
MP: military power
A = 80% (Attacker advantage factor)
P: Position
```
$P_{R0}$: Position of the right player at time 0.

##### Observation space (size: M)

There is a vector map of size `M`. Each cell can be: 
* Empty
* Resources (with an integer number representing the number of resource units)
* A player (There are two players, $L$ and $R$. Each player has two properties: economy and military. Each player starts with `e2m0`.)

##### The action space (size: 4)

VectorRTS is a turn-based game. Player $L$ goes first. At each turn, a player can:
* Go towards the opponent (the speed is `S` cell/turn)
* Go away from the opponent (the speed is 1 cell/turn) - This is to make sure the attacker can always catch up with the player running away.
* Rush (Convert one e or `E2M %` of e, whichever is greater, to m)
* Boom (Increase economy by 1 or by `B %`, whichever is greater)

##### The game environment (the step function) 

If a player passes through or reaches a cell with resources, all resource units are converted to economy units for the player.

```math
MP_L = m \cdot (1 + \frac{P_L - \frac{P_{R0} - P_{L0}}{2}} {M}) \cdot A
```

```math
MP_R = m \cdot (1 + \frac{\frac{P_{R0} - P_{L0}}{2} - P_R  }{M}) \cdot A
```

If a player passes through or collides with the other player, the player with a greater $MP$ wins. Ties are possible. 

##### The maps

Here are a few example maps. Develop an RL algorithm that will outperform a Random AI, a Rush AI, a Boom AI (boom 80%, rush 20%), and other AI agents from the rest of the class.

```
vector_map1 = [ "", "", 3, "", "e2m0", "", 3, "", "", "", 3, "", "e2m0", "", 3, "", "" ]
vector_map2 = [ "", 5, "", 3, "", "e2m0", "", "",  "", "", "", "", "", "", "e2m0", "", 3, "", 5, "" ]
# If the length is not M, insert empty cells to both ends to make it M.
```
