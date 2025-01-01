# AI-Powered Chatbot for Learning Data Structures and Algorithms (DSA)

## Abstract
This project explores the use of Artificial Intelligence (AI) to improve traditional learning methods through an AI-powered chatbot. Utilizing Natural Language Processing (NLP) and machine learning algorithms, this chatbot aims to:

- Develop a real-time, interactive learning tool for students.
- Assist students in understanding concepts, solving problems, and clarifying doubts.
- Prepare students for Online Assessments and technical interviews for their Integrated Work Study Programme (IWSP).
- Focus specifically on the topic of Data Structures and Algorithms (DSA) to enhance the learning experience by staying on-topic.

## Methodology

### 1. Data Extraction for RAG
- **Tool**: RecursiveUrlLoader
  - Scrapes content from the W3Schools DSA section to a depth of two levels.
  - Ensures comprehensive data gathering while staying within the designated domain.
- **Function**: `extract_main_content`
  - Parses HTML structure to isolate specific content within the `<div>` element with `id="main"`.

### 2. Text Splitting - HTML Content
- **Tool**: `HTMLHeaderTextSplitter`
  - Breaks content based on headers while preserving logical structure.
  - Attaches metadata (e.g., source URL) to each section for improved accuracy and traceability.

### 3. Vectorstore Retriever
- **Algorithm**: Maximal Marginal Relevance (MMR)
  - Balances relevance and diversity to avoid redundancy.
- **Parameters**:
  - `k=5`: Number of documents returned to the user.
  - `fetch_k=10`: Number of documents passed to MMR before filtering.
  - `lambda_mult=0.5`: Balances relevance and diversity.

### 4. LangGraph Workflow
A framework for managing workflows and decision-making processes:

- **Retrieve Documents**: Initial step to gather relevant documents based on user queries.
- **Grade Documents**: Evaluates relevance of each document to the user’s query, ensuring only relevant data is used.
- **Generate with RAG**: Combines user queries with retrieved context to generate answers.
- **Generate with Base LLM**: Falls back to the base LLM if no relevant documents are found or RAG fails.
![download (2)](https://github.com/user-attachments/assets/12e1e6c4-e33f-43aa-a415-bcf1dc62813d)

### 5. Chat History Management
- **Tool**: `create_history_aware_retriever`
  - Contextualizes user queries based on past interactions.
  - Reformulates standalone questions for processing by RAG or Base LLM.
- **Prompt Example**:
  ```plaintext
  Given a chat history and the latest user question, formulate a standalone question 
  which can be understood without the chat history. Do NOT answer the question, 
  just reformulate it if needed and otherwise return it as is.
  ```

### 6. Graphical User Interface (GUI)
- **Tool**: Gradio
- **Example Code**:
  ```python
  import gradio as gr

  def gradio_chatbot(message, history):
      # Call your existing function to get the response
      example = {"input": message}

      # Call your custom agent function
      result = predict_custom_agent_local_answer(example)

      # Return the response
      return result["response"]

  # Create the ChatInterface in Gradio
  gr.ChatInterface(gradio_chatbot).launch()
  ```

## Results

### 1. Redirecting Off-Topic Inputs
- Off-topic inputs are effectively recognized and redirected.
- Example: For input "What is your favorite song?", the chatbot responds: "Please ask DSA-related questions only."
![download](https://github.com/user-attachments/assets/dfc20063-a7fc-4d24-a859-1b47e2793176)

### 2. Fallback on Base LLM
- Seamless fallback to the base LLM if RAG fails to generate a satisfactory answer.
![download (1)](https://github.com/user-attachments/assets/2b0d0c6d-d42e-4b68-b60d-bcdd1dc172cf)

### 3. Chat History
- Retains context from earlier interactions, enabling coherent responses.
![download (2)](https://github.com/user-attachments/assets/6ceffc72-894c-4de0-bdeb-16f3b878dc4f)

### 4. Follow-Up Questions
- Engages students with follow-up questions to promote active learning.
![download (3)](https://github.com/user-attachments/assets/fa3f10f2-bfbc-4b0a-8f73-00e33428f1dc)

## Conclusion
This project successfully implemented a dynamic chatbot system integrating:

- **Retrieval-Augmented Generation (RAG)**
- **Large Language Models (LLMs)**
- **LangGraph for modular workflow design**
- **Gradio for GUI**
