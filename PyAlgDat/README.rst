=======
CONTENT
=======
+ ABOUT PYALGDAT
+ FEATURES
+ REQUIREMENTS
+ INSTALLATION
+ EXAMPLES
+ DOCUMENTATION
+ LICENSE
+ AUTHOR
+ CHANGELOG

ABOUT PYALGDAT
==============
PyAlgDat is a collection of data structures and algorithms written in Python.
The purpose of the code is to show how many of the abstract data types (ADTs) and
algorithms being thought in Computer Science courses can be realised in Python.

My primary focus has been to write a library which presents a clear
implementation of the various data structures and algorithms and how they can
be used. This means that I have made a conscious tradeoff where clarity of the
code outweighs subtle and exotic implementation constructs.

The library has mostly been implemented as a recreational project and should
as such not be used in production code, since most of the data structures and
algorithms are already available in the standard Python library. However,
writing software that is robust, performs well, and is easy to maintain requires
knowledge of data structures and algorithms. Therefore, implementing and
experimenting with these provides valuable knowledge about the inner workings
and implementation details found in such standard libraries.

FEATURES
========
Data structures included in the library

+ Dynamic array
+ Stack
+ Queue
+ BinaryHeap

   - MinHeap
   - MaxHeap

+ LinkedList

   - Singly linked list
   - Doubly linked list

+ Partition/Union-Find
+ Graph
   - Directed
   - Undirected 
   - Directed weighted
   - Undirected weighted

Additionally, the library contains the most common algorithms and operations
needed when working with these data structures.

REQUIREMENTS
============
The library is selfcontained and does not have any external dependencies.
PyAlgDat should run on any platform with Python 2.7 or above.

INSTALLATION
============
The package can be installed using `pip <https://pypi.python.org/pypi/pip>`_

.. code-block:: shell

   $ pip install PyAlgDat

EXAMPLES
========
PyAlgDat has a collection of functional test examples which shows how the
library can be used from a client's perspective. 

Shortest path using Dijkstra's algorithm
----------------------------------------
Below is a simple example showing howto create a directed weighted graph
using PyAlgDat and how the shortest path in this graph can be found using
Dijkstra's algorithm.

.. code-block:: python

   #!/usr/bin/env python

   """
   Test of Dijkstra's algorithm for a Directed Weighted Graph.
   """

   def create_graph():
        """
   	Creates a Directed Weighted Graph
	"""
    	# Create an empty directed weighted graph
    	graph = DirectedWeightedGraph(7)

    	# Create vertices
    	vertex0 = UnWeightedGraphVertex(graph, "A")
    	vertex1 = UnWeightedGraphVertex(graph, "B")
    	vertex2 = UnWeightedGraphVertex(graph, "C")
    	vertex3 = UnWeightedGraphVertex(graph, "D")
    	vertex4 = UnWeightedGraphVertex(graph, "E")
    	vertex5 = UnWeightedGraphVertex(graph, "F")
    	vertex6 = UnWeightedGraphVertex(graph, "G")

    	# Add vertices
    	graph.add_vertex(vertex0)
    	graph.add_vertex(vertex1)
    	graph.add_vertex(vertex2)
    	graph.add_vertex(vertex3)
    	graph.add_vertex(vertex4)
    	graph.add_vertex(vertex5)
    	graph.add_vertex(vertex6)

    	# Add edges
    	graph.add_edge(vertex0, vertex1, 7)   # ( A <- B, 7 )
    	graph.add_edge(vertex1, vertex2, 2)   # ( B <- C, 2 )
    	graph.add_edge(vertex1, vertex6, 3)   # ( B -> G, 3 )
    	graph.add_edge(vertex2, vertex3, 2)   # ( C -> D, 2 )
    	graph.add_edge(vertex2, vertex6, 4)   # ( C -> G, 4 )
    	graph.add_edge(vertex3, vertex4, 5)   # ( D -> E, 5 )
    	graph.add_edge(vertex3, vertex6, 1)   # ( D -> G, 1 )
    	graph.add_edge(vertex4, vertex5, 6)   # ( E -> F, 6 )
    	graph.add_edge(vertex5, vertex0, 1)   # ( F <- A, 1 )
    	graph.add_edge(vertex5, vertex6, 4)   # ( F <- G, 4 )
    	graph.add_edge(vertex6, vertex0, 7)   # ( G -> A, 7 )
    	graph.add_edge(vertex6, vertex4, 1)   # ( G -> E, 1 )

    	#       B--<--7--<--A
    	#      / \         / \
    	#     /   \       /   \
    	#    2     3     7     1
    	#   /       \   /       \
    	#  /         \ /         \
    	# C-->--4-->--G--<--4--<--F
    	#  \         / \         /
    	#   \       /   \       /
    	#    2     1     1     6
    	#     \   /       \   /
    	#      \ /         \ /
    	#       D-->--5-->--E

    	return graph

   if __name__ == "__main__":

      # Make it possible to use py_alg_dat without performing
      # an installation. This is needed in order to be able
      # to run: python dijkstra_test.py, without having
      # performed an installation of the package. The is
      # neccessary due to Python's handling of relative
      # imports.
      if __package__ is None:
        import sys
        from os import path
        sys.path.append(path.dirname(path.dirname(path.abspath(__file__))))
        from py_alg_dat.graph import DirectedWeightedGraph
        from py_alg_dat.graph_vertex import UnWeightedGraphVertex
        from py_alg_dat.graph_algorithms import GraphAlgorithms
      else:
        from ..py_alg_dat.graph import DirectedWeightedGraph
        from ..py_alg_dat.graph_vertex import UnWeightedGraphVertex
        from ..py_alg_dat.graph_algorithms import GraphAlgorithms

      # Create the graph
      GRAPH = create_graph()
      # Run Dijkstra starting at vertex "A"
      TABLE = GraphAlgorithms.dijkstras_algorithm(GRAPH, GRAPH[0])
      # Find the edges in the Spanning Tree and its total weight
      SPANNING_TREE_EDGES = set()
      SPANNING_TREE_WEIGHT = 0
      for i in xrange(len(TABLE)):
        entry = TABLE[i]
        if entry.predecessor != None:
            edge = entry.edge
            SPANNING_TREE_EDGES.add(edge)
            SPANNING_TREE_WEIGHT += edge.get_weight()
      print "Edges in Spanning Tree: " + str(SPANNING_TREE_EDGES)
      print "Weight of Spanning Tree: " + str(SPANNING_TREE_WEIGHT)


