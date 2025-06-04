# Wolfram Blockchain Agent
This repository contains a prototype for the Wolfram Blockchain Agent that provides natural language access to blockchain data and insights. The system combines Wolfram's symbolic AI capabilities with large language models (LLMs) to make blockchain exploration more accessible.

## Project Background
This work builds upon previous projects funded by:
- XRPL Grants - Enabling real-time blockchain data integration (Wave 6)

The project leverages the [WAEXLink paclet](https://resources.wolframcloud.com/PacletRepository/resources/KirillBelov/WAEXLink) for Wolfram Language integration with blockchain data services.

API access for this project is provided through [EchoFeed](waexservices.com), offering comprehensive blockchain data.

## RAG Pipeline ＋ Computational Augmented Generation

A **Retrieval‑Augmented Generation (RAG)** reference implementation built with **Wolfram Language** that is tailored for on‑chain data analysis. The project combines fast vector search, language‑model reranking and Wolfram’s computational kernel so you can ask natural‑language questions about a blockchain and receive accurate, reproducible answers – complete with executable code snippets.

---

## ✨ Key Features

| Capability                     | Description                                                                                            |  
| ------------------------------ | ------------------------------------------------------------------------------------------------------ |  
| **Two‑Stage RAG Pipeline**     | Query embedding → similarity lookup → LLM‑based re‑ranking → answer synthesis.                         |  
| **Computational Augmentation** | Answers can call Wolfram kernels (e.g. `BlockchainBlockData`, `CurrencyConvert`) for live calculation. |  
| **Pre‑built Semantic Index**   | 360+ Markdown documentation chunks embedded with*MiniLM* and stored in a `USearch` vector DB.         |  
| **Reranker Neutrality**        | Switch between no‑rerank, heuristic or LLM reranking with one flag.                                    |  
| **Out‑of‑the‑box Examples**    | Notebook & CLI examples for Ethereum, Bitcoin, Bloxberg, and private chains.                           |

---

## 🏗️ Architecture

![alt text](retrieve_rerank_pipeline.png)

> **Note** A high‑resolution PNG of the full diagram is located at `docs/rag_pipeline.png` and rendered automatically on GitHub.

---

## 🔧 Installation

```bash  
# 1. Clone the repo  
$ git clone https://github.com/<your‑org>/rag‑blockchain‑wl.git  
$ cd rag‑blockchain‑wl

# 2. Set up a Wolfram Environment (13.3 or newer recommended)  
#    For Wolfram Script users:  
$ wls install paclet ‑‑auto SemanticSearch  # if not already installed

# 3. (Optional) create a Python venv if you plan to call external LLM APIs  
```

---

## 🚀 Quickstart (Notebook)

1. Open `notebooks/RAG‑Demo.nb`.  
2. Evaluate **Shift ↵** cell‑by‑cell.  The notebook will:

   * Load the semantic index (`CreateSemanticSearchIndex`)  
   * Embed the user query  
   * Retrieve the **top‑20** docs  
   * Optionally re‑rank with GPT‑4 (set `RerankingMethod -> "LLM"`)  
   * Synthesize the answer with in‑context citations.

Example cell:

```wl  
query = "Obtain information about a blockchain block with a specified hash.";  
cands = SemanticSearch[indx, query, MaxItems -> 20, RerankingMethod -> None];  
ranked = SemanticRanking[cands, query][[;;10]];  
LLMSynthesize["Your task is to...", ranked]  
```

Sample output ↓

```txt  
To obtain information about a blockchain block using a specified hash, use  
    BlockchainBlockData["hash"]  
... (full answer omitted)  
```

---

## 🧰 Relevant Wolfram Language Symbols

```wl  
symbols = {  
  "BlockchainData", "BlockchainBlockData", "BlockchainTransactionData",  
  "BlockchainAddressData", "BlockchainTokenData", "BlockchainBase",  
  "$BlockchainBase", "GenerateAsymmetricKeyPair", "PrivateKey", "PublicKey",  
  "BlockchainKeyEncode", "BlockchainTransaction", "BlockchainTransactionSign",  
  "BlockchainTransactionSubmit", "BlockchainPut", "BlockchainGet",  
  "CurrencyConvert"  
};  
```

All documentation for these symbols is pre‑chunked as Markdown and included in the `docs/snippets/` directory. The semantic index ships pre‑built, but you can regenerate it via:

```wl  
snippets = Import /@ FileNames["*.md", "docs/snippets"];  
indx = CreateSemanticSearchIndex[snippets];  
```

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.
