# Actor Node Graph
 
A Rust graph-analysis project that models IMDb actors as a network (actors are nodes, and two actors are connected if they appeared in the same title). It uses that network to answer three questions:
 
- Which actors are the **most connected**?
- Do some actors act as **bridges** between groups that usually work together (e.g. Marvel actors, or Bollywood and Hollywood)?
- How many steps separate two actors from very different film industries?
> Built as the final project for DS 210 (Programming for Data Science) at Boston University.
 
## Highlights
 
- Processed **~18 million rows** of IMDb's non-commercial datasets
- Wrote **degree, closeness, and betweenness centrality** from scratch
- Found the shortest path between actors with **Dijkstra's algorithm**
- Unit tests for the graph functions
**Fun result:** Amitabh Bachchan (Bollywood) and Timothée Chalamet (Hollywood) are only **2 connections apart**.
 
## Dataset
 
Source: [IMDb Non-Commercial Datasets](https://developer.imdb.com/non-commercial-datasets/), specifically `name.basics.tsv.gz` and `title.principals.tsv.gz`.
 
Preprocessing (Python/Pandas):
 
1. Filtered `title.principals` down to actors only.
2. Extracted (actor ID, title ID) pairs.
3. Joined with `name.basics` to attach actor names.
4. Dropped rows with missing values.
The result has three columns (`actor_id`, `name`, `title_id`) and about 18M rows. The cleaned data is available [here](https://drive.google.com/drive/folders/19m2i4oNvlRNCXcam_27tmER4-NjLS0Uw?usp=drive_link).
 
## Project structure
 
```
src/
├── main.rs     # runs the analyses and prints the results
├── parser.rs   # reads the TSV into Record structs
└── graph.rs    # graph construction, centrality, shortest paths, and tests
```
 
| Function | What it does | Method |
|---|---|---|
| `degree_centrality` | Counts each actor's direct co-stars | Neighbor count |
| `closeness_centrality` | Measures how quickly an actor can reach everyone else | Dijkstra |
| `betweeness_centrality` | Finds actors that bridge separate clusters | BFS |
| `shortest_path_length` | Gives the degrees of separation between two actors | Dijkstra |
 
## How it works
 
1. `parser.rs` loads the cleaned dataset.
2. `build_graph` creates an undirected actor graph and maps each actor ID to a node index.
3. It runs every centrality measure on:
   - a subgraph centered on Priyanka Chopra Jonas
   - a random sample of 500 actors
4. It computes the shortest path from Amitabh Bachchan to Timothée Chalamet.
Sample output is in [`output.txt`](output.txt).
 
## Run it
 
```bash
# put the cleaned dataset in the project root first
cargo run --release
cargo test
```
 
