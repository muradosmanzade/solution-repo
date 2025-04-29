# Problem 1

Analyzing Equivalent Resistance Through Graph-Based Methods

## 1. Theoretical Foundation

### 1.1 Fundamental Circuit Principles  
For resistors arranged in series or parallel, the total (equivalent) resistance can be determined as follows:

Series:  
$$ R_{eq} = \sum_{i=1}^n R_i $$

Parallel:  
$$ \frac{1}{R_{eq}} = \sum_{i=1}^n \frac{1}{R_i} $$

### 1.2 Circuit Modeling with Graphs  
An electrical circuit can be modeled as a weighted undirected graph \( G(V, E) \), where:  
- **V (vertices):** represent electrical junctions or connection points  
- **E (edges):** correspond to resistive components  
- **Weights:** indicate resistance values on each edge


```python
import numpy as np
import matplotlib.pyplot as plt
import networkx as nx

def create_example_circuit():
    G = nx.Graph()
    # Add edges with resistance values
    edges = [(0,1,2), (1,2,4), (2,3,1), (0,2,3), (1,3,5)]
    G.add_weighted_edges_from(edges)
    return G

def plot_circuit(G, title="Circuit Graph"):
    plt.figure(figsize=(10, 8))
    pos = nx.spring_layout(G)
    
    # Draw edges with weights
    nx.draw_networkx_edges(G, pos, width=2)
    nx.draw_networkx_nodes(G, pos, node_color='lightblue', 
                          node_size=500)
    nx.draw_networkx_labels(G, pos)
    
    # Add edge labels (resistance values)
    edge_labels = nx.get_edge_attributes(G, 'weight')
    nx.draw_networkx_edge_labels(G, pos, edge_labels)
    
    plt.title(title)
    plt.axis('off')
    plt.show()

# Create and plot example circuit
G = create_example_circuit()
plot_circuit(G)
```

![Basic Circuit Graph](images/problem%201.0.PNG )

## 2. Algorithm Implementation

### 2.1 Simplifying Series Connections  
To perform series reduction, we target nodes that are connected to exactly two other nodes:


```python
def find_series_nodes(G):
    return [node for node in G.nodes() 
            if G.degree(node) == 2]

def reduce_series(G, node):
    neighbors = list(G.neighbors(node))
    r1 = G[node][neighbors[0]]['weight']
    r2 = G[node][neighbors[1]]['weight']
    
    # Add new combined resistance
    G.add_edge(neighbors[0], neighbors[1], 
               weight=r1 + r2)
    G.remove_node(node)
    return G

# Demonstrate series reduction
G_series = create_example_circuit()
plot_circuit(G_series, "Before Series Reduction")

node = find_series_nodes(G_series)[0]
G_series = reduce_series(G_series, node)
plot_circuit(G_series, "After Series Reduction")
```

![Series Reduction](images/problem%201.1.PNG    )

![After Series Reduction](images/problem%201.1.1.PNG    )

### 2.2 Parallel Reduction
For parallel resistors between the same nodes:

```python
def reduce_parallel(G):
    for u in G.nodes():
        for v in G.nodes():
            if u < v and G.has_edge(u, v):
                # Find parallel edges
                paths = list(nx.edge_disjoint_paths(G, u, v))
                if len(paths) > 1:
                    # Calculate equivalent resistance
                    r_eq = 0
                    for path in paths:
                        r_path = sum(1/G[path[i]][path[i+1]]['weight'] 
                                   for i in range(len(path)-1))
                        r_eq += r_path
                    r_eq = 1/r_eq
                    
                    # Remove old edges and add new equivalent
                    for path in paths:
                        for i in range(len(path)-1):
                            G.remove_edge(path[i], path[i+1])
                    G.add_edge(u, v, weight=r_eq)
    return G

# Demonstrate parallel reduction
G_parallel = nx.Graph()
G_parallel.add_weighted_edges_from([(0,1,2), (0,1,3)])
plot_circuit(G_parallel, "Before Parallel Reduction")

G_parallel = reduce_parallel(G_parallel)
plot_circuit(G_parallel, "After Parallel Reduction")
```

