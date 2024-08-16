### 1. Requirements

- editing: `concurrent edit`, `edit offline`
- eventually the consistent state of the doc

### 2. Capacity estimates

- how many users? - 1 billion
- how many documents oer day？
- file size: eg: 1million chars in a doc -> `1MB`, smaller file might be around `10KB`
- total storage need: around `1Tb per day`

### 3. Data model

- not use RDBMS
- documents store -> noSQL: mongoDB, google clound fire store

### 4. API

- HTTP only to fetch doc: `/fetch/docId`
- HTTP is not feasible to support editing, websocket is better to communicate with server.
- `edit(), insertChar()`: websocket

**How to resolve conflicts?**

- optimistic concurrency control: 1）versioning，2）conflicts resolution
- Sync strategy:
  - `event passing`: sync by char, or by line,
  - `diff sync`: simialr to git diff.
  - Operational Transformation: [article](https://medium.com/coinmonks/operational-transformations-as-an-algorithm-for-automatic-conflict-resolution-3bf8920ea447)

### 5. Architecture
