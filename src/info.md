# Ant as information vehicle
* every ant has an information buffer (scalar)
* during waking, the walking distance linearly increases the information buffer
* when two ants meet, both ants exchange information (scalar) so decreasing the information buffer for both ants
* Each ant has two states:
* At the beginning, the ant information buffer is 0, then the ant is in "exploration" state. In this state, the ant is walking and increasing the information buffer.
* When the information buffer is full, the ant is in "gossip" state. In this state, the ant try to find another ant to exchange information with. 
* For example, when A is in the gossip state, it will try to find (approach) close ants (within a certain distance) and exchange information with them.
* If A finds another ant B, then A and B exchange information (scalar) and decrease their information buffer. 
* If A's information buffer is still not empty, A will continue to search for another ant C to exchange information with. A will not exchange information with B again, because A already exchanged information with B. 
* When A's information buffer is empty, A will go back to the exploration state and start walking again. A will clean the list of ants it has already exchanged information with.

# visual angle
Now make a large revision
Each ant has its visual angle (scalar). Ants can only see other ants in their visual angle.
This is angle but not the distance, like 60 degrees.
This is important. When ants in the gossip state, they will only approach other ants that are within their visual angle to exchange information.
