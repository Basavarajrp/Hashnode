---
title: "Train Chat GPT on your custom data 🚀"
seoTitle: "Next-Level AI Applications: A Guide to Using Embeddings"
seoDescription: "Unlock the transformative power of embeddings in AI using ChatGPT with Python and harness the efficiency of vector search with the Chroma database."
datePublished: Mon Jul 31 2023 19:07:25 GMT+0000 (Coordinated Universal Time)
cuid: clkr8sho7000d09l44mj53vb6
slug: train-chat-gpt-on-your-custom-data
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1690828973876/8c4727db-5e22-4d2c-86b2-f7456243ab32.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1690830114156/6ae70577-c313-482f-b0cd-6233869cfff5.png
tags: ai, python, nlp, gpt-3, python-projects

---

I was using ChatGPT for some of my causal queries. I realized that it is a trained model, it is trained on the huge collection of internet data using supercomputers.

### **What happens if we ask a question to this language model on data or information that it has not been specifically trained on?**

I asked ChatGPT about Chroma DB, and here's what it said! 😅.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690821211996/fddd1c29-6221-4435-8a4e-2ecea73d24cd.png align="center")

<mark>ChatGPT is a trained model with vast amounts of internet data and is highly proficient in contextual understanding.</mark> So it can be used for various applications like chatbots, virtual assistants, customer support systems, and more.

Even we can use this model on our own custom dataset for our own use cases.

<mark>Companies can utilize this model to train it on their own data and employ it as a chatbot, enabling responses generated from the trained dataset.</mark>

---

## Let's try to build a doc-chat(chat with your document) app ⚒️.

<mark>Let's utilize a PDF file to train the GPT model and enable it to engage in chat conversations.</mark>

I have created a flow diagram to offer a clearer understanding of the application we are going to build now.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1690824190767/ffe673a7-0724-4d84-9dd2-0c9ab8600601.png align="center")

**Let's break down the above flow diagram into words:**

1. We use Python to read PDF files and store them in a 'vector database' called Chroma DB.
    
    Vector search databases are useful for applications that require search suggestions and recommendations.
    
    <mark>In our case, we read PDF documents, convert them to vectors, and save these vectors in the database. Using these vectors, we can perform search operations on related data.</mark>
    
2. <mark>When a user asks a question, we query the vector database to retrieve the most relevant fields from the documents.</mark>
    
3. After obtaining highly related information based on the searched key, we can use the ChatGPT model to better understand the context of the data.
    
    <mark>By providing the model with this retrieved search information, it gains an understanding of the data's context.</mark>
    
4. Once the model understands the context, we can ask any question related to the retrieved information.
    

---

## Let's do some coding 👨‍💻.

We gonna use the following stack for our project:

1. Python.
    
2. Chroma DB (vector search database).
    
3. OpenAI `embedding model` API to convert text to vectors.
    
4. OpenAI `gpt-3.5-turbo` API to generate the relevant response/chats.
    

**Let's import some modules.**

```python
import os
from pypdf import PdfReader
import openai
import chromadb
from chromadb.utils import embedding_functions
import random
```

**Process the PDF content to vectors and store it in DB.**

```python
client = chromadb.PersistentClient(path=chromadb_path)
openai_ef = embedding_functions.OpenAIEmbeddingFunction(api_key=openai_api_key, model_name=openai_model_name)
collection = client.get_or_create_collection("my_searchable_collection", embedding_function=self.openai_ef)

def save_data_to_chrom_db(text_chunks):
        try:
            for text in text_chunks:
                collection.add(documents=text.strip(), ids=[str(random.randint(1, 1000000))])
        except Exception as e:
            print('error:======', e)

def pdf_handler(file_path):
        reader = PdfReader(file_path)
        page = reader.pages[0]
        text_line_by_line = []

        for i in range(len(reader.pages)):
            page = reader.pages[i]

            # extract the text line by line and save it in the array lines
            lines = page.extract_text().splitlines()
            filtered_lines = [item for item in lines if item.strip()]
            text_line_by_line.extend(filtered_lines)

        save_data_to_chrom_db(text_line_by_line)
```

1. First, we create the DB client connection.
    
2. This client accepts two parameters: the name of the `collection` to store the data and an `embedding_function` that converts the text to vectors before saving the data.
    
3. If we don't pass the `embedding_function` explicitly Chroma DB uses the [**Sentence Transformers**](https://www.sbert.net/) `all-MiniLM-L6-v2` model to create embeddings.
    
4. <mark>In our case, we are utilizing OpenAI's embedding model API to perform vector embedding. We achieve this by instructing the </mark> `chromadb.utils` <mark>helper function by providing the </mark> `openai_ef` <mark>helper function, which is connected to OpenAI's embedding model API using the appropriate API key.</mark>
    
5. In the next step, the `pdf_handler` function reads the file data line by line and creates small chunks of text. These chunks are then passed to the `save_data_to_chrom_db` function, where they are converted into vectors and stored in the database.
    

**We create a search function that reads the data from the database and passes it to the GPT model. By doing so, we aim to retrieve more relevant and meaningful information based on the user's queries or questions.**

```python
def search_data_from_chrom_db(text):
        try:
            search_result = collection.query(query_texts=[text], n_results=10)
            return search_result.get("documents")[0]
        except Exception as e:
            print('error:======', e)

def handleSearch(search_text):
        search_result = search_data_from_chrom_db(search_text)
        
        prompt = f"consider these statements this are search results from the db {search_result}. \
                \n Please give me the related information that the search results have about {search_text} in a short and descriptive way as an answer to user and avoid additional sentences in the responce."

        openai.api_key = openai_key # pass your generated openai key
        response = openai.ChatCompletion.create(
                                            model="gpt-3.5-turbo",
                                            messages=[{"role": "user", "content": prompt}]
                                            )
        return response.choices[0].message.content
```

1. `search_data_from_chrom_db(text)`: This function searches the database (Chroma DB) for documents related to the given `text`. It queries the database and returns the most relevant document based on the search text.
    
2. `handleSearch(search_text)`: This function handles the search process. It calls `search_data_from_chrom_db` to get the relevant data from the database based on the `search_text`. It then forms a prompt using this data and sends it along with `search_text` to GPT-3.5 Turbo model.
    
3. `prompt`: <mark>The prompt is the instruction given to the GPT model. It asks the question provided by the user, and it includes the context obtained from the search results retrieved from Chroma DB. By combining the user's question and the context of the data, the prompt guides the GPT model to generate a relevant and concise answer from the document that we used.</mark>
    

---

## Conclusion:

We can use the ChatGPT model on our own custom data by combining it with your database and user inputs. Here's a brief overview of the steps:

1. Set up a database to store custom data. In this case, we used Chroma DB to store PDF vector data.
    
2. Create functions to handle data processing. In our case, we have a function (`pdf_handler`) to read PDF files and convert them into vectors, which are then stored in the database.
    
3. Implement a search function (`search_data_from_chrom_db`) to query the database and retrieve relevant data based on user input.
    
4. Construct a prompt that includes the user's question and the context from the retrieved data. This prompt is used to instruct the ChatGPT model and generate a response relevant to the user's query.
    
5. By passing the prompt to the model, we can obtain a response that combines the user's query and the context provided by the database.
    

> **You can find this entire code in my repo :** [**https://github.com/Basavarajrp/Doc-GPT-App**](https://github.com/Basavarajrp/Doc-GPT-App)

### Feel free to connect and share your valuable inputs in the comment section. See you all in the next blog! 👋.

**Tq, Happy Coding 👨‍💻.**