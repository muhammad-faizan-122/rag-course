# Introduction to Large Language Models in RAG

## key-points
*   The Large Language Model (LLM) is the "brains" of a RAG system, responsible for generating the final response using retrieved information.
*   This module will cover the fundamentals of LLMs, including the transformer architecture.
*   You will learn how to construct and code LLM calls.
*   A key focus is on "grounding" the LLM's responses in the information provided by the retriever to ensure high quality and accuracy.
*   Both practical and advanced techniques for improving LLM performance within a RAG system will be explored.
*   The module concludes with a hands-on programming assignment to build out a RAG system.

## summary
This module introduces the Large Language Model (LLM) as the core generative component of a RAG system. You will learn about the transformer architecture and practical techniques for calling LLMs in code. The primary goal is to master methods that ensure the LLM generates high-quality responses grounded in the retrieved context.

---

# How Large Language Models Work

## key-points
*   LLMs are built on the "decoder" part of the Transformer architecture, which excels at text generation.
*   **The Process:** An LLM processes a prompt by breaking it into tokens, creating initial vector representations for each, and adding positional information.
*   **Attention Mechanism:** The core of the LLM, where each token analyzes its relationship with every other token in the prompt to build a deep contextual understanding.
*   **Iterative Refinement:** The model repeatedly passes the tokens through attention and feedforward layers (8-64 times) to progressively refine their meaning.
*   **Token Generation:** The LLM predicts the next most likely token as a probability distribution and selects one, appends it, and then repeats the entire process to generate the next token.
*   **Implications for RAG:** This architecture explains why RAG works (LLMs can understand retrieved context), but also highlights their computational expense and inherent randomness.

## summary
Large Language Models use the Transformer architecture's "attention mechanism" to deeply understand the context of a prompt. They generate text one token at a time by repeatedly processing the entire sequence to predict the next most likely word. This powerful but computationally expensive process is what allows LLMs to effectively use the information provided by a RAG system.

---

# Controlling LLM Output and Randomness

## key-points
*   LLM text generation is based on making a weighted random choice from a probability distribution for the next possible token.
*   **Greedy Decoding:** A deterministic method that always picks the most probable next token, but can lead to repetitive and predictable text.
*   **Temperature:** The main control for randomness. A low temperature (<1.0) makes the output more predictable, while a high temperature (>1.0) makes it more creative and random.
*   **Top-k Sampling:** Constrains the LLM to choose only from a fixed number (k) of the most likely next tokens.
*   **Top-p Sampling:** A dynamic method that selects from the smallest set of tokens whose cumulative probability exceeds a threshold (p), making it more flexible than top-k.
*   **Repetition Penalty:** Discourages the model from repeating words or phrases to make the output sound more natural.
*   **Logit Biasing:** Allows you to manually increase or decrease the probability of specific tokens appearing (e.g., to ban profanity or promote certain keywords).

## summary
LLMs generate text by randomly choosing from a probability distribution of the next token. Parameters like temperature, top-p, and repetition penalties allow you to control this randomness. This tuning is essential for tailoring the model's output to be either creative or deterministic for your specific use case.

---

# Selecting the Right Large Language Model (LLM)

## key-points
*   Evaluate LLMs based on quantifiable metrics: model size (parameters), cost (per token), context window, speed/latency, and knowledge cutoff date.
*   Quality is often assessed using benchmarks, which come in three main types:
    *   **Automated Benchmarks (e.g., MMLU):** Use computer-graded tasks like multiple-choice tests to score models on specific subjects.
    *   **Human-Evaluated Benchmarks (e.g., LLM Arena):** Use human preference ratings to create leaderboards, capturing nuanced quality.
    *   **LLM-as-a-Judge:** Use one powerful LLM to score another, but this method can be biased towards their own model family.
*   A good benchmark is relevant to your use case, difficult enough to differentiate models, reproducible, and free from data contamination (where the model was trained on the test questions).
*   The LLM field evolves rapidly; benchmarks become "saturated" and the best model today will likely be surpassed soon, so plan for future upgrades.

