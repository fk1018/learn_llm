# Notes
## Tokens
- A **token** is a unit of text. It could be as small as a single character, part of a word, or an entire word depending on the tokenizer used by the language model.
- For example, the sentence `"ChatGPT is great!"` might break down into the following tokens
  - "ChatGPT"
  - " is"
  - " great"
  - "!"
- Different models may tokenize differently.
- **Pre-training or training using tokens** refers to the total number of tokens that were used to train a language model before it was made available for use. During pre-training, a large-scale dataset (usually a mix of text from books, websites, articles, and more) is processed to teach the model how language works.
    - **Larger Token Counts = More Learning**: A model exposed to more tokens during pre-training typically learns more about language patterns, meaning it can understand and generate better responses.
    - **as of DEC 14 2024**: Models like GPT-4, Claude, and others are trained on massive datasets containing trillions of tokens, enabling them to grasp diverse language structures, facts, and reasoning patterns.
    - A higher token count during pre-training often translates to better general knowledge and a wider understanding of different languages, domains, and styles.
    - Pre-training tokens define what the model knows before it's deployed (like its "education").
## Context
- **Context** refers to the text (or tokens) the model has "seen" or been given to process at once.
- When you send a message, the entire conversation history (up to a certain limit) is included as context. This allows llms to "remember" what you've talked about and respond coherently.
- The **context window** is the maximum number of tokens the model can handle in a single interaction (including both your input and the llms responses). If the conversation exceeds this window, older tokens (usually the earliest messages) are truncated.
    - Techniques like RoPE (Rotary Position Embedding) and Self-extend help increase the size of these windows.
- When you interact with the model, it doesn't "learn" from the interaction in the same way—it uses its pre-trained knowledge within a specific context window to generate responses.