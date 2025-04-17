
## 🧠 **Core Behavior Logic**

### 1. **Agent Properties**

Each ant is modeled with:

* Position `(x, y)`
* Movement angle
* Speed (with Gaussian variability)
* A buffer to accumulate and offload “information”
* State (`exploration` or `gossip`)
* Partner memory buffers for interaction control

### 2. **State Transitions**

* **Exploration** : Accumulates `infoBuffer` as it moves. Transitions to gossip once buffer is full.
* **Gossip** : Seeks others to offload info. Returns to exploration once buffer empties.

### 3. **Social Interaction**

* Head-to-head encounters (based on positions ahead of ants) initiate a pause.
* If both agents are gossiping and haven’t exchanged before, they reduce infoBuffers.

### 4. **Directional Vision**

* Gossip targets must lie within a `visual cone` (150°), enforcing anisotropic perception — biologically plausible and computationally rich.

---

**Simulation Overview:**

* Population of ants explore an enclosed environment (arena).
* Each ant has two primary states:
  * **Exploration** : ants move randomly, accumulating information.
  * **Gossiping** : ants actively seek partners to exchange information.

---

**Initialization:**

* Place each ant randomly within the arena.
* Set each ant's initial state to  **exploration** .
* Each ant begins with an empty information buffer.

---

**Simulation Loop:** *(Executed at each time step)*

For each ant:

1. **Movement Behavior:**
   * If paused (after interaction), wait until pause ends.
   * If exploring:
     * Move randomly, gradually increasing internal information based on distance traveled.
     * When internal information reaches a maximum, switch state to  **gossiping** .
   * If gossiping:
     * Seek nearest ant within visual range that hasn't been previously interacted with recently.
     * Move towards this ant to initiate information exchange.
     * If no suitable ant found, wander randomly.
2. **Interactions:**
   * When two ants come into close contact head-to-head:
     * Both ants pause briefly:
       * Shorter pause if either is exploring (quick interaction).
       * Longer pause if both are gossiping (extended interaction).
     * Gossiping ants exchange information, reducing their internal information.
     * Ants record recent partners to prevent immediate repeated interactions.
3. **Information Dynamics:**
   * **Exploration → Gossiping:**
     * Accumulated exploration leads to a filled information buffer, causing transition to gossiping.
   * **Gossiping → Exploration:**
     * Successful exchanges reduce internal information; when depleted, revert to exploration.
4. **Collision and Boundary Behavior:**
   * Ants avoid overlaps through slight directional adjustments.
   * When reaching arena boundary, ants change direction to stay within bounds.
