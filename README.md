# TMKOC_RAG

This RAG(Retrieval Augmented Generation) model is developed using [Ollama](https://ollama.com/) models, speciafically I used [LLAMA2](https://ai.meta.com/blog/large-language-model-llama-meta-ai/) from Ollama repositories. I tried other models - such as [Mistral-7B](https://mistral.ai/news/announcing-mistral-7b/), [LLAMA3](https://www.llama.com/) , but LLAMA2 seems to work fine in terms of speed and accuracy of the outputs. 

The RAG is based on the very popular Indian TV series [**Taarak Mehta Ka Ooltah Chashmah**](https://www.google.com/search?gs_ssp=eJzj4tLP1TcwtSjIMy0wYPTiKUlMLErMVshNzShJBABlqAgq&q=taarak+mehta&rlz=1C5CHFA_enIT994IT994&oq=taarak+m&gs_lcrp=EgZjaHJvbWUqEAgCEC4YgwEY1AIYsQMYgAQyEAgAEAAYgwEY4wIYsQMYgAQyEAgBEC4YgwEY1AIYsQMYgAQyEAgCEC4YgwEY1AIYsQMYgAQyBggDEEUYOTIHCAQQLhiABDIGCAUQRRg8MgYIBhBFGDwyBggHEEUYPNIBCDI1NDBqMGo3qAIAsAIA&sourceid=chrome&ie=UTF-8), which has been running since 2008 and has aired over 4000 episodes. The datasets for first ~4k episodes are available on [Kaggle](https://www.kaggle.com/datasets/rishabhbhartiya/taarak-mehta-ka-ooltah-chashmah-episode-dateset/data). 

I used this dataset to extract the episode title, description and their duration to create feature / text corpus by combing them all in the format : 
``
Episode Title : Jethalal sees a ghost ; Episode description : Jethalal after a heavy work day .... ; Episode duration : 19 min \n 
``

The text corpus are then used to extract their embeddings using the LLAMA2 model. 

I created a textual prompt format to spit out the output to user's queries : 

``
template = """ 
``
<br/>
``
Answer the questions based on the context provided. If you don't know the answer respond with "I don't know."
``
<br/>
``
Context : {context}
Question: {question}
"""
``

So eg. If the user provides a ``query : Roast the resume``. The output of the models is ``I don't know. I cannot roast the resume as it is not provided in the context. Please provide the resume you want me to roast, and I will be happy to help.``

I use [FAISS - Facebook AI Similarity Search](https://ai.meta.com/tools/faiss/), to store the embeddings by creating a vector database and then use it store it elsewhere. 
One can also use the vector database to create external application. One good example is [here](https://python.langchain.com/docs/integrations/vectorstores/faiss/). 
