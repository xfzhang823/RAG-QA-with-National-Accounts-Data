# RAG-QA-with-National-Accounts-Data
Building a simple retrieval augmented generation model with 2008 SNA manual as retrieval knowledge base.

The steps:
1.	Parsed the Systems of National Accounts (SNA) 2008 manual from PDF to text file format: tried few pdf reader libraries recommended by Langchain; MyPDF2 was the best choice because it is easy to use and can handle the double column layout of the document.
2.	Cleaned the text quickly.
3.	Split the text into lists of sentences using a NLP library, spaCy, and added an ID variable (txt to json).
4.	Chunking: “stitched” 10 sentences + 2 sentences (buffer/overlap) to form each chunk/snippet; created new IDs for them.
5.	Instantiated pinecone vector database and created a new index (database)
6.	Embedded (vectorized) the chunked data using OpenAI’s embedding model (turning each snipped/chunked text into a 1,536-dimensional vector and upload ID, values (vector values), and text (snippet texts) into pinecone. 
7.	Embedded (vectorized) the query “In what situations do national accounts consolidate accounts?" with OpenAI’s embedding model (also a 1,536-dimensional vector.)
8.	Use this query vector to search in the pinecone database to find the top 3 vectors that are “closest” to the query vector and collect their corresponding texts (top 3 snippets that are most “similar” to the query.)
9.	Create new search prompt: use these texts as context (“Answer the question based on the context below…”); ask the question again (“In what situation, …”) 
10.	Use the “augmented” prompt to query via ChatGPT-3 API and return response.

The query:
query = "In what situations do national accounts consolidate accounts?"

This question is more complicated than it looks. National accounts are a system of accounts and balance sheets that provide a detailed and systematic framework for describing an economy's production activities. In laymen’s terms, it’s largely what national statistical agencies do: providing a financial report of an economy - GDP, consumption, investments, etc. 

“Consolidation” has a special meaning in national accounting. When you aggregate different parts of an economy, you have a choice to take out the overlaps between them - this is consolidation. For example, everyone knows GDP. GDP is a good example of “consolidated” metric. It adds up only the “value-added” portion of each industry. It is finished products only. Essentially, GDP is B2C spending only, which is only about 50%-60% of total economic activities. Personally, I think many research firms are over obsessed with GDP, even though their markets are driven by B2B spending. Because “consolidation” is very time-consuming and has political implications, to consolidate or not consolidate is not so straight forward.


LLM Responses (ChatGPT-3) without RAG: A bigger list but miss the essence of what consolidation actually mean
National accounts typically consolidate accounts in the following situations:
"
1. Government accounts: National accounts consolidate the accounts of different levels of government (federal, state, and local) to provide a comprehensive view of the country's public finances.
2. International trade: National accounts consolidate the accounts of imports and exports to measure a country's balance of trade and current account balance.
3. Financial sector: National accounts consolidate the accounts of financial institutions, such as banks and insurance companies, to measure the country's financial sector's contribution to the economy.
4. Public and private sectors: National accounts consolidate the accounts of both the public and private sectors to provide a complete picture of the country's economic activity.
5. National income: National accounts consolidate the accounts of different sectors of the economy (households, businesses, government) to measure the country's national income and gross domestic product (GDP).
6. International comparisons: National accounts consolidate accounts to facilitate international comparisons of economic performance and to assess a country's position in the global economy.
7. Time series analysis: National accounts consolidate accounts over time to track changes in the economy and identify trends and patterns.
8. Policy analysis: National accounts consolidate accounts to inform policy decisions and evaluate the effectiveness of economic policies.
9. Macroeconomic analysis: National accounts consolidate accounts to analyze the overall performance of the economy, including factors such as inflation, employment, and economic growth.
10. Reporting and transparency: National accounts consolidate accounts to provide a transparent and standardized view of the country's economic activity, which is essential for investors, policymakers, and other stakeholders.
"


LLM Responses (ChatGPT-3) with RAG: A shorter answer but captures more of the essence of “what consolidation” means in the national accounts’ context. 

"National accounts consolidate accounts in situations where it is useful for complementary presentations and analyses, such as for the government sector as a whole, to show the net relations between government and the rest of the economy."
