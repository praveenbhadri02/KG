# Energy Research Knowledge Graph (Neo4j Aura)

Builds a knowledge graph from `energy_research_corpus/corpus/*.txt` and answers the ten questions in `queries.txt`.

## Run
```
pip install -r requirements.txt
copy .env.example .env      # fill in the Aura credentials (never committed)
python build_graph.py       # idempotent load (MERGE); --reset removes only this project's nodes
python run_queries.py       # writes results/query_results.csv
```

## Layout
| Path | Purpose |
|---|---|
| `kg/graph_data.py` | Facts extracted from the corpus (entities, relationships) |
| `build_graph.py` | Loads them into Neo4j with uniqueness constraints |
| `kg/queries.py` | One Cypher query per question |
| `run_queries.py` | Runs the queries, records answers and timings |
| `results/query_results.csv` | Deliverable: Question, Cypher Query, Answer, Execution Time |

## Notes
- Transport: uses Bolt when port 7687 is reachable, otherwise Aura's HTTPS Query API (port 443).
- Execution Time is client-side wall time per query, including network round trip.
- Sofia Lindqvist's HIT affiliation is inferred (supervised by Solberg; linked to HIT in the 2025 paper).
- "Collaboration hops" count shared participation in a project; funding agencies are excluded.
- "Co-author country" uses the author's affiliation at the time of each paper.
