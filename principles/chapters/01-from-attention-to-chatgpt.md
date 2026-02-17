# From “Attention” to ChatGPT: the modern LLM timeline (Chapter 1)

## Summary

- AI has been a “perennially on-the-horizon” technology for decades, with notable progress in the 2000s–2010s (e.g., chess engines, speech recognition, self-driving).
- The modern “generative AI” wave accelerated after **2017** with the Transformer paper **“Attention Is All You Need”**.
- LLMs generate text by taking a sequence of **tokens** and predicting the **next token**.
- A major inflection point occurred in **Nov 2022** when **ChatGPT** made LLMs mainstream via a simple chat interface.
- Today’s LLM ecosystem includes multiple major providers with both consumer products and developer APIs.

## Core concepts

### LLMs as next-token predictors

A large language model (LLM) is trained to predict the next token (word/punctuation subunit) given prior tokens. This “autocomplete at scale” turns into surprisingly general capabilities once models are large enough and trained on broad text corpora.

### Why 2017 mattered

The Transformer architecture (“Attention is All You Need”) unlocked scaling to much larger models and better long-range context handling than prior approaches, catalyzing the modern generative AI boom.

### Why 2022 mattered

ChatGPT’s viral moment (Nov 2022) made:

- LLMs widely understood outside research circles
- Conversational UX the default mental model
- Developer demand for LLM APIs explode

## Major provider landscape (high level)

- **OpenAI** (ChatGPT + developer APIs)
- **Anthropic** (Claude)
- **Google** (Gemini / DeepMind)
- **Meta** (Llama open-source model family)
- Others (e.g., Mistral, DeepSeek)

## Practical takeaways

- When building agents, you’re building on top of **next-token prediction**. Many “agent problems” are really _prompting + context selection + tool design_ problems.
- Track **model architecture and UX inflection points** (like Transformers and ChatGPT), because they often change what’s feasible for applications.
