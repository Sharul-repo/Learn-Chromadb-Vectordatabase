# 📘 ChromaDB Tutorial – README

This guide demonstrates how to use **ChromaDB** step by step with a **persistent client**, create a collection, insert documents, and query them. It also includes a simple dry-run example of how similarity search works under the hood.

---

## 🚀 Why ChromaDB?

Vector databases like **ChromaDB** are the backbone of modern AI applications such as **RAG (Retrieval Augmented Generation)**, semantic search, recommendation systems, and similarity search. They store data in the form of **embeddings** (vectors) and allow efficient retrieval based on meaning rather than exact matches.

---

## 🛠️ Steps in Code

### 1️⃣ Create a Persistent Client

A persistent client ensures your data is stored on disk permanently and not lost when the program ends.

```python
import chromadb
from chromadb.config import Settings

client = chromadb.PersistentClient(path="./chroma_db_store")
```

### 2️⃣ Create (or Get) a Collection

A **collection** is like a table where documents and embeddings are stored.

```python
collection = client.get_or_create_collection(name="sample_collection")
```

### 3️⃣ Add Documents

* **ids**: Must be a list of unique strings.
* **documents**: Must be a list of strings (or other supported datatypes).

```python
collection.add(
    ids=["doc1", "doc2", "doc3"],
    documents=[
        "Dogs are wonderful pets.",
        "Cats are independent animals.",
        "I love driving fast cars."
    ]
)
```

### 4️⃣ Query the Collection

Perform a similarity search using embeddings.

```python
results = collection.query(
    query_texts=["Tell me about pets"],
    n_results=2  # return top 2 most similar documents
)

print(results)
```

---

## 🔍 How It Works (Algorithm Dry-Run)

Let’s imagine a very simplified embedding space (2D only):

* **Dog** → \[0.12, 0.45]
* **Cat** → \[0.15, 0.50]
* **Car** → \[0.85, 0.20]
* **Query: "puppy"** → \[0.14, 0.48]

### Step 1: Compute Distances

We calculate similarity (e.g., using **cosine similarity** or **Euclidean distance**) between query and documents.

* Distance(Query, Dog) ≈ **0.03**
* Distance(Query, Cat) ≈ **0.04**
* Distance(Query, Car) ≈ **0.75**

### Step 2: Rank Results

* Closest vectors → Dog, Cat (since they are semantically close to "puppy").
* Far away → Car (not relevant).

### Step 3: Return Top `n_results`

If `n_results=2`, Chroma returns **Dog** and **Cat**.

👉 This is why **vector databases return multiple semantically similar results**, not just exact keyword matches.

---

## 📊 Key Parameters & Algorithms

* **PersistentClient** → Saves data on disk.
* **get\_or\_create\_collection** → Creates or fetches a collection.
* **add()** → Adds documents with unique IDs.
* **query()** → Searches for most similar vectors.
* **n\_results** → Controls how many results are returned.
* **Algorithms used** →

  * *Cosine Similarity*: Measures angle between vectors (common for text embeddings).
  * *Euclidean Distance*: Measures straight-line distance.
  * *HNSW (Hierarchical Navigable Small World)*: Graph-based algorithm for fast nearest-neighbor search.

---

## ✅ Summary

* Vector DBs like ChromaDB make **semantic search possible**.
* Documents are stored as **embeddings** in multi-dimensional space.
* Querying finds **nearest vectors** using similarity metrics.
* `n_results` determines how many results you fetch.

---

✨ With this knowledge, you can start building **AI-powered search and RAG systems** using ChromaDB!
