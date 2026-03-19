# PN_RagPipeline
This is a rag pipeline that is meant to be integrated into a business platform to provide customer services.
The files uploaded in this repository are versions of the RAG Pipeline that builds from a basic RAG with minimal features in version 1 to a RAG pipeline that encompasses more advanced features to enhance responses.
Codes from this repository follow closely the codes by @riancemartin in his rag-from-scratch series. 

## A Brief Overview of RAG
Retrieval-Augmented Generation (RAG) is a technique that enhances the capabilities of Large Language Models (LLMs) by incorporating external knowledge sources. Instead of relying solely on the model’s pre-trained parameters, RAG allows the model to retrieve relevant information from custom documents or databases at query time. This makes it possible to generate more accurate, context-specific, and up-to-date responses, especially in specialized domains.
By integrating retrieval with generation, a RAG pipeline enables the LLM to access domain-specific content dynamically—without the need for retraining—offering more relevant and grounded answers based on the imported knowledge base.

## Version Descriptions 
The text below documents the code interpretation of each version as well as the intent of the version and in some cases, its evaluation.

### Version 1
Version 1 represents a basic, working implementation of a RAG (Retrieval-Augmented Generation) pipeline. It explores core RAG concepts such as cosine similarity for vector comparison and tokenization of both queries and documents. A key component in this version is the HuggingFaceChatRunnable class—a custom class built to integrate prompt templates and handle user queries using Hugging Face models instead of OpenAI’s APIs. This class was necessary due to differences in how Hugging Face models are structured and queried, requiring a custom interface for compatibility.

### Version 2
Upon understanding the underlying concepts of how RAG works, version 2 acts as a clean up of version 1 and consolidates the RAG pipeline into core essential working parts. It removes redundant components and improves code readability, efficiency, and maintainability. The goal was to isolate and retain only the essential parts of the RAG pipeline, resulting in a cleaner and more modular implementation.

### Version 3 
The first major enhancement to the simple RAG pipeline is showcased in Version 3 in Query Translation. This version uses a Query Translation technique known as Step-Back Generation. This approach refines user queries to improve the relevance of retrieved documents. Specifically, it transforms highly specific queries into more generalized forms, broadening the retrieval scope and increasing the probability of capturing useful context. This often leads to more balanced and comprehensive answers. The step-back technique plays a critical role in addressing under-specification and narrow context issues in traditional RAG implementations.

### Version 4 
The next major enhancement is the incorporation of routing. While advanced routing strategies can direct queries to different databases or pipelines, this version implements a simpler form of intent-based routing. Intent detection is a technique aimed at figuring out whether the user is trying to make a comparison, sought out advice, or seeking to obtain a procedure to follow among other intents. By figuring out the core intent of the user, the RAG pipeline is able to create tailored responses that addresses the concerns of the user directly. 

### Version 5
This version aims to enhance the RAG pipeline by improving the indexing process of the working algorithm. The chosen algorithm used in this version is RAPTOR. RAPTOR clusters semantically similar knowledge chunks and uses an LLM to summarize each cluster into a unified knowledge representation. This enables the retrieval of more context-rich and synthesized content, potentially improving the coherence and depth of the final generated response. However, this approach is computationally expensive. Since LLMs are used to generate summarized clusters, RAPTOR significantly increases token usage and, consequently, operational costs. Despite its theoretical advantages, RAPTOR's practical application may be limited due to its resource intensity.

### Version 6
This version uses ColBERT instead of RAPTOR as a enhancement to the indexing segment of a RAG pipeline. ColBERT (Columnar BERT) is a a dense retrieval model designed for fine-grained semantic search. Unlike conventional embedding methods (e.g., Sentence Transformers), ColBERT encodes each token in a passage individually, allowing for late interaction between query and document tokens. This enables more precise matching across different parts of a document corpus, improving the retrieval of relevant information that can be synthesized into coherent answers.
The implementation leverages the RAGatouille library, maintained by Olivier Moindrot (@omoindrot) and collaborators, which provides a streamlined interface for deploying ColBERT-based retrieval systems. While ColBERT offers significant advantages in retrieval accuracy and often reduces downstream LLM token usage (by surfacing more relevant, compact context), it comes with trade-offs. Specifically, it demands greater computational resources, particularly GPU acceleration for efficient retrieval, and has a larger memory footprint due to its token-level embeddings. Nonetheless, for applications where retrieval precision and response quality are paramount, ColBERT remains a compelling choice.

### Version 7
![Flowchart](https://github.com/user-attachments/assets/59becc5e-219c-4451-99fd-1b96793636a2)
In this final version of the RAG pipeline, a self-RAG structure is implemented according to the flowchart depicted above. This fully functioning RAG pipeline incorporates the superior strategy of ColBERT in indexing above. To further improve output reliability, the pipeline performs checks at both the retrieval and generation stages to minimize hallucinations and irrelevant results. It is also interactive—capable of asking follow-up questions and rewriting user queries to incorporate newly gathered information. While this approach does incur higher token usage, the trade-off is justified by the increased factual alignment, relevance, and overall answer quality.
