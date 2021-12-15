# ChristofidesAlgorithm

Project for CS 5800 Algorithm Course.

## CONTEXT: 

Whenever I visit a new city for tourism, the first thing I do is to make a list of places that I am interested in. These places can be restaurants, beaches, or places with historic significance. Browsing the internet for these places is fun, but the biggest hurdle is selecting a hotel that effectively minimizes my travel time to all my places of interest. The less I have to travel, the greater the time and cost savings. The way I do this right now is by opening Google Maps, putting in Google Pins to all the places of interest, and roughly approximating the area that will be close to these locations. Now I go to a hotel booking site and select the area I had approximated earlier and check all the hotels out and pick the one I like the most. The hotel selected is not necessarily the best hotel from which I can visit all the locations optimally as it’s based on rough estimates. The question for me is: How do I select the hotel whose location gives me the most optimal route to visit all the places of interest for me. I am passionate about traveling and solving this problem will allow me to apply the solution every time I travel.  

## ANALYSIS: 

### Implementation of the algorithm:
It becomes difficult at times to select the best hotel from the list of given hotels that can give us the best optimal path to cover all the nearby locations we need to visit and return back to the hotel. As a part of this problem, we are going to create an efficient algorithm that will help us to select the best optimal hotel which would reduce our total travel cost. Our problem statement is basically a travelling salesman problem with little modification to it. Christofides algorithm is a well known approach to solve TSP and also an esteemed algorithm in the field of theoretical Computer Science to find the shortest route from a given set of points. Hence, we will be using Christofides algorithm on the top of the given hotel points to find the hotel that will optimally reduce our total travel cost. Having that said, we will demonstrate you through Christofides algorithm as we will apply Christofides to all the hotels. So let’s get started by understanding the Christofides algorithm and its time complexity.

Christofides Algorithm: A multistep algorithm that guarantees its solution to the TSP within 3/2 of the optimal solution was created by Nicos Christofides in 1976. This approach is better than any of the exact solution approach as the upper bound of the Christofides algorithm is nearly O(V2logV) as the running time and the complexities vary based on the running time of the components of the algorithm and also to the input provided as it is a multistep algorithm. Here is the pseudo code of the Christofides Algorithm with the steps as follows:
1. Create a minimum spanning tree from the given set of vertices to visit.
2. Find the odd degree vertices
3. Apply a perfect matching algorithm to connect odd degree vertices.
4. Apply Eulerian cycle on the edges formed from the previous step to visit all the vertices through the shortest path possible.
5. Apply Hamiltonian cycle on the edges from the Eulerian step to take shortcuts and form a minimum cost path that connects all the points.
6. Calculate total distance by adding the weights of the edges involved in the hamiltonian cycle.

### Step 1: Creating a MST by Prim’s Algorithm
Given a graph, the first step of the Christofides algorithm is to construct an undirected minimum spanning tree (MST) of a graph. Minimum spanning tree connects all the vertices of the graph through a subset of edges, resulting in the lowest cost of the edges to share a common path between all the nodes of the graph without forming a cycle. 

### For forming a minimum spanning tree, we will use the Prim’s algorithm. The Prim’s algorithm is a greedy algorithm and it operates by building the minimum spanning tree one vertex at a time, from a random starting vertex wherein at each step we add the cheapest possible edge from the tree to the other vertex. Prim’s algorithm is very efficient and the running time of it is O(E + |V|log|V|), where E is the number of edges in a graph and V is the total number of vertices in a given graph. As TSP works on a complete graph, here we consider E = V(V - 1). If we upper bound the total time complexity for this step, then the total time complexity will be O(V2 - V + VlogV) = O(V2).
 
### Step 2: Finding odd degree vertices and apply perfect matching to join those vertices
The next step of the Christofides algorithm is to find all the odd degree vertices in the MST and calculate all the vertices having odd degree. The odd degree vertices are the ones which have an odd number of edges connected to it. The basic idea behind doing this is, as the MST does not form a cycle and we want to traverse across all the vertices in a single cycle without repeating the vertices and hence we add the edges to MST to make this traversal possible. 

