# Introduction

## key-points
*   **The Retriever's Job:** To find relevant documents in a knowledge base that help an LLM answer a user's prompt.
*   **The Challenge:** Retrievers must work with messy, unstructured data (emails, memos, articles) and conversational user queries, all while being extremely fast.
*   **What You'll Learn:**
    *   The primary techniques retrievers use to search unstructured information.
    *   The theoretical foundations, strengths, and weaknesses of each technique.
    *   How to combine different retrieval methods for the best results.
    *   Strategies for evaluating a retriever's performance.
*   **Practical Application:** The module includes hands-on coding exercises and a programming assignment to apply these concepts directly.

## summary
This module provides a deep dive into the retriever, the search component of a RAG system. You will learn the techniques it uses to quickly find relevant information in messy, unstructured data based on conversational queries. The module covers the theory, practical application through coding, and methods for evaluating retriever performance.

---


# Retrieval Architecture overview

## key-points
*   **Hybrid Search:** Modern retrievers combine multiple search techniques to find the most relevant documents for a given prompt.
*   **Two Core Search Methods:**
    1.  **Keyword Search:** A traditional approach that finds documents containing the *exact words* from the prompt.
    2.  **Semantic Search:** A more advanced method that finds documents with a similar *meaning* to the prompt, even if the exact words don't match.
