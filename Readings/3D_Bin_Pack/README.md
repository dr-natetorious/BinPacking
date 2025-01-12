# 3D Binpack

Multi-dimensional bin-packing occurs across industries, such as shipping and logistics, where different shaped `boxes` are sorted into the minimal `bins` that's total `weight` remains under a `limit`.

## Smart shipment (2020)

Y. Harrath, M. Aljassim and L. M. Anees, "Smart shipment: An efficient algorithm for packing three-dimensional bins," 3rd Smart Cities Symposium (SCS 2020), Online Conference, 2020, pp. 203-208, doi: 10.1049/icp.2021.0907. [IEEE](https://ieeexplore.ieee.org/document/9545694). [pdf](Harrath_2020.pdf).

The authors provide an efficient method to place boxes within 3D bins, where rotation is available. By re-orienting the box you can more densely pack the container. 

> The proposed solution is composed of two main parts called **setting boundaries** and **filling remaining spaces** to fill a single bin with a number of boxes. The two parts of the algorithm will be repeated until the last box is packed successfully inside a bin. 

The researchers constrain the problem to:

- A `bin` is defined has dimensions (length, width, and depth); assumed decreasing values.
- A `box` has dimensions (length, width, and depth); order is not required.
- A `box-type` maps an identifier to known LWD-tuple.

### Evaluated Algorithms

- The `First Fit Decreasing` and `The Best Fit` are greedy algos often appear because of their fast runtime reaching *an optimal*. By examing the ratio of `sum(used_space)` versus `sum(wasted_space)`, you can compare different solutions produced. 

- The `bin-pack3d` method uses a depth-first search to recursivly find optimal combinations. Researchers compared various versions and found that *sorting the boxes in their non-increasing volumes and then branching them into pairs. The relative position of the largest boxes was obtained in the beginning of the algorithm and infeasibility could be known at the earliest*. 

- The `Layer-Based Heuristic` divides the bin into independent `layers`. By considers the volume, weight this approach substantually reduced the number of bins and improved runtime. Previous researchers didn't consider reorientation or mixed box types.

- The `Multi objective 3D Bin` algo that s focusing on simultaneous objectives that mainly concentrates on efficiency packing and balancing the
weight of the bins. The only drawback faced by that algorithm was the large amount of wasted space in the bins which could not be filled. Similarly, using `Particle-Swarm Optimiziton`, researchers demonstrate methods for suitable placement (not minimum set).

### Setting boundaries

This operations places homogenous boxes across the layeres and determining the unused space. The most efficient usage comes from rotating by shifting the LWD-tuple (e.g., `xyz` > `zxy`) then **calculating how many potential boxes might fit and selecting the maximum orientation**.

Next, the task aligns the potential boxes along the length (largest-dimension), attempting to populate the row. 

![form_layer.png](form_layer.png)

This operation repeats for k-rows until the layer is best populated. 

![form_depth.png](form_depth.png)

### Filling Gaps

After completing every placement there will be gaps. These are populated by selecting the `unused-space` (see red) and iteratively repeating the process. Since the placement targets the dimensions in decending order (length > width > depth); there's only one `inner-bin` to handle.

![form_waste.png](form_waste.png).

You must start by selecting the `box-type` with the maximum length that fits. Often a shortcut exists by first rotating the `box` to have decending dimensions (length > width > depth) before checking. 

## An Improvement of a Heuristic Algorithm for 3D Bin-Packing Problem (2024)

S. Krisadee and W. Jindaluang, "An Improvement of a Heuristic Algorithm for 3D Bin-Packing Problem," 2024 28th International Computer Science and Engineering Conference (ICSEC), Khon Kaen, Thailand, 2024, pp. 1-6, doi: 10.1109/ICSEC62781.2024.10770638. keywords: {Computer science;Three-dimensional displays;Shape;Heuristic algorithms;Memory management;3D bin-packing problem;spatial layer-based heuristic;Three-Stage Layer-Based Heuristic (TSLBH)}. [IEEE](https://ieeexplore.ieee.org/document/10770638). [pdf](An_Improvement_of_a_Heuristic_Algorithm_for_3D_Bin-Packing_Problem.pdf).

The authors propose a procedure using a three-staged version of [Y. Harrath's 3D bin-packing algorithm](Harrath_2020.pdf). This approach reduces (bins, processing time, and memory) decreased to (39%, 58%, and 64%) for large bin counts.

> There are numerous approaches to solving the 3D binpacking problem. One of the basic concepts for solving this problem is the Layer-Based Heuristic approach, as described by [5]. This method uses heuristics to find solutions in each layer within the bin, as illustrated in Fig.2. The approach involves selecting the appropriate type and number of boxes for each layer until no more boxes can fit, indicating the completion of packing for that bin. 

![layers.png](layers.png)

### The issue of using the maximum volume pattern

> As seen in Fig.6, selecting a layout with the largest volume only might notnachieve the goal of utilizing the fewest number of bins. There may be only one type of box in the pattern with the maximum volume. As shown in this figure, a pattern with the maximum volume indicates that we should pack a box of type 2 with an amount of 147. Meanwhile, in a configuration, we have 3 types of boxes with amounts 17, 7, and 76, respectively.

Harrath's algorithm begins the **setting bounds** operation by placing homogenous boxes. This is efficient when the `count(box-type)` is low, but more likely to have gaps and changing patterns is high. 

The authors address this through a memorization optimization technique during the layered-heuristic's second and third stage. 

![optimization.png](optimization.png)
