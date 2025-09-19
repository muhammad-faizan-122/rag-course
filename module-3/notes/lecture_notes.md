# Introduction

## key-points
*   Traditional relational databases are slow and inefficient for large-scale semantic search involving millions of documents.
*   Vector databases are specifically designed to store, manage, and search massive quantities of vector data efficiently.
*   They are a core component of modern production-level RAG (Retrieval-Augmented Generation) systems.
*   This module will provide hands-on experience with a vector database.
*   Advanced techniques like document chunking, query parsing, and re-ranking will be covered to enhance retriever performance.

## summary
This lecture introduces the shift from information retrieval theory to production, highlighting the limitations of traditional databases for large-scale semantic search. It presents vector databases as the optimized solution, crucial for building effective RAG systems. The module will cover practical implementation and advanced techniques to improve retrieval performance.

---

# Approximate Nearest Neighbors (ANN)

## key-points
*   **The Problem with Scale:** Simple vector search, known as k-Nearest Neighbors (kNN), becomes extremely slow with large databases because it must compare a query vector to every single document vector.
*   **k-Nearest Neighbors (kNN):** This basic method calculates the distance between a query and all documents, then sorts them to find the "k" closest ones. Its performance scales linearly, making it impractical for millions or billions of documents.
*   **The Solution: Approximate Nearest Neighbors (ANN):** ANN algorithms trade a small amount of accuracy for a massive increase in search speed. They find documents that are *very close* but not guaranteed to be the absolute closest.
*   **Navigable Small World (NSW):** An ANN algorithm that pre-builds a "proximity graph" connecting each document vector to its nearest neighbors. Searching involves "hopping" through this graph from a random start point, always moving to the neighbor closest to the query.
*   **Hierarchical NSW (HNSW):** An even faster version of NSW that creates multiple layers of proximity graphs. The search starts in a sparse top layer to quickly find the general neighborhood, then progressively drops down to more detailed layers to refine the search.
*   **HNSW Efficiency:** This hierarchical approach changes the search complexity from linear (kNN) to approximately logarithmic, enabling vector search to scale to billions of documents with low latency.

## summary
Standard k-Nearest Neighbor (kNN) vector search is too slow for large-scale systems as it checks every document. To solve this, production systems use Approximate Nearest Neighbor (ANN) algorithms like HNSW. HNSW builds a multi-layered graph of vectors to navigate and find very close matches with incredible speed, trading perfect accuracy for massive performance gains.

---

# Vector Databases

## key-points
*   Vector databases are specialized databases designed to efficiently store and search high-dimensional vector data.
*   They outperform traditional relational databases for semantic search by using optimized algorithms like HNSW (Hierarchical Navigable Small World).
*   The setup process involves creating a database, defining a data "collection," and loading documents which are then automatically vectorized and indexed.
*   **Semantic Search:** Finds documents based on conceptual meaning using vector embeddings.
*   **Keyword Search:** Uses traditional methods like BM25 for exact word matching.
*   **Hybrid Search:** Combines semantic and keyword search, weighting the results to get the best of both worlds, and is commonly used in production.
*   **Filtering:** Allows search results to be narrowed down based on metadata properties (e.g., author, date).

## summary
Vector databases, like the example Weaviate, are the backbone of production RAG systems, built to handle massive-scale vector search efficiently. They simplify the process of storing data and performing advanced queries like hybrid search, which combines semantic and keyword matching. By also allowing filters, they provide a powerful and flexible way to retrieve the most relevant information.

---

# Chunking

## key-points
*   **What is Chunking?:** The process of breaking down large documents from a knowledge base into smaller, more focused text segments or "chunks."
*   **Why Chunking is Essential:**
    1.  **Embedding Model Limits:** Many models have a maximum input text length.
    2.  **Improves Search Relevancy:** Smaller chunks create more specific vectors, preventing the meaning from being diluted as it would be if an entire book were one vector.
    3.  **Efficient LLM Context:** Ensures only the most relevant, concise information is sent to the Large Language Model, saving space in its context window.
*   **The "Goldilocks" Problem:** Chunks can't be too big (lose specific meaning) or too small (lose surrounding context). Finding the right size is key.
*   **Common Chunking Strategies:**
    *   **Fixed-Size Chunking:** Divides a document into chunks of a set number of characters (e.g., every 250 characters).
    *   **Fixed-Size with Overlap:** Consecutive chunks share some text (e.g., 10% overlap). This prevents splitting a cohesive thought and preserves context at the boundaries.
    *   **Recursive Character Splitting:** A more dynamic method that splits text based on structural characters (like newlines `\n` between paragraphs), which better preserves the document's logical structure.

## summary
Chunking breaks large documents into smaller pieces to create more specific vectors for better search results. This is vital for managing embedding model limits and using the LLM's context window efficiently. Common methods include fixed-size chunks with overlap or dynamic splitting that respects the document's structure.

---

# Advance chunking techniques