*   **The Process:**
    1.  Both keyword and semantic searches are run in parallel, each generating a ranked list of documents.
    2.  **Metadata Filtering:** Both lists are then filtered based on document metadata (e.g., date, author, user's department) to exclude irrelevant documents.
    3.  **Re-ranking:** The two filtered lists are combined into a single, final ranked list of the most relevant documents.
*   **Benefits of Hybrid Search:** This approach combines the precision of keyword search with the flexibility of semantic search and the rigid logic of metadata filtering, leading to better overall performance.

## summary
A modern retriever uses a hybrid search architecture, running both a keyword search (for exact matches) and a semantic search (for meaning) simultaneously. The results are then filtered using metadata to ensure contextual relevance. Finally, these filtered lists are combined and re-ranked to produce the most accurate set of documents for the LLM.

---

# Metadata Filtering

## key-points
*   **What it is:** A technique that uses rigid criteria based on a document's metadata (e.g., author, creation date, access rights) to include or exclude it from search results.
*   **How it works:** Similar to a SQL query or filtering a spreadsheet, it keeps only the documents that meet every specified condition.
*   **Role in RAG:** It's not used as a primary search method but rather to *refine* and narrow down the results returned by keyword or semantic search.
*   **Common Use Cases:** Filters are often based on user attributes (like subscription status or location) rather than the text of the user's prompt.
*   **Advantages:** It is simple, fast, mature, and provides strict, rule-based control over which documents are retrieved.
*   **Limitations:** It is overly rigid, ignores the actual content of the documents, and cannot rank the results that pass the filter. It is useless as a standalone retrieval method.

## summary
Metadata filtering refines search results using strict rules based on document attributes like date or author. In a RAG system, it's a secondary tool used to narrow down results from other search methods, often based on user attributes. While it offers essential, rigid control, it is ineffective on its own because it completely ignores document content.

# keyword search - TF-IDF

## key-points
*   **Core Idea:** Retrieves documents that share common words (keywords) with the user's prompt, based on the assumption that shared words indicate relevance.
*   **Bag of Words Model:** The order of words is ignored; only the presence and frequency of each word matter.
*   **Sparse Vectors:** Text is converted into a "sparse vector," which is a long list that counts the frequency of each word in the system's vocabulary.
*   **Inverted Index (Term-Document Matrix):** To enable fast searching, documents are pre-processed into an index. This is a grid where rows represent words and columns represent documents, making it easy to find all documents containing a specific word.
*   **TF-IDF Scoring:** The foundational scoring method, which combines:
    *   **Term Frequency (TF):** How often a keyword appears in a document (normalized for document length).
    *   **Inverse Document Frequency (IDF):** How rare that keyword is across the entire knowledge base, giving more weight to meaningful words ("pizza") and less to common ones ("the").
*   **Modern Approach (BM25):** While TF-IDF is foundational, modern systems often use a more refined and effective algorithm called BM25.

## summary
Keyword search finds relevant documents by matching shared words between a prompt and a knowledge base. It uses TF-IDF scoring to prioritize documents that contain frequent but rare keywords, ignoring common filler words. This simple yet effective technique is a core component of modern RAG systems.

---

# keyword search - BM25

## key-points
*   **BM25 (Best Matching 25):** The modern standard algorithm for keyword search in most retrievers, representing a significant improvement over the classic TF-IDF method.
*   **Key Improvements over TF-IDF:**
    *   **Term Frequency Saturation:** BM25 applies diminishing returns, meaning a document's relevance score doesn't increase linearly with every repetition of a keyword. For example, the 20th mention of "pizza" is less impactful than the 10th.
    *   **Document Length Normalization:** It penalizes long documents less aggressively than TF-IDF, allowing them to rank highly if they contain a high frequency of keywords.
*   **Tunable Hyperparameters:** BM25 includes parameters that allow you to customize the degree of term frequency saturation and document length normalization to best fit your specific knowledge base.
*   **Strengths of Keyword Search (BM25):** It is simple, effective, and provides a strong performance baseline. Crucially, it ensures that retrieved documents contain the user's exact keywords, which is vital for technical terms or product names.
*   **Primary Weakness:** Keyword search fails to match documents that have a similar *meaning* but use different words (synonyms) than the user's prompt.

## summary
BM25 is the industry-standard algorithm for keyword search, improving upon TF-IDF with features like term frequency saturation and better document length normalization. While its strength lies in matching exact keywords, making it simple and effective, its main weakness is its inability to understand semantic meaning. This limitation necessitates the use of other techniques, like semantic search, to handle queries with synonyms or varied phrasing.

---

# Semantic search - Introduction

## key-points
*   **Core Idea:** Unlike keyword search, semantic search matches documents to prompts based on shared **meaning** and context, not just shared words. It can understand synonyms (e.g., 'happy' vs. 'glad').
*   **Embedding Models:** The core technology is an **embedding model**, which converts text (words, sentences, or documents) into a numerical representation called a **vector**.
*   **High-Dimensional Space:** These vectors act like coordinates, placing the text at a specific point in a high-dimensional space. The key principle is that semantically similar concepts are mapped to points that are **close together**.
*   **Measuring Similarity:** Relevance is quantified by measuring the distance or similarity between the prompt's vector and each document's vector. The most common measure is **cosine similarity**, which checks if vectors point in a similar direction.
*   **The Search Process:**
    1.  Pre-calculate vectors for all documents in the knowledge base.
    2.  Create a vector for the user's prompt using the same embedding model.
    3.  Rank documents by how close their vector is to the prompt's vector.

## summary
Semantic search uses embedding models to convert text into numerical vectors that capture its meaning. It operates in a high-dimensional space where similar concepts are placed close together. By finding the document vectors closest to a prompt's vector, it retrieves results based on contextual relevance, not just keyword matches.

---

# Semantic search - Embedding model deepdive

## key-points
*   **Core Principle (Contrastive Training):** The model learns by being shown **positive pairs** (similar text that should be close) and **negative pairs** (dissimilar text that should be far apart). This process is called contrastive training.
*   **The Training Process:** It's an iterative process:
    1.  The model starts by assigning **random vectors** to text.
    2.  It evaluates how well it places the positive/negative pairs.
    3.  It updates its internal parameters to **pull positive pairs closer** and **push negative pairs further apart**.
    4.  This loop is repeated millions of times until a meaningful structure emerges.
*   **The Role of High-Dimensional Space:** Embedding models use hundreds or thousands of dimensions to provide enough "space" to manage the complex push-and-pull forces from millions of training pairs, allowing nuanced relationships to form.
*   **Key Takeaways for Practical Use:** The resulting vector locations are relative; only the *distance* between them matters. Consequently, you can **only compare vectors generated by the same embedding model**; mixing models will produce nonsensical results.

## summary
Embedding models learn through **contrastive training**, using millions of "positive" (similar) and "negative" (dissimilar) text pairs. The model iteratively adjusts its parameters to pull positive pairs closer and push negative pairs further apart in a high-dimensional space. This transforms an initially random space into one where vector distance accurately reflects semantic meaning.

---

# Hybrid Search

## key-points
*   **Combining Strengths:** Hybrid search combines three techniques to maximize relevance:
    *   **Metadata Filtering:** Provides a strict, yes/no rule-based filter (e.g., by date or author).
    *   **Keyword Search:** Excels at matching exact terms (like product names) but misses synonyms.
    *   **Semantic Search:** Finds documents with similar meaning using vector embeddings, offering flexibility that keyword search lacks.
*   **The Hybrid Search Pipeline:**
    1.  Run keyword and semantic searches in parallel, creating two separate ranked lists of documents.
    2.  Apply metadata filters to both lists to remove documents that don't meet specific criteria.
    3.  Merge the two filtered lists into a single, final ranking using an algorithm like **Reciprocal Rank Fusion (RRF)**.
*   **Reciprocal Rank Fusion (RRF):**
    *   RRF combines the two lists by giving documents a score based on the reciprocal of their rank in each list (e.g., 1st place gets 1 point, 2nd place gets 1/2 point).
    *   A **beta** weight is used to control the importance of the semantic vs. keyword search rankings (a 70% semantic / 30% keyword split is a common starting point).
*   **Final Output:** The retriever returns the top 'K' documents from this final, fused ranking, providing a more balanced and relevant set of results than any single method could.

## summary
Hybrid search combines the precision of keyword search, the contextual understanding of semantic search, and the strict rules of metadata filtering. It runs keyword and semantic searches in parallel, filters the results, and then merges them into a single superior ranking using Reciprocal Rank Fusion. This tunable approach leverages the best of each technique to produce highly relevant and robust search results.

---

# Evaluating Retriever

## key-points
*   **Ground Truth is Essential:** To measure a retriever's quality, you first need a "ground truth" list of all the correct, relevant documents for a given prompt to serve as the answer key.
*   **Precision vs. Recall:** These are two fundamental metrics:
    *   **Precision:** Measures how trustworthy the results are. It's the percentage of retrieved documents that are actually relevant (`Relevant Retrieved / Total Retrieved`).
    *   **Recall:** Measures how comprehensive the results are. It's the percentage of all possible relevant documents that your retriever found (`Relevant Retrieved / Total Relevant`).
*   **The @K Standard:** Metrics are typically calculated for the **top K** results (e.g., Precision@5, Recall@10), focusing the evaluation on the most highly ranked documents.
*   **Mean Average Precision (MAP):** A holistic metric that evaluates the average precision across the top K results. It heavily rewards retrievers that rank relevant documents higher up in the list.
*   **Mean Reciprocal Rank (MRR):** Measures how high up the *first* relevant document appears in the rankings, on average. It's useful for tasks where finding one good answer quickly is the priority.
*   **The Main Challenge:** All of these metrics depend on having a manually-labeled ground truth dataset, which can be time-consuming and expensive to create.

## summary
Evaluating a retriever's performance means measuring its ability to find relevant documents. This is done using key metrics like Precision (trustworthiness) and Recall (comprehensiveness), often calculated on the top 'K' results. More advanced metrics like MAP and MRR provide deeper insights, but all methods require a manually-created "ground truth" dataset for comparison.

---

# conclusion

## key-points
*   **Hybrid Search:** Modern retrievers combine three main techniques: keyword search, semantic search, and metadata filtering.
*   **Keyword Search:** Ranks documents based on shared keywords with the prompt. It's fast and ensures exact word matching, which is useful for technical terms.
*   **Semantic Search:** Ranks documents based on similar meaning, using embedding models to represent text as vectors. This provides flexibility that keyword search lacks.
*   **Metadata Filtering:** Excludes documents based on rigid criteria (e.g., date, author), acting as a strict rule-based filter.
*   **The Process:** A hybrid search runs keyword and semantic searches in parallel, filters the results with metadata, and then fuses the two ranked lists into a final, superior ranking.
*   **Evaluation:** Retriever quality is measured using metrics like precision, recall, and MAP, which help evaluate performance and tune the system's various hyperparameters.

## summary
This module covered the hybrid architecture of a modern RAG retriever, which combines keyword search, semantic search, and metadata filtering. You learned how each technique works, their unique strengths, and how they are fused to create a superior ranking. Finally, you were introduced to key metrics used to evaluate and tune the retriever's performance.