## summary
Choosing an LLM involves balancing quantifiable factors like cost and speed with qualitative performance measured by benchmarks. These benchmarks can be automated, human-rated, or use another LLM as a judge, each with unique trade-offs. Given the rapid pace of innovation, it's crucial to select a good model for now while designing your system to easily accommodate future upgrades.

---

# Prompt Engineering for RAG Systems

## key-points
*   **Prompt Structure:** Prompts are often structured using a messages format with distinct roles: `system`, `user`, and `assistant`.
*   **System Prompt:** This provides high-level, persistent instructions to the LLM about its personality, tone, and operational rules (e.g., "only use retrieved documents to answer").
*   **Multi-turn Conversations:** LLMs don't have memory; the entire conversation history is resent with each new user message to maintain context.
*   **Prompt Template:** A template is used to organize the final augmented prompt, systematically injecting the system prompt, retrieved documents, conversation history, and the latest user query.
*   **Experimentation:** Using a template makes it easy to experiment with different prompt structures to see what yields the best results.

## summary
Prompt engineering for RAG involves using a structured format with `system`, `user`, and `assistant` roles to guide the LLM. A well-crafted system prompt sets the overall behavior, while a prompt template organizes the conversation history and retrieved documents. This structured approach is key to consistently generating high-quality, context-aware responses.

---

# Advanced Prompt Engineering Techniques

## key-points
*   **In-Context Learning (Few-shot/One-shot):** Improve LLM responses by providing examples of high-quality question-answer pairs directly in the prompt.
*   **Chain-of-Thought Prompting:** Instruct the LLM to "think step-by-step" or use a "scratchpad" to reason through a problem before providing the final answer, which improves accuracy on complex tasks.
*   **Reasoning Models:** Newer LLMs that are pre-trained to perform step-by-step reasoning internally. They are more accurate for complex tasks but are also slower, more expensive, and require different prompting strategies.
*   **Context Window Management:** Advanced techniques increase prompt length, making it crucial to manage the context window. This is done via "context pruning," such as summarizing conversation history or only keeping the most recent messages.

## summary
Advanced prompt engineering techniques like in-context learning and chain-of-thought prompting can significantly boost LLM performance. Newer reasoning models automate this process but come with higher costs and require careful context window management. It's best to add these advanced techniques incrementally only when a clear need arises in your RAG system.

---

# Detecting and Reducing Hallucinations in RAG Systems

## key-points
*   **What are Hallucinations?** LLMs are designed to generate probable text, not necessarily factual text, leading them to create plausible but incorrect information.
*   **Why are they a Problem?** Hallucinations provide inaccurate information, are difficult to detect because they sound plausible, and erode user trust over time.
*   **RAG Systems Still Hallucinate:** While RAG (Retrieval-Augmented Generation) is a primary method to reduce hallucinations by grounding the LLM with a knowledge base, it doesn't eliminate them entirely.
*   **Types of Hallucinations:** They can range from minor inaccuracies (e.g., stating a discount is 5% instead of 10%) to inventing entirely new facts (e.g., creating a non-existent student discount).
*   **Prompt Engineering:** Instruct the LLM in the system prompt to only make factual claims based on the retrieved information and to cite its sources.
*   **External Verification Systems:** Use tools like `ContextCite` to check if each sentence in the response is supported by the source documents, which helps prevent hallucinated citations.
*   **Benchmarking:** Evaluate your system's performance using benchmarks like `ALCE`, which measure fluency, factual correctness, and citation quality against a pre-set knowledge base.

## summary
LLM hallucinations, the generation of plausible but false information, remain a challenge even in RAG systems. The most effective strategies to minimize them are grounding responses in retrieved data through strict prompt engineering and requiring citations. For greater reliability, use external systems to verify source grounding and benchmarks to evaluate overall system accuracy.

---

# Evaluating LLM Performance in RAG Systems