As we already have edges from the previous step, we will just iterate over it to get the count of every vertex and store it in a map. If we encounter that the value is already present in the map then we will increase the current count and if not then we will initialize it at 1 with a running time of O(E) where E is the number of edges. Here, we have already applied MST in the previous step and every MST has V - 1 edges to connect all the vertices and so the running time for this step will be O(V - 1) = O(V) where V is the number of vertices. After getting the frequency counts of all the vertices, we will iterate over the map and add vertices to another data structure that has an odd count with a running time of O(V) where V is the total number of vertices. If we accumulate both the results, then finding odd degree vertices would yield us a total of O(V + V) = O(2V) = O(V) running time where V is the total number of vertices in the graph.

### Step 3: Applying perfect matching algorithm to connect all the odd degree vertices
The third step of Christofides algorithm is to create a minimum perfect matching of the odd degree vertices that we found in the second step. The necessity to join the odd vertices is to build the Eulerian multigraph. Here we explore all the possible paths between the odd degree vertices in the graph and find the combination of matchings with the smallest distance covered by iterating over them. The implementation of this part is done using the greedy approach that searches the minimum cost between the edges of the odd degree vertices. The algorithm will start with an odd vertex from the current list and find the nearest odd vertex from the current selected vertex. Then it will check and accept if the edge formation between both the odd vertices is the optimal and then remove both the vertices from the current list and search for other pairs unless there are none. The total time complexity for finding the perfect match for every odd vertex would be O(V2logV).

### Step 4: Applying Eulerian Cycle
In the next step, the combination of the matching and the MST from the initial step is done to create a Eulerian multigraph. This graph is guaranteed to have a Eulerian tour, in which it can have multiple edges between the same two nodes.

Here we form a new graph by copying over all the nodes and edges of each graph without any duplicate nodes. The basic reason behind doing this is we want all the odd degree vertices of the graph to be an even degree vertex such that we can find the TSP tour and can proceed further to the next step. 

We will now form a Eulerian tour of the multigraph. To do this we make use of an empty stack and empty path. If all the vertices are even degree vertices then we can start from any vertex or else if any two vertices have an odd number of edges then start from one of them. Checking if the current vertex has at least one adjacent vertex, if so discover that node and using backtracking discover the current vertex. We now add the current vertex to stack and remove the edge between the current vertex and neighbouring vertex and update the current vertex to neighbouring vertex. If the current vertex does not have a neighbouring vertex that does not exist then add it to the path and pop the current vertex. Repeat the process until the current vertex has no neighbours and the stack is empty.

The creation of the Eulerian cycle takes the total running time of O(V2), where V is the number of vertices.

### Step 5: Applying Hamiltonian Cycle
The last step of the algorithm is to create a Hamiltonian path by removing the already visited place and moving forward. To compute this we will walk along the Eulerian path and check each vertex if it is already visited. If we find that the vertex is already visited then we skip that vertex and move to the next vertex. In addition to this, the graph that we form satisfies the triangle inequality and following this it reduces the length of our path.

The running time of this step is O(V), where V is the total number of vertices in a graph as in this step we will visit each vertex to check if it is already visited or not.

After completing all the above steps, we will accumulate all the time complexities that we deduced from the above step and hence the total time complexity of Christofides algorithm is O(V2 + V + V2logV + V2 + V) = O(V2logV + 2V2 + V). If we upper bound this time complexity then it would be O(V2logV) of the entire Christofides algorithm where V is the number of vertices in the graph but for our use case these would be the number of locations to visit in addition to two other locations for hotels respectively. 

That being said, we deduced the time complexity of the Christofides algorithm and we will use this in our algorithm as we already know that we will use this algorithm to compute the distance from all the hotels and so the time complexity of the entire algorithm will be wrapped by the number of hotels we provide in our input i.e., O(n * V2logV) where n is the number of hotels we are interested in computing.


### Step 6 : Calculating total distance
In Step 5, we formed a Hamiltonian Cycle through which all the vertices can be visited. In order to find the total distance covered by the hamiltonian cycle, we add the sum of weights of all the edges involved in the cycle. We iterate through all the vertices involved in the Hamiltonian cycle and add the distance of the edges that are connecting them. The running time of this step will be O(V).

### References : 
Prim’s Algorithm - Introduction to Algorithm 3rd Edition Book
https://www.geeksforgeeks.org/eulerian-path-undirected-graph/ 
https://cse442-17f.github.io/Traveling-Salesman-Algorithms/
https://github.com/elunichkin/Christofides/tree/master/Java%20%2B%20visualization
