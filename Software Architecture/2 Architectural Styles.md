# What Is an Architectural Style?
Architecture Style =  Component/Connector vocabulary, Topology, Semantic Constraints 
# Five Classical Style Families
## Data Flow
### Batch Sequential
Read All Records ➡ Process All ➡ Generate Outputs

Characteristics 
- Components: independent programs 
- Connectors: files / media 
- Entire dataset per step 
- No concurrency 
- High latency, simple logic

### Pipe-and-Filter
Source ➡ Filter A ➡ Filter B ➡ Sink

Filter Operations 
- Enrich — Add information 
- Refine — Remove / condense 
- Transform — Change format 
- Decompose — Split stream 
- Merge — Combine streams 
Filter Types: 
- Active (pull/push data) 
- Passive (data-driven)