Minimum spanning tree using Kruskal's algorithm
-----------------------------------------------
Below is a simple example showing howto create an un-directed weighted graph
using PyAlgDat and how the minimum spanning tree of this graph can be found
using Kruskal's algorithm.

.. code-block:: python

   #!/usr/bin/env python

   """
   Test of Kruskal's algorithm for a UnDirected Weighted Graph.
   """

   def create_graph():
       """
       Creates an UnDirected Weighted Graph
       """
       # Create an empty undirected weighted graph
       graph = UnDirectedWeightedGraph(7)

       # Create vertices
       vertex1 = UnWeightedGraphVertex(graph, "A")
       vertex2 = UnWeightedGraphVertex(graph, "B")
       vertex3 = UnWeightedGraphVertex(graph, "C")
       vertex4 = UnWeightedGraphVertex(graph, "D")
       vertex5 = UnWeightedGraphVertex(graph, "E")
       vertex6 = UnWeightedGraphVertex(graph, "F")
       vertex7 = UnWeightedGraphVertex(graph, "G")

       # Add vertices
       graph.add_vertex(vertex1)
       graph.add_vertex(vertex2)
       graph.add_vertex(vertex3)
       graph.add_vertex(vertex4)
       graph.add_vertex(vertex5)
       graph.add_vertex(vertex6)
       graph.add_vertex(vertex7)

       # Add edges
       graph.add_edge(vertex1, vertex2, 7)    # (A - B, 7)
       graph.add_edge(vertex1, vertex4, 5)    # (A - D, 5)
       graph.add_edge(vertex2, vertex3, 8)    # (B - C, 8)
       graph.add_edge(vertex2, vertex4, 9)    # (B - D, 9)
       graph.add_edge(vertex2, vertex5, 7)    # (B - E, 7)
       graph.add_edge(vertex3, vertex5, 5)    # (C - E, 5)
       graph.add_edge(vertex4, vertex5, 15)   # (D - E, 1)
       graph.add_edge(vertex4, vertex6, 6)    # (D - F, 6)
       graph.add_edge(vertex5, vertex6, 8)    # (E - F, 8)
       graph.add_edge(vertex5, vertex7, 9)    # (E - G, 9)
       graph.add_edge(vertex6, vertex7, 11)   # (F - G, 11)
       return graph

   if __name__ == "__main__":
       # Make it possible to use py_alg_dat without performing
       # an installation. This is needed in order to be able
       # to run: python kruskal_test.py, without having
       # performed an installation of the package. The is
       # neccessary due to Python's handling of relative
       # imports.
       if __package__ is None:
       	  import sys
          from os import path
          sys.path.append(path.dirname(path.dirname(path.abspath(__file__))))
          from py_alg_dat.graph import UnDirectedWeightedGraph
          from py_alg_dat.graph_vertex import UnWeightedGraphVertex
          from py_alg_dat.graph_algorithms import GraphAlgorithms
       else:
	  from ..py_alg_dat.graph import UnDirectedWeightedGraph
          from ..py_alg_dat.graph_vertex import UnWeightedGraphVertex
          from ..py_alg_dat.graph_algorithms import GraphAlgorithms

       # Create the graph
       GRAPH = create_graph()
       # Run Kruskal's algorithm
       MST = GraphAlgorithms.kruskals_algorithm(GRAPH)
       print MST

The above examples -and others can be found in the 'examples' folder in
the PyAlgDat directory.

DOCUMENTATION
=============
The PyAlgDat API contains Docstrings for all classes and methods. Additional
documentation about the library can be found in the 'docs' folder in the
PyAlgDat directory.

The full documentation is at http://pyalgdat.readthedocs.org/en/latest/.

LICENSE
=======
PyAlgDat is published under the MIT License. The copyright and license are
specified in the file "LICENSE.txt" in the PyAlgDat directory and shown
below.

AUTHOR
======
Brian Horn, trycatchhorn@gmail.com

