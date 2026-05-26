---
title: "A* : The Algorithm That Thinks Ahead"
datePublished: 2026-05-26T17:25:27.288Z
cuid: cmpmworcn00bw2elx6o458jsm
slug: a-the-algorithm-that-thinks-ahead

---

Have you ever wondered how a delivery app finds the fastest route through a city? Or how map navigation function?  
That where A\* search algorithm comes in. It is a shortest pathfinding algorithm between two points. Now you may ask doesn’t Dijkstra accomplish the same thing? It absolutely does. But here are few key differences between the two algorithms - 

<table style="min-width: 50px;"><colgroup><col style="min-width: 25px;"><col style="min-width: 25px;"></colgroup><tbody><tr><td colspan="1" rowspan="1"><p><strong>Dijikstra</strong></p></td><td colspan="1" rowspan="1"><p><strong>A*&nbsp;</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p>Explores all nodes uniformly in all directions</p></td><td colspan="1" rowspan="1"><p>Moves towards the goal intelligently</p></td></tr><tr><td colspan="1" rowspan="1"><p>Time consuming</p></td><td colspan="1" rowspan="1"><p>Faster and more practical</p></td></tr><tr><td colspan="1" rowspan="1"><p>Correctness guaranteed</p></td><td colspan="1" rowspan="1"><p>Correctness depends on correct estimation of heuristic</p></td></tr></tbody></table>

A\* algorithm’s core idea is to choose the next node based on the below formula:

f(n) = g(n) + h(n)

Where, g(n) = cost from the source node to the current node

            h(n) = **estimated** cost between current node and the destination node

            f(n) = estimated total cost of the complete path

h(n) is the variable indicating how closer we are getting towards are goal. This is called **heuristic**. There are several ways to calculate this heuristic value which will be discussed shortly.

Algorithm:

1.  Maintain two lists containing open nodes and closed nodes. Start with the source node A in the open list.
    
2.  Compute f(n) for all the nodes which can be traversed next to A. Keep the node having the lowest f(n) at the top of the open list. Discard A into the closed list.
    
3.  Repeat the same process by taking out the top node (suppose B) from the open list and compute f(n) for all the subsequent nodes from B.
    
4.  Stop when you have reached your destination node.
    

Above process always explores the node with lowest f(n) value. Hence, it keeps expanding on promising nodes while avoiding expensive/unnecessary/blocked paths.

Choosing the correct heuristic greatly affects the performance of the algorithm. Here are some ways you can calculate heuristic value in a 2D grid - 

<table style="min-width: 307px;"><colgroup><col style="min-width: 25px;"><col style="width: 257px;"><col style="min-width: 25px;"></colgroup><tbody><tr><td colspan="1" rowspan="1"><p><strong>Method</strong></p></td><td colspan="1" rowspan="1" colwidth="257"><p><strong>Formula</strong></p></td><td colspan="1" rowspan="1"><p><strong>Formula</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p>Manhattan distance</p></td><td colspan="1" rowspan="1" colwidth="257"><p>h(n) =&nbsp; |x1 - x2| + |y1-y2|</p></td><td colspan="1" rowspan="1"><p>Only vertical and horizontal movements are allowed</p></td></tr><tr><td colspan="1" rowspan="1"><p>Euclidean distance</p></td><td colspan="1" rowspan="1" colwidth="257"><p>h(n) = sqrt((x1-x2)^2 + (y1-y2)^2)</p></td><td colspan="1" rowspan="1"><p>All 8 direction movements are allowed</p></td></tr><tr><td colspan="1" rowspan="1"><p>Chebyshev Distance</p></td><td colspan="1" rowspan="1" colwidth="257"><p>h(n) =&nbsp; max(|x1-x2|, |y1-y2|)</p></td><td colspan="1" rowspan="1"><p>Diagonal movements are also allowed</p></td></tr></tbody></table>

Questions that come to mind - 

1.  What if the space is not a 2D grid but a graph or a weighted graph?
    
2.  What if there are obstacles in path that move dynamically?
    
3.  What happens when we overestimate the heuristic value?