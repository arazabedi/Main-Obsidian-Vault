AWS SageMaker vs Google VertexAI

AWS is a better pure cloud provider, but Google is better for data/AI/ML.

LLMs are glorified dictionaries with algorithms that predict the next word.

AI contains all of machine learning, which contains deep learning, which has been used it generative AI purposes.

Data Quality:

![[Pasted image 20240926134300.png]]

Validity - when data has predefined standards and definitions (e.g. YYYY/mm/dd vs dd/mm/YYYY). The data contravenes the rules. Think validation in web dev. It's about the format, not content.

Accuracy - Wrong content. Reflects the correctness of the data. 

![[Pasted image 20240926134703.png]]

Data drift - model has been trained, but the data no longer reflects the new conditions.

Google Cloud provides a number of services for AI:

![[Pasted image 20240926135312.png]]

Pre-trained has the lowest amount of effort. It's quick and you don't need to do any training. No ML knowledge or data needed for you. But - zero flexibility if you're running a specialised business.

Auto-ML - You have data but you don't know/want to code. SaaS.

In custom training create an instance of a workbench and create a Python Jupyter notebook.

Alex will show an entire pipeline.

SQL - transactional (something that happens a lot - milliseconds - data keeps coming in).

In Auto-ML you can use a Cloud SQL instance to train a model.

Cloud SQL is for recording transactions. People continuously add data through APIs.

BIgQuery is a data warehouse. Cloud SQL is a database.

BQ provides analytics - it's for looking at the past - historical data. Not streamed data. All data warehouses are about historical data. 

If you want to analyse historical data, you use a data warehouse like BigQuery/AWS RedShift.

Data warehouses are built on top of databases.

In BigQuery just add data from local files or cloud sql.

BigQuery can just handle the results from Google Cloud SQL. 

Click on the three dots and create a dataset.
Then create a table in SQL.
Then upload the data to SQL.

Because you've connected Cloud SQL to BigQuery, you get a service account (and a service account id). That account need the right permissions to access Cloud SQL. You have to do this manually.

If you go to IAM management, you see your accounts. BUT. You can't see your connection service account because it's hidden. BUT. You can go into GRANT ACCESS and copy the id and it'll find it. Then you can assign it the role of Cloud SQL client (connectivity access to Cloud SQL instances).

GIVE THE SERVICE ACCOUNT CONNECTIVITY ACCESS.




