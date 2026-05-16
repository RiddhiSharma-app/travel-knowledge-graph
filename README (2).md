# 🧠 Travel Memories Knowledge Graph
### Turning Travel Experiences into Semantic Knowledge — A Knowledge Graph & Ontology Project

> Modelling real-world travel data using OWL ontologies, SPARQL queries, Description Logic reasoning, and an AI Chatbot interface.

---

## 🎬 Live Demo

> 📽️ Watch the full walkthrough — ontology design, SPARQL queries, and chatbot in action.

**[▶ Download / View Demo Video](./Riddhi_GENAI_Ontology_Demo.mp4)**

---

## 📊 Presentation

> The complete slide deck covering methodology, design decisions, and results.

**[📥 Download Slides (PPTX)](./KG_Riddhi_Final.pptx)**

---

## 💡 Project Overview

This project models **personal travel memories** as a Knowledge Graph using semantic web standards. The goal is to go beyond flat databases and capture rich, queryable relationships between people, places, activities, emotions, and moments — enabling both structured SPARQL queries and intelligent chatbot responses.

**Domain:** Personal travel (trips, companions, places, activities, photos, moods)  
**Tools:** OWL, SPARQL, RDF, SHACL, SWRL, GraphDB  
**Presented by:** Riddhi Sharma

---

## 🏗️ Knowledge Graph Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                  TRAVEL KNOWLEDGE GRAPH                     │
├──────────────────┬──────────────────┬───────────────────────┤
│      TBox        │      ABox        │        RBox           │
│  (Schema /       │  (Instances /    │  (Role Properties)    │
│   Ontology)      │   Real Data)     │                       │
├──────────────────┼──────────────────┼───────────────────────┤
│ Person           │ Riddhi, Amit     │ isNearBy (transitive) │
│ Place            │ London, Oxford   │ friendOf (symmetric)  │
│ Activity         │ Boat Ride,       │ bookedAccommodation   │
│ Photo            │ Museum Visit,    │ bookedTicket          │
│ Memory           │ Lighting Show    │ hasAccommodation      │
│ City             │ Leamington Spa   │   (functional)        │
│ Accommodation    │ Primark, H&M     │                       │
│ Reservation      │ Hotel records    │                       │
│ History          │ Trip data        │                       │
└──────────────────┴──────────────────┴───────────────────────┘
```

---

## 🔬 Ontology Design — TBox / ABox / RBox

### TBox — Schema Layer (Classes & Properties)

| Classes | Object Properties | Data Properties |
|---------|------------------|----------------|
| `Person` | `bookedAccommodation` — Domain: `Trip`, Range: `Accommodation` | `contactNumber` — Domain: `Contact`, Range: `String` |
| `Place` | `bookedTicket` — Domain: `Person`, Range: `Entertainment` | `Mood` — Domain: `Activity`, Range: `String` |
| `Activity` | `withPerson` — links trips to companions | |
| `Photo` | `hasAccommodation` — person's stay | |
| `Memory` | `friendOf` — social relationship | |
| `City`, `Accommodation`, `Reservation`, `History` | `isNearBy` — geographic proximity | |

### ABox — Instance Layer (Real Data)

| Class | Instances |
|-------|-----------|
| `Person` | `Riddhi`, `Amit` |
| `City` | `London`, `Oxford`, `Leamington Spa` |
| `Activity` | `Boat Ride`, `Lighting Show`, `Museum Visit` |
| `Shopping` | `Primark`, `H&M` |

### RBox — Role Properties & Axioms

| Property | Characteristic | Example |
|----------|---------------|---------|
| `isNearBy` | **Transitive** | London → Oxford → Leamington ∴ London → Leamington |
| `friendOf` | **Symmetric** | Riddhi friendOf Amit ∴ Amit friendOf Riddhi |
| `hasAccommodation` | **Functional** | A person has at most one accommodation per trip |
| `isVisitedBy` | **Inverse Functional** | |

---

## 🎯 Target Questions

These are the competency questions the knowledge graph is designed to answer:

| Category | Question |
|----------|----------|
| 🗺️ Travel | Which city did Riddhi visit in 2024? |
| 🏨 Booking | Which accommodation did the person book? |
| 😊 Mood | Which activities made the person feel excited or relaxed? |
| 👥 Social | With whom did Riddhi travel on the trip? |
| 📸 Photos | Which photos show Amit at a place marked as 'memorable'? |
| 🔍 Comparison | Which companions gave different reviews for the same place? |
| 💭 Emotion | Which place triggered both joy and nostalgia across visits? |
| 🍽️ Reviews | Which restaurants received a positive review from Riddhi in London? |

---

## 📋 SPARQL Queries

**Who did Riddhi travel with?**
```sparql
PREFIX : <http://www.semanticweb.org/pf3wr/ontologies/2025/6/riddhi_trip/>
SELECT ?trip ?companion
WHERE {
  ?trip rdf:type :Trip .
  ?trip :withPerson :Riddhi .
  ?trip :withPerson ?companion .
  FILTER(?companion != :Riddhi)
}
```

**Which accommodation was booked?**
```sparql
PREFIX : <http://www.semanticweb.org/pf3wr/ontologies/2025/6/riddhi_trip/>
SELECT ?person ?accommodation
WHERE {
  ?person :hasAccommodation ?accommodation .
}
```

**Friend relationships:**
```sparql
PREFIX : <http://www.semanticweb.org/pf3wr/ontologies/2025/6/riddhi_trip/>
SELECT ?p1 ?p2
WHERE {
  ?p1 rdf:type :Person .
  ?p2 rdf:type :Person .
  ?p1 :friendOf ?p2 .
}
```

**Class hierarchy:**
```sparql
PREFIX : <http://www.semanticweb.org/pf3wr/ontologies/2025/6/riddhi_trip/>
SELECT ?subject ?object
WHERE { ?subject rdfs:subClassOf ?object }
```

**All trips with companions (excluding self):**
```sparql
PREFIX : <http://www.semanticweb.org/pf3wr/ontologies/2025/6/riddhi_trip/>
SELECT ?person ?trip ?companion
WHERE {
  ?trip rdf:type :Trip .
  ?trip :withPerson ?person .
  ?trip :withPerson ?companion .
  FILTER(?person != ?companion)
}
```

---

## 🤖 AI Chatbot Architecture

The knowledge graph is connected to an **AI Chatbot** that accepts natural language questions and translates them into SPARQL queries against the ontology — making the graph accessible without any query language knowledge.

```
User Question (Natural Language)
        ↓
   AI Chatbot Layer
        ↓
  SPARQL Query Generator
        ↓
  Knowledge Graph (GraphDB / RDFox)
        ↓
  Results → Natural Language Answer
