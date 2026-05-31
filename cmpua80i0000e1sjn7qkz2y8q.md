---
title: "Why RAG Fails on Codebases — And How ReAct Agents Fix It"
datePublished: 2026-05-31T21:18:43.850Z
cuid: cmpua80i0000e1sjn7qkz2y8q
slug: why-rag-fails-on-codebases-and-how-react-agents-fix-it
cover: https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/0bde46e3-32db-4dd1-8078-c708bcc1b4f0.png

---

Rags on Codebases might seem to be a good idea at first, to some extent it is but the underlying trade offs tells a different story. A brief on how RAG works is:

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/9949e29b-dd93-4631-a56b-242b695b5508.png align="center")

For more details on RAG there is a detailed article on the working on [RAGs and its working](https://blog.subhampanda.com/retrieval-augmented-generation-rag-chunk-embed-retrieve-explained).  
  
The main aim of the vectorDB and retrieval is to prevent the LLM from hallucinating. So it should not spit out a whole document, in this case its the codebase nor should it miss out on important information.  
Lets take a JAVA code example.  
We have multiple packages, each package has multiple class and each class has multiple functions.  
Each of them are dependent on each other in a intermingled way. Lets take the below example of two classes and four functions.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/bf0b6aea-3291-4152-80bd-b4e76dc6a2e2.png align="center")

For storing the above in vectorDB, two of the most common chunking strategy is either function based chunking or class based chunking. Its trivial to understand the chunking strategy based on the names itself.  
  
When you are retrieving the data or a query related to func-1, the vector similarity search would fetch the details of func-1, may be to some extent func-4 but would start missing details on each dependency we jump to. **The issue is not with the information but for a complete information you need the complete trail with proper lineage.**  
  
In short:  
For code, naive chunking + vector search usually fails because:

*   functions depend on other files
    
*   variable names matter
    
*   imports define relationships
    
*   call chains span modules
    
*   small syntax changes completely change meaning
    
*   embeddings often lose structural information
    

There are multiple algorithms that are made to handle bits and pieces of the above issues, **example: language parsing algorithms like tree sitter, storing the knowledge graph of the codebase in a graph db etc.**  
*Details of all the above algorithm is out of scope of the current post will create a separate post describing each of those algorithm in details.*

***<mark class="bg-yellow-200 dark:bg-yellow-500/30">The solution that I would like to show you is just adding a ReAct agent on the top of your RAG system</mark>***. How does a ReAct agent works is shown below:

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/e80cad12-faa3-4acc-bd79-ba17f40fb356.png align="center")

On the given function dependency, it would give you the information by making a series of vectorDB queries instead of just one.  
Loop 1: Find func: 1, It would get the code and details of function 1. Now there it would find name of function 4\[in observation stage\].  
Loop 2: Find func 4, same as above go to func 2.  
Loop 3: details of func2. It got all the required details compiles them and the loop breaks.

After the loop breaks the agent compiles the result and sends user the required response.  
Without making much changes to the existing architecture, we got the full lineage with all required info for our query on func 1.  
  
RAG systems work well for documents because information is mostly linear. Codebases are different — understanding a single function often requires traversing dependencies, imports, and multi-hop call chains spread across the repository.

By adding an iterative ReAct-style retrieval layer on top of traditional RAG, we can move from isolated semantic search to dependency-aware code understanding, enabling far more accurate and context-complete responses.

But then what are the tradeoffs between a ReAct agent and the existing language parsing algorithm vs storing the knowledge graph of the codebases. Which of them will be the best options in what scenario. These are some of the questions we will discuss in the upcoming posts in details.

* * *