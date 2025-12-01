---
date created: 2025-12-01 14:27
---

## What is Tokenization?

- Tokenization is the process of converting a request, often a string, into the series of Token ID's that model receives as an input. A token can be a word, character or an symbol.
- The tokenizer's job is to break apart the string and convert it into a sequence of token IDs, a step called "encoding." It is also the tokenizer's job to convert a sequence of IDs back into a string, a step called "decoding." Once provided with a sequence of token IDs, the model tries to predict the next token.

Imagine a scenario where we have the following sentence: "Hello, how are"


1. The tokenizer will first break apart the sentence into different pieces, or tokens. For example:
    
    - `Hello,`
    - `how`
    - `are`
2. The tokenizer will then get the token ID corresponding to each token. For example:
    
    - `Hello,` -> `432`
    - `how` -> `523`
    - `are` -> `87`
    
    This sequence of token IDs is then provided to the model.
    
3. The model will try to predict the next token. Let's say it predicts:
    
    - `75`
    
    We will then have a full sequence of token IDs:
    
    - `432` + `523` + `87` + `75`
4. This sequence can be decoded back into a full string:
    
    - `Hello,` + `how` + `are` + `you` -> `Hello, how are you`

We can then repeat this process as much as needed.

5. The model will try to predict the next token again. Let's say it predicts:
    
    - `868`
    
    We will then have a full sequence of token IDs:
    
    - `432` + `523` + `87` + `75` + `868`
6. This sequence will be decoded back into a full string:
    
    - `Hello,` + `how` + `are` + `you` + `?` -> `Hello, how are you?`

This iterative process of encoding, prediction, and decoding allows the model to generate coherent and contextually relevant text.

---