## key-points
*   **Problem:** Basic chunking methods (like fixed-size) can split text in ways that lose critical context, harming retrieval accuracy.
*   **Semantic Chunking:** This technique groups sentences together based on their meaning. It measures the vector distance between the current chunk and the next sentence, starting a new chunk only when the topics diverge significantly.
*   **LLM-based Chunking:** Uses a large language model (LLM) with specific instructions to intelligently divide a document into logically cohesive sections. This is a high-performing but often "black box" approach.
*   **Context-Aware Chunking:** An enhancement for any chunking method where an LLM adds a summary or contextual explanation to each chunk. This improves both the vector's quality for search and the LLM's understanding during generation.
*   **Strategy & Trade-offs:** Advanced methods offer better performance but are computationally expensive. It's best to start with simpler methods and only adopt advanced techniques if testing proves they provide a worthwhile benefit for your specific data.

## summary
Advanced chunking techniques like semantic and LLM-based chunking overcome the limitations of basic methods by splitting documents based on meaning, not arbitrary rules. These intelligent approaches create more relevant chunks but come with higher computational costs. The right strategy involves balancing retrieval performance with implementation complexity, starting simple and upgrading only when necessary.

---

# Query parsing

## key-points
*   **The Problem:** Human, conversational prompts are often messy and inefficient for database search.
*   **The Solution (Query Parsing):** Before searching, the retriever should parse and transform the user's prompt into an optimized query.
*   **LLM Query Rewriting:** The most common and effective technique. An LLM is prompted to rewrite the user's query by clarifying ambiguity, adding relevant terminology (e.g., medical terms), and removing irrelevant details (e.g., "I was walking my dog...").
*   **Named Entity Recognition (NER):** An advanced technique that identifies and labels specific categories in a query (like people, places, dates), which can then be used for more precise vector search or metadata filtering.
*   **Hypothetical Document Embeddings (HyDE):** Instead of searching with the user's question, an LLM generates a hypothetical *ideal answer* document. The vector of this perfect answer is then used for the search, which can improve relevance by comparing "document to document" rather than "question to document."

## summary
User prompts are often conversational and must be cleaned up before being sent to a retriever in a process called query parsing. The most practical solution is using an LLM to rewrite the prompt, making it clearer and more specific for the database. While advanced techniques like NER and HyDE exist, a simple rewrite is the best starting point for most production RAG systems.

---

# Cross-Encoder, and ColBERT

## key-points
*   **Bi-Encoder:** The standard approach where documents and queries are embedded into separate, single vectors. It's very fast and scalable because document vectors can be pre-computed.
*   **Cross-Encoder:** Concatenates the query and a document together before passing them to a model to get a relevancy score. This provides the highest quality results ("gold standard") but is extremely slow and not scalable for initial retrieval.
*   **ColBERT:** A hybrid model that creates a vector for *every token* in both the query and documents. It scores relevance by matching individual query tokens to their most similar document tokens, offering near cross-encoder quality with much better speed.
*   **The Trade-off:**
    *   **Bi-Encoder:** Speed > Quality
    *   **Cross-Encoder:** Quality > Speed
    *   **ColBERT:** Balances quality and speed, but at the cost of massively increased vector storage needs.

## summary
Standard semantic search uses a fast bi-encoder, which embeds queries and documents separately. For higher accuracy, a cross-encoder processes them together but is too slow for initial search. ColBERT offers a compromise, providing high-quality results by matching individual tokens, trading massive storage requirements for a balance of speed and precision.

---


# ReRanking

## key-points
*   **What is Reranking?:** A second-stage process that takes an initial set of retrieved documents and re-orders them using a more powerful, accurate model.
*   **The Process:**
    1.  **Over-fetch:** First, retrieve a larger number of documents than needed (e.g., 20-100) using a fast method like hybrid search.
    2.  **Re-score:** Use a computationally expensive but highly accurate model, like a cross-encoder or a specialized LLM, to re-evaluate the relevance of just this smaller set of documents.
    3.  **Return Top Results:** Finally, return the top-ranked documents (e.g., 5-10) from the new, more accurate list.
*   **Why it Works:** It combines the speed of initial retrieval with the accuracy of powerful models like cross-encoders, which are too slow to use on the entire database.
*   **Benefits vs. Costs:** The significant boost in search relevance almost always outweighs the small amount of added latency.
*   **Practical Use:** Reranking is an easy and highly effective technique to implement for improving the performance of a RAG pipeline.

## summary
Reranking improves search results by taking a large initial set of documents from a fast search and re-ordering them with a slower, more accurate model like a cross-encoder. This two-step process combines speed with precision, significantly boosting relevance at the cost of a little extra latency. It is a highly recommended and easy-to-implement upgrade for any production RAG system.

---

# Conclusion

## key-points
*   **Approximate Nearest Neighbors (ANN):** A fast alternative to kNN for vector search, trading perfect accuracy for massive speed gains, making large-scale retrieval possible.
*   **Vector Databases:** Specialized databases designed to store vector data and run ANN searches, forming the foundation of scalable RAG systems.
*   **Chunking:** The practice of breaking large documents into smaller pieces to create more precise vectors and efficiently use the LLM's context window.
*   **Query Parsing:** Techniques to clean up and optimize user-submitted prompts to make them more effective for retrieval.
*   **Reranking:** A post-retrieval step that uses powerful but slower models (like cross-encoders) to re-order an initial set of documents, significantly improving final relevance.

## summary
This module covered the essential techniques for building a high-performance RAG retriever for production. It explained how vector databases use ANN for fast searching and detailed key optimization strategies. These include document chunking, query parsing, and reranking to ensure the most relevant information is found and passed to the LLM.