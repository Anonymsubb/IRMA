# IRMA - Iterative Repair For Graph Matching.

IRMA is an algorithm for alignment two similar graphs from different domains (seeded graph matching).
IRMA creates a primary alignment using an existing percolation algorithm, then it iteratively repairs the mistakes in
previous alignment steps. 


### How to run
* If you have two graphs and a seed, you may represent them as networkx graphs, and use Irma for matching:

```python
import networkx as nx
from irma.irma import Irma

# Some networkx graphs with the same vertices
graph1 = nx.Graph() 
graph2 = nx.Graph()
nodes_to_match = []  # The vertices that belongs for both graphs. Just for statistics
seeds = []  # The initial information Irma - list of vertices that the algorithm will use as already aligned vertices. 

algo = Irma(graph1, graph2, seeds, nodes_to_match)
algo.run()

print(algo.M) # algo.M contains the matching of the algorithm
```

* If you have only one graph, you can simulate one more using the function generate_file_graphs in utils:

```python

from irma import utils

file_path = "./path_to_graph"
overlap = 0.6
seed_size = 100
delimiter = ","

graph1, graph2, seed, nodes_to_match = utils.generate_file_graphs(file_path, overlap, seed_size, delimiter=delimiter)
```
### DATA

| Data Name | Resource Link |
|-----------|---------------|
| Fb-pages-media("New Sites" Graph)    | [Fb-pages-media](https://snap.stanford.edu/data/gemsec-Facebook.html) |
| Soc-brightkite    | [Soc-brightkite](http://networkrepository.com/soc-brightkite.php) |
| Soc-epinions    | [Soc-epinions](http://networkrepository.com/soc-epinions.php) |
| Soc-gemsec-HU    | [Soc-gemsec-HU](http://networkrepository.com/soc-gemsec-HU.php) |
| Soc-sign-Slashdot081106    | [Soc-sign-Slashdot081106](http://networkrepository.com/soc-sign-Slashdot081106.php) |
| Deezer-europe-edges    | [Deezer-europe-edges](http://snap.stanford.edu/data/feather-deezer-social.html) |
| bio-HS-CX    | [bio-HS-CX](https://networkrepository.com/bio-HS-CX.php) |
| bio-grid-human    | [bio-grid-human](https://networkrepository.com/bio-grid-human.php) |
| bio-grid-fruitfly    | [bio-grid-fruitfly](https://networkrepository.com/bio-grid-fruitfly.php) |
| bio-grid-fission-yeast    | [bio-grid-fission-yeast](https://networkrepository.com/bio-grid-fission-yeast.php) |
| bio-DM-HT    | [bio-DM-HT](https://networkrepository.com/bio-DM-HT.php) |
| bio-yeast    | [bio-yeast](https://networkrepository.com/bio-yeast.php) |
| bio-diseasome    | [bio-diseasome](https://networkrepository.com/bio-diseasome.php) |
| bio-HS-HT    | [bio-HS-HT](https://networkrepository.com/bio-HS-HT.php) |
| bio-DR-CX    | [bio-DR-CX](https://networkrepository.com/bio-DR-CX.php) |
| bio-celegans-dir    | [bio-celegans-dir](https://networkrepository.com/bio-celegans-dir.php) |
| bio-CE-GT    | [bio-CE-GT](https://networkrepository.com/bio-CE-GT.php) |

### Notation

| Notation | Explanation |
|-----------|---------------|
| GM   | Graph matching (Graph Alignment) |
| $G$   | Graph |
| $V$   | Set of vertices of some graph $G$ |
| $E$   | Set of edges of some graph $G$ |
| $ST$   | $ST \subset V_1$ that one may limit the comparison of graphs for evaluation |
| $seed$   | $seed \subset V_1 \times V_2$ |
| $\deg(v)$   | Degree of vertex $v$ |
| $M$   | Bijection $M: V_1 \rightarrow V_2$, such that $M$ maps vertices in $V_1$ and $V_2$ |
| $p$   | A pair of vertices which each one from different graphs |
| $(u,v)$   | An edge in graph G between vertices u and v |
| $weight(M)$   | $weight(M) = \vert \{(u,v) \vert (u,v)\in E_1, (M(u),M(v)) \in E_2 \}\vert $ |
| $d_{q,v}$   | $d_{q,v}$ is the degree of a vertex $v$ in graph $q$ |
| $marks_t(p)$   | $marks_t(p)$ is defined as the number of marks that $p$ received from other pairs until time $t$, where the unit of time is the insertion of one pair to $M$ |
| $\epsilon$   | Infinitesimal $\epsilon > 0$ ,ensure that the if a pair has a higher $mark_t$ value than a different pair, it will also have a higher $score_t$ value |
| $score_t([u,v])$   | $score_t([u,v]) = marks_t([u,v]) - \epsilon \cdot \vert d_{1,u} - d_{2,v} \vert$ |
| $R$   | Set of all vertex pairs of vertices that represent the same entity in both of the graphs |
| $\delta$   | IRMA stops when $weight(M) \leq (1+ \delta)*weight(M_{i-1})$ |
| $G(n,p)$   | $G(n,p)$ Erdos-Renyi graph, defined as a graph with $n$ vertices, where every edge of the possible $\binom{n}{2}$ exists with a probability of $p$ |
| $s$   | $s$ is the fraction of edges sampled from each graph |
| $\Lambda_t$   | The number of right pairs used to spread out marks until time $t$ |
| $\Psi_t$   | The number of wrong pairs used to spread out marks until time $t$ |

[//]: # (This project meant to enable restoring all experiments done in IRMA paper.)

[//]: # ()
[//]: # (The code makes use of several packages as:)

[//]: # (networkx, random, matplotlib, math, numpy, json, pprint, time, threading, functools.)

[//]: # (All can be installed using pip.)

[//]: # ()
[//]: # (1. myQueue.py is an implementation to priority-queue that used along the algorithm.)

[//]: # ()
[//]: # (2. utils.py implements some function, mostly to initilize data sets for the algorithm based on config.json)

[//]: # ()
[//]: # (3. data.7z need to be extracted such that 'data' directory is in the same directory as IRMA.py, and inside are 6 files.)

[//]: # ()
[//]: # (4. config.json control several parameters for the algorithm:)

[//]: # (   - nodes: determine the amount nodes in the source graph. used only when use_file_graph = False.)

[//]: # (   - avg_deg: determine the amount edges in the source graph. used only when use_file_graph = False.)

[//]: # (   - seed_size: determine the size of the seed to use.)

[//]: # (   - graphs-overlap: determine the S used to sample graphs from the source graph &#40;as explained in the paper&#41;)

[//]: # (   - smooth: used during research for pretty plots.)

[//]: # (   - parallel: determine if use the parallel version.)

[//]: # (   - threads: only relevant if parallel = True)

[//]: # (   - evaluate_prints: control prints along IRMA's run.)

[//]: # (   - use_file_graph: control if use one of the graphs in 'data' as a source or either use a fully simulated graphs.)

[//]: # (   - graph_number: a value in range 0-5 to choose which graph to use among those in 'file_graphs' field.)

[//]: # (   - file_graphs: DO NOT TOUCH. list of all graphs in 'data' director.)

[//]: # (   - graphs_directory: DO NOT TOUCH. path to the file graphs.)

[//]: # (   - file_graph_name: DO NOT TOUCH. used in the code to keep the file_graph's name.)

[//]: # (   - plot_dir: used during research for printing plots.)

[//]: # (   - draw_dir: the code enable to embed the graph and print it. currently the relevant code is in comment.)

[//]: # ()
[//]: # (5. shoval.7z: If wants to use the ability of draw_dir , this file need to be extracted such that 'shoval' directory is in thr same directory as IRMA.py.)

[//]: # (then remove the comment from the relevant import in utils.py and three lines in the 'run_IRMA' function.)

[//]: # ()
[//]: # (6. IRMA.py is the main logic of our algorithm.)

[//]: # (   It starts by generating the data graphs for the algorithm &#40;lines 455-468&#41;)

[//]: # (   and then initilizes IRMA object and perform run_IRMA&#40;&#41;. This function)

[//]: # (   control the pipline of running first EWS and latter the repairing-iterations.)

[//]: # (   notice that 'evaluate' function is called after each iteration &#40;including EWS&#41;)

[//]: # (   to print the status of our current map. )

[//]: # (   The code prints by itself all measures that have been used in the paper. )

[//]: # (   the implementation of 'repairing iteration' is a bit complicated but there is no reason to fully understand it in order to run IRMA. )
