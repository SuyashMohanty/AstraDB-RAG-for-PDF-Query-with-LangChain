# 🚀 AstraDB RAG for PDF Query with LangChain

This project demonstrates how to build a powerful Retrieval Augmented Generation (RAG) system to query the contents of a PDF document. It uses **Astra DB** (Serverless Cassandra with Vector Search) as a scalable vector database, **LangChain** to orchestrate the workflow, **Hugging Face** models for generating text embeddings, and **Groq** for fast LLM inference to answer questions.

---
## ✨ Features

* **PDF Document Querying**: Enables you to "chat" with your PDF files by asking questions in natural language. The example uses a `budget_speech.pdf` file.
* **Astra DB Vector Store**: Leverages Astra DB, a serverless Cassandra database, for robust and scalable storage and retrieval of vector embeddings.
* **LangChain Orchestration**: The entire RAG pipeline—from document loading and splitting to querying—is built and managed using the LangChain framework.
* **High-Quality Embeddings**: Utilizes `HuggingFaceEmbeddings` with the `all-MiniLM-L6-v2` model to create dense vector representations of the document text.
* **Fast LLM Inference**: Uses `ChatGroq` with the `gemma2-9b-it` model for efficient and high-speed generation of answers.
* **Simple QA Interface**: Implements an interactive loop that allows users to ask multiple questions about the document's content.

---
## ⚙️ How it Works

The application follows a standard RAG pipeline to allow for querying the PDF document:

1.  **Setup and Initialization**: The script starts by setting up the necessary credentials for Astra DB and Groq. It initializes a connection to your Astra DB instance using `cassio`.
2.  **Document Loading**: The text content is extracted from a local PDF file (`budget_speech.pdf`) using the `PyPDF2` library.
3.  **Text Splitting**: The extracted raw text is divided into smaller, more manageable chunks using LangChain's `CharacterTextSplitter`. This is crucial for creating effective and contextually relevant embeddings.
4.  **Vector Embedding and Storage**:
    * The `HuggingFaceEmbeddings` model is used to convert each text chunk into a dense vector embedding.
    * These embeddings, along with their corresponding text, are stored in a `Cassandra` vector store table within your Astra DB database. The `astra_vector_store.add_texts` function handles this process.
5.  **Question Answering**:
    * A `VectorStoreIndexWrapper` is created to facilitate querying.
    * The user is prompted to enter a question in a `while` loop.
    * For each question, the system first performs a similarity search in the Astra DB vector store to retrieve the most relevant text chunks.
    * These retrieved chunks (the context) and the original question are then passed to the `ChatGroq` LLM.
    * The LLM generates a final answer based on the provided context, which is then displayed to the user.

---
## 📋 Requirements

* Python 3.x
* Jupyter Notebook or an equivalent environment.
* An **Astra DB** serverless database. You will need your **Database ID** and an **Application Token**.
* A **Groq API Key**.
* A PDF file to query (the notebook is configured for a file named `budget_speech.pdf` in the same directory).
* Required Python libraries: `cassio`, `datasets`, `langchain`, `langchain_groq`, `langchain_huggingface`, `PyPDF2`, `openai`, `tiktoken`.

---
## 🛠️ Setup & Installation

1.  **Clone the repository or download the `experiment.ipynb` notebook.**

2.  **Place Your PDF**: Make sure your PDF file (e.g., `budget_speech.pdf`) is in the same directory as the notebook.

3.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows use `venv\Scripts\activate`
    ```

4.  **Install the required Python libraries:**
    The notebook uses `!pip install` commands, but you can install them directly for a clean setup:
    ```bash
    pip install cassio datasets langchain langchain_groq langchain_huggingface PyPDF2 openai tiktoken
    ```

5.  **Configure Credentials**:
    * Open the `experiment.ipynb` notebook.
    * Find the cell designated for setup and replace the placeholder values with your actual credentials:
        ```python
        ASTRA_DB_APPLICATION_TOKEN="Your_AstraDB_Application_Token"
        ASTRA_DB_ID="Your_AstraDB_Database_ID"
        Groq_API_KEY="Your_Groq_API_Key"
        ```

---
## ▶️ Usage

1.  **Open and Run the Notebook**:
    * Start your Jupyter Notebook server and open `experiment.ipynb`.
    * Run the cells sequentially from top to bottom.

2.  **Indexing**:
    * The notebook will first load the PDF, split it into chunks, and then add them to your Astra DB vector store. You will see a confirmation message like "Inserted 50 headlines."

3.  **Ask Questions**:
    * Once the indexing is complete, you will be prompted by an input field in the last cell to ask a question.
    * Type your question about the document and press Enter.
    * The system will display the generated answer and the most relevant document chunks that were used to create it.
    * You can continue asking questions in the loop or type `quit` to exit.

---