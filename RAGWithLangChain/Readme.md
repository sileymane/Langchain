# RAG Solution

## Why we need RAG
Retrieval Augmented Generation = LLM reasoning + external knowledge

for example, if a user ask a question, RAG find first the revelant informations then uses that data to help the model create a responses this make the answer more reliable and based on facts not just what the model learn during training 

1) RAG powers grounded, explainable AI systems 
 Examples
   1) Customer support chatbots using company manuals 
   2) HR assistants answering from policy documents 
   3) Research copilots citing academic papers 
   
2) Key Takeways:
   1) RAG = model reasoning + external evidence 
   2) make the LLMs **reliable**, **current** and **verifiable**

## Anatomy of RAG PIPELINE 
#### From data to intelligence: the RAG Pipeline
RAG connects your knowledge base to an LLM in a structured way 
## Pipeline schema

steps:
1) Documents
2) Chunking
3) Embeddings
4) Vector Store
5) Retriever
6) LLM




```mermaid
%%{init: {"flowchart": {"useMaxWidth": false, "nodeSpacing": 55, "rankSpacing": 65, "padding": 20}, "themeVariables": {"fontSize": "20px"}} }%%
flowchart TB
    subgraph INDEX["1 - Indexing (offline)"]
        direction TB
        A["Sources<br/>PDF, HTML, MD, DB"] --> B["DocumentLoader"]
        B --> C["TextSplitter<br/>chunks + overlap"]
        C --> D["Embeddings model"]
        D --> E[("Vector store<br/>Chroma / FAISS / pgvector")]
    end

    subgraph QUERY["2 - Retrieval + Generation (online)"]
        direction TB
        Q["User question"] --> QE["Embeddings model"]
        QE --> R["Retriever<br/>top-k similarity"]
        R --> CTX["Context chunks"]
        CTX --> P["Prompt template<br/>context + question"]
        Q --> P
        P --> LLM["LLM<br/>claude-sonnet-5"]
        LLM --> ANS["Answer + sources"]
    end

    E -.->|similarity search| R

    classDef box fill:#eef4ff,stroke:#3b6cb7,stroke-width:2px,color:#12213a
    classDef store fill:#fff3e0,stroke:#c77700,stroke-width:2px,color:#3a2a12
    class A,B,C,D,Q,QE,R,CTX,P,LLM,ANS box
    class E store
```

###  RAG with LangChain 
LangChain makes RAG Modular, langchain abstracts RAG into reusable building blocks 


```mermaid
%%{init: {"flowchart": {"useMaxWidth": false, "nodeSpacing": 45, "rankSpacing": 55, "padding": 18}, "themeVariables": {"fontSize": "20px"}} }%%
flowchart LR
    EMB["<b>1 - Embeddings</b><br/>transform text into<br/>vector form"]
    VS["<b>2 - Vector Stores</b><br/>index and retrieve<br/>context"]
    CH["<b>3 - Chains</b><br/>orchestrate the<br/>flow of data"]
    AG["<b>4 - Agents</b><br/>decide when and how to<br/>use retrieval"]

    EMB --> VS --> CH --> AG

    classDef emb fill:#b7a49e,stroke:#8d7a74,stroke-width:2px,color:#2b2b2b
    classDef vsx fill:#cbb87c,stroke:#a3914f,stroke-width:2px,color:#2b2b2b
    classDef chn fill:#8fc6a9,stroke:#5f9c80,stroke-width:2px,color:#2b2b2b
    classDef agt fill:#c5a2cc,stroke:#9a76a3,stroke-width:2px,color:#2b2b2b

    class EMB emb
    class VS vsx
    class CH chn
    class AG agt
```

| # | Block | Role | LangChain API |
|---|-------|------|---------------|
| 1 | **Embeddings** | Transform text into vector form | `Embeddings` |
| 2 | **Vector Stores** | Index and retrieve context | `VectorStore` |
| 3 | **Chains** | Orchestrate the flow of data | `Runnable` / LCEL |
| 4 | **Agents** | Decide when and how to use retrieval | `AgentExecutor` |


Langchain provide ready made chains:

* **RetrivalQA** : One shot question answering from documents (retrieves relevant content and generate direct answer)
*  **ConverstionalRetrievalChain** : context aware multi-turn chat (allow the model to retain context across multiple turns and deliver a true chat like experience)
  
Both chains manage retrival, context injection and response generation 

#### Integrating RAG with Agents 
Agents can use RAG as tools to: 
* Search company knowledge bases
* Answer HR or product questions 
* Plan multi-step reasoning accross sources 

### Real world Use case 
With Langchain RAG, treams can build:
* **Entreprise search**: systems grounded in internal documents 
* **Contextual chatbots**: powered by live company data 
* **reseaarch copilots** : that summarize and cite sources 

### Key takeaway: 
1) Langchain make RAG pratical and accessible 
2)  it offers pre bult components, flexible customization and integration with your existing systems 
3)  Langchain = plug and play for RAG workflows