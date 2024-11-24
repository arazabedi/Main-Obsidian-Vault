Missed half the lecture - go through the slides.

Data Lakes

Object Storage - Stores data in bucketsAWS S3/Googe Cloud Storage

**Classes of Cloud Storage**

![[Pasted image 20240925115014.png]]

Nearline equivalent in AWS is Infrequent Access. Storage is cheaper than standard but there is a charge for access.

Coldline storage - remember it's 90 days.

**Databases**
Cloud SQL - DaaS, can be PostgreSQL, MySQL etc... Does backups and security patches automatically. Fully manages database service like AWS RDS. Google also do Oracle but require a dedicated physical machine for it. 

Cloud Spanner - relational database that scales horizontally to handle unexpected business spikes. Fully managed, Google uses this. PostgreSQL and MySQL. Similar to Aurora.

BigQuery - huge storage (petabytes)

Cloud Bigtable - NoSQL big data, large networks at low latency.

Firestore - key/value pairings
