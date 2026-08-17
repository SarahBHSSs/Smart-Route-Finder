# 🗺️ Smart Route Finder

A personal project where I implemented Dijkstra's algorithm from scratch and used it to compute real shortest-path routes on an actual city map (Tunis), with an interactive interface to click and see the route appear.

I built this to really understand how Dijkstra's algorithm works past the classroom examples with 6 nodes and a whiteboard. Here, the algorithm runs on a real road network pulled live from OpenStreetMap, thousands of nodes and edges, and the whole thing is wrapped in a map you can actually click on.

## Why I made this

Most of the Dijkstra examples I found online or in class use tiny toy graphs, which is fine for understanding the theory but doesn't really show what happens when you run it on real, messy data. I wanted to:

- implement the algorithm myself (priority queue, distance relaxation, predecessor tracking) instead of just calling `networkx.shortest_path()`
- test it on an actual road network instead of a diagram
- make it interactive, so I (or anyone) could click two points and see the result instead of reading numbers in a terminal


## Architecture 

```
+------------------+      +----------------------+      +----------------------+      +--------------------+
|  OpenStreetMap   | ---> |   Road network graph  | ---> |  Dijkstra (manual)   | ---> |  Interactive map    |
|  (via OSMnx)     |      |   (networkx graph)    |      |  shortest path       |      |  + route plot       |
+------------------+      +----------------------+      +----------------------+      +--------------------+
   raw map data              weighted graph              custom algorithm             user-facing result
```

## How it works

1. The user clicks two points on the map (origin and destination).
2. Each click is snapped to the nearest node in the road graph via `osmnx.distance.nearest_nodes`.
3. `dijkstra_manual()` explores the graph with a binary heap (`heapq`), relaxing distances edge by edge until the destination is reached.
4. The path is reconstructed by walking back through predecessor pointers.
5. The route is drawn on the interactive map in red; the two endpoints are resolved to addresses via reverse geocoding (Nominatim).
6. A second, static plot shows the immediate neighborhood around the route with edge lengths labeled, for inspecting the algorithm's decisions.

## Tech stack

- **Python** : core language
- **OSMnx** : download and model real-world street networks from OpenStreetMap
- **NetworkX** : graph data structure
- **Dijkstra's algorithm (custom implementation)** : shortest-path search with a binary heap
- **ipyleaflet** : interactive map rendering inside Jupyter
- **Matplotlib** : static visualization of the computed route
- **Geopy / Nominatim** : reverse geocoding (coordinates → addresses)
