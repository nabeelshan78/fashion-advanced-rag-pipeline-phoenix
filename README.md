# 👗 Fashion Forward RAG Chatbot: An Intelligent AI Shopping Assistant

[![Python Version](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://www.python.org/)
[![Frameworks](https://img.shields.io/badge/Frameworks-Gradio%20%7C%20Flask-green.svg)](https://gradio.app/)
[![Database](https://img.shields.io/badge/Vector%20DB-Weaviate-orange.svg)](https://weaviate.io/)
[![Observability](https://img.shields.io/badge/Tracing-Arize%20Phoenix-red.svg)](https://phoenix.arize.com/)
[![License](https://img.shields.io/badge/License-MIT-purple.svg)](LICENSE)

An advanced, end-to-end Retrieval-Augmented Generation (RAG) system that powers a conversational AI assistant for a fashion retail store. This project goes beyond a simple chatbot implementation by incorporating **intelligent task routing, dynamic parameter tuning, and a robust observability pipeline for performance and cost optimization**.

---

### 🌟 Live Demo & Screenshots

Here is a glimpse of the AI Shopping Assistant in action, built with Gradio for a seamless user experience.

<table>
  <tr>
    <td><img src="images/1.png" width="600" height="900"></td>
    <td><img src="images/3.png" width="600" height="900"></td>
  </tr>
  <tr>
    <td><img src="images/5.png" width="600" height="900"></td>
    <td><img src="images/7.png" width="600" height="900"></td>
  </tr>
</table>

---


## ✨ Key Features

* **🧠 Intelligent Task Routing**: The system first classifies a user's query to determine intent:
    * **Product vs. FAQ**: Routes the query to either the product database or the FAQ knowledge base.
    * **Creative vs. Technical**: Further classifies product queries to tailor the LLM's response style. Creative queries (e.g., "suggest an outfit") use higher `temperature` for imaginative answers, while technical queries (e.g., "do you have blue shirts?") use lower `temperature` for factual precision.
* **🚀 Optimized RAG Pipeline**: Implements two distinct RAG strategies:
    * **Standard Pipeline**: A comprehensive approach using an LLM to generate metadata filters from the query, followed by a filtered vector search.
    * **Simplified Pipeline**: A cost- and latency-optimized approach that uses direct vector search, significantly reducing token consumption.
* **📊 End-to-End Observability**: Integrated with **Arize Phoenix** to **trace every step of the RAG pipeline**. This allows for in-depth analysis of:
    * **Cost Management**: Tracking token usage and cost per query for different models and prompts.
    * **Latency Monitoring**: Identifying and optimizing bottlenecks in the pipeline.
    * **Performance Evaluation**: Debugging LLM and retriever outputs to improve accuracy.
* **Advanced Vector Search**: Leverages **Weaviate** as a vector database for efficient semantic search over a catalog of 44,000+ fashion products and a comprehensive FAQ set.
* **Modular & Scalable Architecture**: The system is built with decoupled components, including a **Flask API** to serve embedding and reranking models, making it scalable and easy to maintain.
* **Interactive UI**: A user-friendly and aesthetically pleasing chat interface built with **Gradio**.

---

## 🛠️ System Architecture

The project follows a modular RAG architecture where each component is specialized for a specific task. The data flows from the user interface through the routing and retrieval logic, culminating in a context-aware response from the LLM. The entire process is monitored by Phoenix.

```mermaid
graph TD
    subgraph "User Interface"
        A["Gradio UI"]
    end

    subgraph "Core RAG Pipeline"
        B["Query Router"]
        C["Product Workflow"]
        D["FAQ Workflow"]
    end

    subgraph "Product Workflow Components"
        C1["Nature Classifier <br>(Creative/Technical)"]
        C2["Dynamic LLM Params"]
        C3["Retriever <br>(Weaviate DB)"]
        C4["Context Formatter"]
    end

    subgraph "FAQ Workflow Components"
        D1["Retriever <br>(Weaviate DB)"]
        D2["Context Formatter"]
    end

    subgraph "Backend & Generation"
        E["LLM <br>(Together.ai API)"]
        F["Flask API <br>(Embeddings/Reranking)"]
    end

    subgraph "Observability"
        G["Arize Phoenix Tracing"]
    end

    A --> B
    B -- "Product Query" --> C
    B -- "FAQ Query" --> D

    C --> C1 --> C2
    C --> C3 --> C4

    D --> D1 --> D2

    C4 --> E
    D2 --> E

    E --> A

    C3 --> F
    D1 --> F
    
    B -- "trace" --> G
    C -- "trace" --> G
    D -- "trace" --> G
    E -- "trace" --> G
```

---



## Performance Optimizations and Monitoring

### Token Efficiency
- **Simplified Routing** → Token usage reduced from 250+ → ~130 per classification.
- **Smart Retrieval** → Semantic search without metadata saves ~1500 tokens.
- **Context Optimization** → Dynamic context window management.

---

### Monitoring & Analytics
**Real-time Observability & Cost Tracking**
- Token usage per query
- Model cost optimization
- Performance vs. cost trade-off analysis


### Real-time Monitoring
<table>
  <tr>
    <td><img src="optimizing_chatbot/images/trace_view_36_queries.png" width="600" height="900"></td>
    <td><img src="optimizing_chatbot/images/q4_trace_details.png" width="600" height="900"></td>
  </tr>
  <tr>
    <td><img src="optimizing_chatbot/images/faq_prod_labeling_token_comp.png" width="600" height="900"></td>
    <td><img src="optimizing_chatbot/images/traces.png" width="600" height="900"></td>
  </tr>
</table>

---

### 📊 Key Performance Metrics  

| **Metric** | **Result** |
|-------------|------------|
| **Classification Accuracy** | **95%+** |
| **Average Response Time** | **< 2 seconds** |
| **Token Efficiency** | **40% reduction** |

---

## 🔬 Optimization & Observability Analysis

A key focus of this project was to analyze and optimize the RAG pipeline's performance and cost. By implementing Arize Phoenix, we gained deep insights into every component.

### The Challenge: Cost vs. Performance

The initial "Standard" RAG pipeline used an LLM to generate detailed metadata filters for every product query. While highly accurate, this approach was token-intensive and slow.

### The Solution: A Simplified, Hybrid Approach

We introduced a "Simplified" pipeline that bypasses LLM-based filter generation for most queries, relying instead on direct semantic search. This dramatically reduces cost and latency.

| Metric Comparison | Standard Pipeline | Simplified Pipeline | Outcome |
| :--- | :---: | :---: | :---: |
| **Avg. Tokens (Query Pre-processing)**| ~1,450 tokens | **0 tokens** | ✅ **Drastic Cost Reduction** |
| **Avg. Latency** | High | **Low** | ✅ **Faster Responses** |
| **Accuracy** | Very High | High | ☑️ **Acceptable Trade-off** |

### Phoenix Tracing in Action

The Phoenix dashboard provides a granular view of the entire request lifecycle, from the initial query routing to the final LLM generation.

| | |
| :---: | :---: |
| ![Full Trace](optimizing_chatbot/images/trace_view_34.png) | ![Trace Details](optimizing_chatbot/images/q1_trace_details.png) |

This level of observability is critical for building production-ready AI systems, enabling continuous improvement and robust debugging.

---


## 💻 Technology Stack

| Category | Technology |
|---|---|
| **Core Language** | Python |
| **LLM Backend** | Together.ai API (Llama 3.1 & 3.2) |
| **Vector Database**| Weaviate (Embedded) |
| **Web UI** | Gradio |
| **API Server** | Flask |
| **Observability** | Arize Phoenix |
| **Libraries** | OpenTelemetry, Weaviate-client, OpenAI, Transformers |


---

## 📂 Project Structure

```
fashion-advanced-rag-pipeline-phoenix/
├── dataset/                    # Pre-processed product and FAQ data (.joblib)
├── images/                     
├── optimizing_chatbot/         # Notebook and scripts for optimizing the RAG pipeline
│   ├── optimize_rag.ipynb
│   └── images/                 # Phoenix tracing screenshots
├── phoenix_rag_pipeline/       # Notebook and scripts related to Phoenix tracing
├── fashion_assistant_rag_pipeline.ipynb # Main notebook for the initial RAG implementation
├── flask_app.py                # Flask server for model inference (embeddings, reranking)
├── weaviate_server.py          # Script to initialize and manage the Weaviate instance
└── utils.py                    # Utility functions for LLM calls, chatbot logic, etc.
```

---

## 🚀 Getting Started

Follow these steps to set up and run the project locally.

### 1. Prerequisites

* Python 3.9 or higher
* A virtual environment tool (e.g., `venv`, `conda`)

### 2. Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/nabeelshan78/fashion-advanced-rag-pipeline-phoenix.git
    cd fashion-advanced-rag-pipeline-phoenix
    ```

2.  **Create and activate a virtual environment:**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install the required dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Set up API Keys:**
    You will need an API key from [Together.ai](https://together.ai/). Create a `.env` file in the root directory and add your key:
    ```
    TOGETHER_API_KEY="your_together_ai_api_key"
    ```

### 3. Running the Application

1.  **Start the Weaviate Server & Flask API:**
    Run the main Jupyter notebook (`fashion_assistant_rag_pipeline.ipynb` or `optimizing_chatbot/optimize_rag.ipynb`). The cells in the notebook will programmatically start the embedded Weaviate instance and the Flask background thread.

2.  **Launch the Chatbot UI:**
    Execute the final cells in the notebook that contain the `gradio` app code. This will launch the web interface.

    ```python
    # Example from the notebook
    demo.launch(server_port=8081, share=True)
    ```

3.  **Open the Phoenix UI:**
    The notebook will also provide a URL to view the tracing data in the Phoenix UI, allowing you to monitor your chatbot's performance in real-time.

---


## Future Work

* **Knowledge Graph Integration**: Build a knowledge graph of fashion items and styles to answer more complex, multi-hop questions (e.g., "Find me shoes that match the summer dress I bought last month").
* **Fine-tuning**: Fine-tune a smaller, open-source LLM on the specific conversational data from the fashion domain to improve response quality and reduce reliance on larger models.

---
