# Transfer-Learning-in-NOC-routing
applying network routing technique for a Network-on-chip using GANs with Transfer learning 

---

## Dataset for training

[dataset_into_gan](https://docs.google.com/spreadsheets/d/1a2HSS-BV8j_RIXT-vZ5zJlmoL3FuS6us11S5r1XgxaI/edit?usp=sharing)

### Router Features
For each router in the network:
- **X-coordinate (`router.x`)**: This represents the router's position along the X-axis in the network topology. It's a spatial feature that helps in understanding the router's location relative to others.
- **Y-coordinate (`router.y`)**: Similarly, this represents the router's position along the Y-axis. Together with the X-coordinate, it defines the router's precise location in the network.


These spatial features are crucial for understanding the geometry of the network and how data packets might be routed from source to destination.

### Edge Features
For each edge in the network (i.e., the connection between two routers):
- **Length of the edge (`edge.len`)**: This could represent the physical distance between two connected routers or a cost associated with transmitting data over this edge. It's an essential feature for routing algorithms, which may prefer shorter or less costly paths.
- **Load on the edge (`edge.load`)**: This indicates how much data (or how many data packets) is currently being transmitted over the edge. It's a critical feature for understanding network congestion and for routing algorithms that aim to balance load across the network.

Including these features allows the GAN to generate synthetic network states that reflect both the static topology of the network and its dynamic usage patterns.

### Data Packet Features
For the data packet being considered:
- **Current router (`data_packet.now`)**: This is the label or identifier of the router where the data packet currently resides. It's crucial for routing decisions, which depend on the packet's current location in the network.
- **Target router (`data_packet.target`)**: This is the label or identifier of the router to which the data packet is destined. Along with the current location, this defines the routing problem for this packet.
- **Size of the data packet (`data_packet.size`)**: This could influence routing decisions, especially in networks where larger packets might be split or where certain paths have size constraints.
- **Priority of the data packet (`data_packet.priority`)**: This could be used to simulate Quality of Service (QoS) features, where higher priority packets are given preferential routing.

By incorporating these features into each state vector, the GAN can learn to generate synthetic network states that realistically reflect the routing challenges in a network. This includes not just the physical topology and the dynamic load but also the varying importance and requirements of different data packets moving through the system.

## what does a single row(single state vector) denote

single state vector row denotes a snapshot of the network of where the data packets are located. If your network consists of `N` routers, then for each router, you're adding two features (`router.x` and `router.y`) to the state vector. Therefore, the first `2N` entries in a single row are dedicated to representing the positions of all routers in the network.

### example:

Imagine you have a small network with **3 routers** and **2 data packets**. Here's a simplified description:

- **Routers:**
  - Router 1: Located at (x=1, y=2)
  - Router 2: Located at (x=2, y=3)
  - Router 3: Located at (x=3, y=1)
  
- **Edges:**
  - Edge between Router 1 and Router 2: Length=5, Load=0.5
  - Edge between Router 2 and Router 3: Length=6, Load=0.3
  - Edge between Router 1 and Router 3: Length=7, Load=0.2

- **Data Packet (Example):**
  - Current location (now): Router 1
  - Target destination (target): Router 3
  - Size: 1000 (arbitrary units)
  - Priority: High (represented numerically, let's say 1)

Given this setup, let's construct a **state vector** for this data packet:

1. **Router Coordinates:**
   - Router 1's coordinates: `1, 2`
   - Router 2's coordinates: `2, 3`
   - Router 3's coordinates: `3, 1`
   
   This part of the vector will look like: `[1, 2, 2, 3, 3, 1]`
   
2. **Edge Features:**
   - Edge 1-2: Length=5, Load=0.5
   - Edge 2-3: Length=6, Load=0.3
   - Edge 1-3: Length=7, Load=0.2
   
   Adding these to the vector, we get: `[1, 2, 2, 3, 3, 1, 5, 0.5, 6, 0.3, 7, 0.2]`

3. **Data Packet Features:**
   - Since the packet is starting from Router 1 and aiming for Router 3, with a size of 1000 and high priority:
   
   Completing the vector, it becomes: `[1, 2, 2, 3, 3, 1, 5, 0.5, 6, 0.3, 7, 0.2, 1, 3, 1000, 1]`

This state vector thus represents a complete picture of the network from the perspective of this specific data packet. The first 6 numbers give you the positions of all routers in the network, the next 6 numbers give you the characteristics of all edges between these routers, and the final 4 numbers tell you about the specific data packet's current location, destination, size, and priority.
