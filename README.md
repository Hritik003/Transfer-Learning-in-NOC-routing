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

---

## Proposed method

```
1. Define the network state representation.
2. GAN for Data generation.
3. Pre-training with the synthetic data using Q-learning
4. Fine tune the Q-Learning model
```


