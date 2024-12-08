---
title: "Exploring Ollama on Google Colab with the Ollama Python Library and LangChain"
date: 2024-12-08
author: "Luciano Ayres"
tags:
  - Ollama
  - Google Colab
  - LangChain
  - Python
---

I’d like to share my experience integrating Ollama with Google Colab using the [Ollama Python Library](https://github.com/ollama/ollama-python) and the [LangChain library](https://python.langchain.com/docs/integrations/providers/ollama/). I hope you find this guide helpful.

## Why Ollama and Google Colab?

You might be wondering, **"Why use Ollama on Google Colab?"** Ollama provides a straightforward way to interact with powerful language models, while Google Colab offers a free, cloud-based environment ideal for experimentation without the need for complex local setups. Combining these two tools allows for effective language processing and accessible, scalable computing resources.

## Getting Started: Setting Up Ollama on Colab

Let’s walk through the steps I took to set up Ollama on Google Colab.

### 1. Installing Ollama

First, install Ollama using the following `curl` command:

```bash
!curl -fsSL https://ollama.com/install.sh | sh
```

This script downloads and installs Ollama on your Colab instance, making the setup process quick and easy.

### 2. Starting the Ollama Server

Next, start the Ollama server to enable interaction with the language models:

```bash
!nohup ollama serve &
```

The `nohup` command ensures the server runs in the background, allowing you to continue using your Colab session without interruption.

### 3. Pulling a Language Model

Ollama supports various language models. For this guide, I pulled the `llama3.2` model:

```bash
!ollama pull llama3.2
```

### 4. Installing the Ollama Python Library

To interact with Ollama programmatically, install the Ollama Python library:

```bash
!pip install ollama
```

## Interacting with Ollama: Generating Responses

With Ollama set up, let’s explore how to generate responses.

### Simple Text Generation

Generate a straightforward response using the following code:

```python
import ollama
from ollama import generate

response = generate(model='llama3.2', prompt="What's the capital of France?")
print(response['response'])
```

### Chatting with Ollama

Ollama also supports more interactive conversations. For example, translating sentences to different languages:

```python
from ollama import chat, ChatResponse

# Translate to French
response: ChatResponse = chat(model='llama3.2', messages=[
    {
        'role': 'system',
        'content': "You're a helpful translator. Translate the user sentence to French."
    },
     {
        'role': 'user',
        'content': 'I love programming!'
    }
])
print(response['message']['content'])
```

Now, translating the same sentence to Portuguese:

```python
response = ollama.chat(model='llama3.2', messages=[
    {
        'role': 'system',
        'content': "You're a helpful translator. Translate the user sentence to Portuguese."
    },
     {
        'role': 'user',
        'content': 'I love programming!'
    }
])
print(response['message']['content'])
```

## Integrating LangChain with Ollama

To enhance functionality, I integrated LangChain with Ollama. LangChain is a powerful library for building applications with language models, and combining it with Ollama offers expanded capabilities.

### Installing LangChain-Ollama

First, install the `langchain-ollama` package:

```bash
!pip install langchain-ollama
```

### Using OllamaLLM with LangChain

Here’s how to use the `OllamaLLM` class to invoke language models:

```python
from langchain_ollama import OllamaLLM

llm = OllamaLLM(model='llama3.2')
text = llm.invoke("Who won the 1984 Super Bowl?")
print(text)
```

### ChatOllama for Enhanced Conversations

For more interactive and structured conversations, use `ChatOllama`:

```python
from langchain_ollama import ChatOllama

llm = ChatOllama(model='llama3.2')
messages = [
    ("system", "You are a helpful translator. Translate the user sentence to Spanish."),
    ("human", "I love programming!"),
]
response = llm.invoke(messages)
print(response.content)
```

## Key Takeaways

Here are some insights from my experience:

1. **Ease of Setup:** Installing Ollama on Google Colab is straightforward. A few simple commands get everything up and running.
2. **Versatile Interactions:** Ollama handles both simple text generation and more complex chat-based interactions effectively.
3. **Powerful Integrations:** Combining Ollama with LangChain unlocks advanced functionalities, facilitating the development of sophisticated language-based applications.
4. **Resource Efficiency:** Google Colab provides the necessary computational resources without the need for expensive local hardware setups.

Feel free to check out my [Google Colab notebook](https://colab.research.google.com/drive/1a3nAnxyyT9u52NPiXC6vwOz--eDkDq4s?usp=sharing) where you can try the code yourself.

## References

- [Ollama Python Library on GitHub](https://github.com/ollama/ollama-python)
- [LangChain Integration with Ollama](https://python.langchain.com/docs/integrations/providers/ollama/)
