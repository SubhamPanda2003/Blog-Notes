---
title: "Retrieval-Augmented Generation (RAG): Chunk, Embed, Retrieve Explained"
datePublished: 2026-05-23T21:04:35.854Z
cuid: cmpiu70uk00c01smtho57e77n
slug: retrieval-augmented-generation-rag-chunk-embed-retrieve-explained
cover: https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/bcf77bf3-e28f-4a0a-abad-fe8afe17693c.png

---

* * *

What is Retrieval-Augmented Generation (RAG)? The name says it: you retrieve relevant information and then generate responses based on that information. RAG is useful because large language models (LLMs) often lack access to—or awareness of—specific documents (for example, a particular book or a 50‑page report) and can produce generic or hallucinatory answers when a precise response is required. By grounding generation in retrieved, relevant passages, RAG helps ensure answers are specific, accurate, and tied to the source material.

Explaining briefly on why we need a RAG on a document.  
Firstly an llm might not be aware of the specific information that we have. For example it might be a specific book or a 50 page document that its unaware of.  
Secondly, llms tends to hallucinate and give generic answer when we are expecting specific answer.  
The above problems are handled by a RAG, It gives the specific information that the llm needs to answer a particular user query.

LLM Response vs a RAG response

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/f7f7880a-4977-443d-b788-31d3a2f0294a.png align="center")

The most interesting part of the RAG is the Information Retrieval part. How do you store the info and then give the meaningful response in return.

The information retrieval works on the below steps:

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/02c869e0-3b09-4674-b987-d41b7306319d.png align="center")

First the data should be structured. This is where chunking becomes handy. It is just breaking the raw data into small meaning full closely knit parts. The 50 page document can be broken based on themes or sections and sub sections, etc.

After the chunking is done, we convert the information into embeddings. What is an embedding. Embedding is just converting a certain document into a vector of numbers that preserve the meaning.  
After we get the vectors we store them in a vector DB.

When we get the user query we break it into keywords. The keywords are converted to embedding vectors with the same algorithm we used above to convert the chunks to vectors.

There are two types of search we perform the vector DB.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/2c0c9f68-113b-4627-82db-d3e7f6a2867e.png align="center")

Exact search is apples to apples match. If we are finding a word, there should be a exact one to one match.

The semantic search is based on meaning. Twice and double have the same meaning so they will be a match in the semantic search.

Most RAG system we use a hybrid of both the approach. Only exact search might leave some more relevant info and only semantic search might return some unnecessary information that might lead to llm hallucination.

The vectorDB search occurs by a cosine similarity score.  
According to wikipedia : "**Cosine similarity** is **<mark class="bg-yellow-200 dark:bg-yellow-500/30">a metric used to measure how similar two vectors are, regardless of their magnitude</mark>**. By calculating the cosine of the angle between vectors, it determines if they point in roughly the same direction. In fields like AI and information retrieval, it is highly useful for comparing text and documents. "

Vector Cosine Product

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/d1817a07-3e27-4eac-83d4-787bcae5a42c.png align="center")

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/3f438033-8f0f-44f7-90ff-273c4543c61a.png align="center")

Cos 0 is 1. So when theta is close to 0 then its the maximum value for Cosine.

This score is used to compare the similarity of two embeddings. So the embeddings with similar meaning would have closer angles to 0. So same meaning keyword embeddings would lie in the same direction.

After we get the top k result based on similarity we rerank them based on the context using a reranker model. This is an optional stage but is required when the document size becomes huge.  
The top k result that we got we pass it to the llm to get the refined required response.

Some questions worth wondering:

1.  How does an embedding model work. How does two similar meaning text have similar vector embeddings? Why cosine similarity why not a sine product?
    
2.  How does a reranker model works? On what basis does this model rearranges the chunks priority?
    
3.  What if the the document size is huge with unrelated pages and sections having same meaning keywords?
    
4.  What would be the issue if we use the above rag approach on a codebase and try to get the lineage from a function to another?
    

The above questions we will discuss in the upcoming articles. So stay tuned.

* * *