# FineTuned_AgenticRAG_Text_to_SQL

Generates SQL queries from natural language input, utilizing fine-tuning techniques like PEFT and QLoRA, limited context query reconstruction and agentic RAG for dynamic schema retrieval

## Project Overview
  
FineTuned_AgenticRAG_Text_to_SQL is a fine-tuned Retrieval-Augmented Generation (RAG) model designed to generate SQL queries from natural language input using relevant table and column schema. The model leverages meta-llama/Meta-Llama-3-8B-Instruct and is fine-tuned using the b-mc2/sql-create-context dataset, achieving a **training loss of 0.56 and 91% accuracy during evaluation**.

Key features include **Agentic RAG for dynamic schema retrieval, limited context query reconstruction, and efficient fine-tuning techniques like PEFT and QLoRA.**

## Model Architecture and Fine-tuning  
  
**Base Model**: meta-llama/Meta-Llama-3-8B-Instruct   
  
**Dataset**: b-mc2/sql-create-context    
  
**Fine-tuning**: LoRA (Low-Rank Adaptation) and QLoRA techniques were used to fine-tune the model down to a training loss of 0.56.  
  
**BitsandBytes Config**: The model uses 4-bit precision with FP16 for efficient memory management during training and inference.  
  
The fine-tuning process optimizes the model for Text-to-SQL generation, ensuring that it can efficiently generate SQL queries from limited context data, making it suitable for large databases with complex schema.    

## Inference Pipeline
The inference process involves multiple steps, ensuring that SQL queries are accurately generated based on user input and database schema:

1. **Create FAISS Index**: Initialize and embed column descriptions, storing them in FAISS for efficient retrieval.
  
2. **User Query**: A natural language query is sent by the user.

3. **Convert Query to Embeddings**: The user query is converted to embeddings.
   
4. **Agentic RAG Steps**:  
4a. **Search FAISS Index**: Retrieve the top N relevant column descriptions.  

4b. **Retrieve Corresponding IDs**: Fetch the IDs of the corresponding column and table names.  

4c. **Get Column and Table Names**: Extract column and table names based on the retrieved IDs.  

4d. **Create Table Statements**: Construct SQL CREATE statements with relevant column names and data types.  
  
5. **Combine Schema with User Query**: Combine the user query and retrieved schema information with the system prompt.
      
6. **Tokenize and Generate SQL**: Tokenize the enhanced input and generate the SQL query using the fine-tuned LLM model.
     
7. **Execute SQL Query**: Execute the generated SQL query against the database.
    
8. **Display Results**: Format and display the results for the user.  
     
## Model Evaluation
  
The generated SQL queries were evaluated against reference queries using the **meta-llama/Meta-Llama-3-70B-Instruct model as a judge**. The following prompt was used to assess whether the generated queries matched the reference queries:  

**_f"Are these SQL queries functionally the same? Ignore differences in formatting, quotes, or order of clauses if they are not functionally significant.\n\n"
f"Generated Query:\n{generated_query}\n\n"
f"Reference Query:\n{reference_query}\n\n"
f"Respond with only '1' if they are the same or '0' if they are not. Provide no other explanation."_**  

Using this approach, the model achieved **91% accuracy**, confirming_ that the generated queries are functionally equivalent to the reference queries.

