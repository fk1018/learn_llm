# Notes

## Tokens

- A **token** is a unit of text. It could be as small as a single character, part of a word, or an entire word depending on the tokenizer used by the language model.
- For example, the sentence `"ChatGPT is great!"` might break down into the following tokens
  - "ChatGPT"
  - " is"
  - " great"
  - "!"
- Different models may tokenize differently.
- **Pre-training or training using tokens** refers to the total number of **tokens** that were used to train a language model before it was made available for use. During pre-training, a large-scale dataset (usually a mix of text from books, websites, articles, and more) is processed to teach the model how language works.
  - **Larger Token Counts = More Learning**: A model exposed to more **tokens** during pre-training typically learns more about language patterns, meaning it can understand and generate better responses.
  - **as of DEC 14 2024**: Models like GPT-4, Claude, and others are trained on massive datasets containing trillions of tokens, enabling them to grasp diverse language structures, facts, and reasoning patterns.
  - A higher **token count** during pre-training often translates to better general knowledge and a wider understanding of different languages, domains, and styles.
  - Pre-training **tokens** define what the model knows before it's deployed (like its "education").

## Context

- **Context** refers to the text (or tokens) the model has "seen" or been given to process at once.
- When you send a message, the entire conversation history (up to a certain limit) is included as **context**. This allows llms to "remember" what you've talked about and respond coherently.
- The **context window** is the maximum number of tokens the model can handle in a single interaction (including both your input and the llms responses). If the conversation exceeds this window, older tokens (usually the earliest messages) are truncated.
  - Techniques like **RoPE (Rotary Position Embedding)** and **Self-extend** help increase the size of these windows.
- When you interact with the model, it doesn't "learn" from the interaction in the same way—it uses its pre-trained knowledge within a specific **context window** to generate responses.

## Text Representation && Numerical Representation

- These are both approaches for the retrieval of documents

### Text Representation

- Works with text. It directly matches the input to documents based on keywords, phrases, or other textual patterns.

### Numerical Representation

- In modern systems, both the query and the documents are transformed into numerical vectors (lists of numbers like **[x, y, z]**).
- This numerical representation enables more powerful matching techniques using **mathematical similarity measures** (like cosine similarity).
- Why use numerical representation?
  - Text is converted into numbers because computers work better with numbers for tasks like **searching, ranking, and comparing similarities**.
  - The model converts text (both the query and the documents) into a **dense embedding** (a numerical vector).
  - These vectors encode the meaning of the text, so similar texts have similar vectors.

#### **Cosine Similarity**

- **Cosine similarity** is a mathematical function that measures the similarity between two vectors.
- Think of it as checking how "close" two points (vectors) are in a high-dimensional space.
- If the vectors point in the same direction, they are considered similar.
- Formula:
  `Cosine Similarity=A⋅B/∥A∥∥B∥`
  - A and B are the vectors (representing the query and document).
  - The numerator A⋅BA⋅B is the dot product (a measure of alignment).
  - The denominator normalizes the vectors by their magnitudes.
- **Cosine similarity** helps decide which document vectors are closest to the query vector, making retrieval more accurate.
- Example: Imagine you prompt "What is the capital of France?"
  1. Convert your question into a vector like **[0.9, -0.3, 0.8]**.
  2. Compares this vector with document vectors (e.g., **[0.9, -0.2, 0.85]** for a document mentioning "Paris").
  3. Using cosine similarity, the system finds that "Paris is the capital of France" has the highest similarity to your query, so it retrieves that document.

# RAG

## Query Translation

- Translate the question into a form that is better suited for retrieval
- Question -> 🧠 -> Re-phase, break-down, abstract, convert to hypotethical docs
  - **Multi-query**: Generates multiple variations of the query to maximize the chances of retrieving relevant results from the database.
  - **RAG-Fusion**: Combines outputs from multiple retrieval strategies or sources to improve the final result.
  - **Decomposition**: Breaks down complex queries into simpler sub-queries that are easier to retrieve answers for.
  - **Step-back**: Revisits the query or refines it if initial retrieval fails, potentially using feedback from the system.
  - **HyDE (Hypothetical Document Embeddings)**: Converts the query into hypothetical documents or summaries to guide retrieval.

# Running llama.cpp

## Temperature

- For deterministic and repeatable outputs: Lower temperature and use a seed.
- For creative or exploratory tasks: Higher temperature and enable sampling (top_k or top_p).
- How Temperature Works"
  - Temperature affects the probability distribution over the model's possible outputs.
  - A lower temperature makes the model more deterministic, while a higher temperature introduces more randomness and creativity.
  - f p(x)p(x) is the probability of a token x:
    `p(x) = exp(logit(x)/T) / ∑exp(logit(y)/T)`
    - Where:
      - T is the **temperature**.
      - logit(x) is the raw score for token x.
    - When T>1T>1, the probabilities are spread more evenly, increasing randomness. When T<1T<1, the probabilities are more concentrated on the top choices.
- Temperature Values and Their Effects
  - Low Temperature (e.g., 0 or 0.1):
    - The model becomes **deterministic** and always selects the most probable token.
    - Best for tasks where accuracy and consistency are crucial (e.g., summarization, factual answers).
  - Moderate Temperature (e.g., 0.7):
    - Adds some randomness to the output, producing creative but relevant responses.
    - Good for tasks like storytelling or brainstorming.
  - High Temperature (e.g., 1.0 or above):
    - The model becomes highly random, sometimes generating surprising or nonsensical results.
    - Useful for exploratory tasks, but less reliable for factual answers.
- Examples:
  - Input Prompt: `"The quick brown fox"`
    - With Temperature T=0.1T=0.1:
      - "The quick brown fox jumps over the lazy dog."
    - With Temperature T=0.7T=0.7:
      - "The quick brown fox leapt gracefully over a sleepy canine."
    - With Temperature T=1.2T=1.2:
      - "The swift auburn vixen vaults the drowsy mutt."
- When to Adjust Temperature
  - Lower Temperature:
    - Tasks requiring precise and consistent answers.
    - Example: Code generation, factual Q&A, or structured tasks.
  - Higher Temperature:
    - Tasks requiring creativity or variability.
    - Example: Writing poetry, brainstorming ideas, or generating fiction.