```

---

## 🆚 Why a Triple Store? (vs Property Graph)

| Feature | Property Graph | Triple Store (RDF) |
|---------|---------------|-------------------|
| Data Model | Nodes + edges with key-value pairs | Subject–Predicate–Object triples |
| Schema | Flexible / optional | Schema-driven (RDFS, OWL) |
| Edge Properties | ✅ Native support | ⚠️ Requires reification |
| Query Language | Cypher / Gremlin | **SPARQL** |
| Reasoning Support | Limited | ✅ Strong via OWL ontologies |
| Interoperability | Limited | ✅ High — W3C standards |
| Examples | Neo4j, JanusGraph | Apache Jena, RDFox, **GraphDB** |

> **Choice:** Triple Store (RDF/OWL) was chosen for its strong reasoning support, W3C standardisation, and ability to infer new facts via property axioms (e.g. transitivity, symmetry).

---

## ✅ OWL Concepts Demonstrated

**Property Characteristics:**

| Characteristic | Property Used |
|---------------|--------------|
| Transitive | `isNearBy` |
| Symmetric | `friendOf` |
| Asymmetric | `hasChild` |
| Reflexive | `knows` |
| Irreflexive | `isParentOf` |
| Functional | `hasAccommodation` |
| Inverse Functional | `isVisitedBy` |
| Inverse | `bookedBy` ↔ `bookedAccommodation` |

**Validation & Rules:**
- `SHACL` — shape constraints and data validation
- `SWRL` — rules for derived facts
- `Abstraction levels` — variable precision in modelling
- `External data mapping` — SubClasses, Annotations, DataTypes integrated from external sources

---

## 🗂️ Repository Structure

```
📦 travel-knowledge-graph
 ┣ 📄 README.md
 ┣ 🎬 Riddhi_GENAI_Ontology_Demo.mp4     ← Live demo video
 ┣ 📊 KG_Riddhi_Final.pptx               ← Full presentation
 ┣ 📁 ontology/
 ┃ ┗ 🧠 riddhi_trip.owl                  ← OWL ontology file
 ┣ 📁 queries/
 ┃ ┗ 📋 sparql_queries.rq                ← All SPARQL queries
 ┗ 📁 data/
   ┗ 📄 instances.ttl                    ← ABox instances (Turtle format)
```

---

## 🚀 How to Run

1. **Install GraphDB** (free edition): [graphdb.ontotext.com](https://graphdb.ontotext.com)
2. **Import the ontology** — load `ontology/riddhi_trip.owl` into a new GraphDB repository
3. **Import instances** — load `data/instances.ttl` into the same repository
4. **Run SPARQL queries** — paste any query from `queries/sparql_queries.rq` into GraphDB's SPARQL editor
5. **Watch the demo** — see `Riddhi_GENAI_Ontology_Demo.mp4` for a full walkthrough

---

## 👩‍💻 About

**Riddhi Sharma** — Mobile & Web Developer · Knowledge Graph Engineer

[![LinkedIn](https://img.shields.io/badge/-LinkedIn-0072b1?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/riddhi-sharma-softwaredeveloper/)
[![GitHub](https://img.shields.io/badge/-GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/RiddhiSharma-app)

---

<p align="center"><i>⭐ Found this interesting? Give the repo a star!</i></p>