## key-points
*   **Isolate the LLM's Role:** When evaluating a RAG system, it's crucial to distinguish between the retriever's performance (finding relevant information) and the LLM's performance (using that information to create a good response).
*   **LLM Responsibilities:** The LLM's job is to answer the user's prompt, correctly use the retrieved information, cite sources, and ignore any irrelevant documents the retriever might have provided.
*   **LLM-based Evaluation:** Since response quality can be subjective, many metrics use another LLM as a "judge" to assess the output in a scalable way. The open-source `Ragas` library is a key tool for this.
*   **Key Ragas Metrics:**
    *   **Response Relevancy:** Measures if the LLM's answer is relevant to the user's original question.
    *   **Faithfulness:** Checks if the factual claims made in the LLM's response are actually supported by the retrieved documents.
*   **Human Feedback:** System-wide metrics, like user "thumbs up/down" ratings, are valuable for A/B testing changes to the LLM (e.g., a new system prompt) and measuring their impact on user satisfaction.
*   **Combined Approach:** The most effective evaluation strategy combines automated, LLM-based metrics with direct human feedback to get a complete picture of the LLM's performance.

## summary
To improve your RAG system, you must evaluate the LLM's performance separately from the retriever. This is often done using other LLMs as judges to measure metrics like response relevancy and faithfulness to the source documents. Combining these automated evaluations with direct user feedback provides the most confident assessment of your LLM's quality.

---

# Improving RAG with Agentic Workflows

## key-points
*   **What is an Agentic Workflow?** It's a system that uses multiple LLMs, each responsible for a single, specific step in a larger process, like a flowchart.
*   **Modular Approach:** Instead of one large LLM doing everything, you can use different models for different tasks (e.g., a small, fast model for routing queries and a larger, more powerful model for generating the final answer).
*   **Example RAG Workflow:**
    1.  A **Router LLM** decides if a knowledge base search is needed.
    2.  An **Evaluator LLM** checks if the retrieved information is sufficient.
    3.  A **Generator LLM** creates the draft response.
    4.  A final **LLM** adds accurate citations.
*   **Common Workflow Patterns:**
    *   **Sequential:** A linear, step-by-step process (e.g., rewrite query -> generate -> add citations).
    *   **Conditional:** An LLM makes a decision that determines the next step (e.g., routing a query).
    *   **Iterative:** A loop where an LLM refines its output based on feedback until it's correct (e.g., writing and testing code).
    *   **Parallel:** A task is broken into sub-tasks, processed by different LLMs simultaneously, and then recombined.

## summary
Agentic workflows improve RAG systems by breaking down tasks into a series of steps, each handled by a specialized LLM. This modular approach allows for using different models for different jobs, creating more capable and flexible systems. Common patterns include sequential, conditional, iterative, and parallel workflows to manage the flow of information.

---

# Fine-Tuning and RAG

## key-points
*   **What is Fine-Tuning?** It is the process of retraining a general-purpose LLM on a specific, labeled dataset to specialize its performance for a particular domain or task.
*   **RAG vs. Fine-Tuning:** RAG is best for **knowledge injection** (giving the LLM access to new, up-to-date information). Fine-tuning is best for **domain adaptation** (changing the model's style, tone, and expertise, like making it sound like a doctor).
*   **Fine-Tuning's Impact:** It primarily affects *how* a model responds (style, structure, expertise) rather than teaching it new factual information.
*   **Trade-offs:** Specializing a model through fine-tuning improves its performance in the target domain but can reduce its performance on other, unrelated tasks.
*   **Complementary Tools:** RAG and fine-tuning are not competitors; they work best together. You can fine-tune an LLM to be better at its specific role within a RAG pipeline, such as more effectively incorporating retrieved context into its answers.

## summary
Fine-tuning retrains an LLM on specialized data to adapt its style and expertise for a specific domain, like medicine or law. While RAG excels at injecting new knowledge, fine-tuning is best for changing a model's behavior and tone. These two techniques are complementary and can be combined to create a highly optimized and capable RAG system.

---