![Parallel Circuit](assets/prob1_a4.png)
![After Parallel Reduction](assets/prob1_a5.png)

## 3. Complete Algorithm

```python
def calculate_equivalent_resistance(G):
    while len(G.nodes()) > 2:
        # Try series reduction first
        series_nodes = find_series_nodes(G)
        if series_nodes:
            G = reduce_series(G, series_nodes[0])
            continue
            
        # Then try parallel reduction
        G_before = G.copy()
        G = reduce_parallel(G)
        if nx.is_isomorphic(G, G_before):
            break
    
    if len(G.nodes()) == 2:
        nodes = list(G.nodes())
        return G[nodes[0]][nodes[1]]['weight']
    return None

# Test with example circuits
def test_circuit(edges, title="Test Circuit"):
    G = nx.Graph()
    G.add_weighted_edges_from(edges)
    plot_circuit(G, f"{title} - Initial")
    
    R_eq = calculate_equivalent_resistance(G)
    print(f"Equivalent Resistance: {R_eq:.2f} Ω")
    plot_circuit(G, f"{title} - Final")
    return R_eq

# Example 1: Simple series-parallel
test_circuit([(0,1,2), (1,2,3), (0,2,6)], 
            "Series-Parallel Circuit")
```

![Test Circuit 1](images/problem%201.3.PNG  )

Equivalent Resistance: 8.00 Ω
![Final Circuit 1](images/problem%201.3.1.PNG   )

## 4. Analysis and Complexity

### 4.1 Time Complexity  
- **Series Reduction:** \( O(V) \) to identify eligible nodes, \( O(1) \) per reduction step  
- **Parallel Reduction:** \( O(V^2) \) for evaluating all node pairs  
- **Total Complexity:** Up to \( O(V^3) \) in the worst-case scenario for full reduction

### 4.2 Space Complexity  
- Requires \( O(V + E) \) to represent the graph structure  
- Additional \( O(V) \) memory is needed for intermediate computations and tracking

## 5. Applications and Extensions

Graph-theoretic methods for analyzing circuits offer multiple real-world applications and avenues for further development:

- **Automated Circuit Simplification:** Useful in computer-aided design (CAD) tools for electronics  
- **Educational Tools:** Interactive visualizations based on graph theory for teaching circuit concepts  
- **Complex Network Modeling:** Extending the approach to analyze impedance in AC circuits or signal flow in logical networks  
- **Integration with Simulation Software:** Embedding graph-based solvers into simulation environments like SPICE


### 5.1 Circuit Analysis Software
The algorithm can be integrated into:
- Automated circuit simplification tools
- Quick resistance calculation modules
- Real-time analysis systems
- Component parameter optimization software

### 5.2 Network Optimization
The methods extend naturally to:
- Power grid analysis and modeling
- Circuit design optimization
- Load balancing calculations 
- Network reliability assessment

### 5.3 Educational Tools
The visual nature makes it ideal for:
- Interactive circuit visualization
- Step-by-step reduction demonstrations
- Virtual circuit building exercises
- Learning progress tracking systems

The graph theory approach provides a robust foundation for these applications while maintaining mathematical rigor and computational efficiency### 5.1 Circuit Analysis Software
The algorithm can be incorporated into:
- Tools for automated circuit simplification
- Modules for quick resistance calculations
- Real-time analysis platforms
- Software for optimizing component parameters

### 5.2 Network Optimization
The methods can be naturally extended to:
- Power grid modeling and analysis
- Optimization of circuit designs
- Calculations for load balancing 
- Assessing network reliability

### 5.3 Educational Tools
Due to its visual nature, it is well-suited for:
- Interactive circuit visualizations
- Step-by-step reduction presentations
- Virtual circuit assembly exercises
- Systems for tracking learning progress

The graph theory approach offers a solid foundation for these applications while ensuring mathematical rigor and computational efficiency.


## 6. Conclusions

The graph theory approach offers:
1. A systematic method for circuit analysis
2. Clear visual representation of reduction steps
3. An extensible framework for handling complex circuits

Possible future improvements could include:
- Calculations for voltage and current
- Support for active components
- Optimization tailored to specific circuit types


