# Project Title: **Forecasting Stock Movements with Complaint Analysis**

## Group Information
- **Group Name:** BigData.rndm
- **Group Number:** 04

## Members
| Name                  | Email                    | Student ID   |
|-----------------------|--------------------------|--------------|
| Aditya Magarde        | amaga113@ucr.edu         | 862464946    |
| Siddhant Thakare      | sthak027@ucr.edu         | 862466315    |
| Sudhanshu Gulhane     | sgulh001@ucr.edu         | 862465762    |
| Rachit Prajapati      | rpraj002@ucr.edu         | 862466094    |

---

# **Division of Contributions**

## **Aditya Magarde**
1. **Hadoop and Hive Setup**
   - Configuration and deployment using Docker.
   - Setting up HDFS and Hive tables.
   - Data ingestion and schema creation.
2. **Row Store and Column Store Comparison**
   - Performance testing and analysis.
   - Evaluation of execution times and storage efficiencies.
3. **Query Time Evaluation with Different Datanodes**
   - Benchmarking performance using variable cluster sizes.

---

## **Siddhant Thakare**
1. **Frontend and Backend Development**
   - Development of the ReactJS frontend for visualization.
   - Backend API design with FastAPI.
   - Integration of machine learning models with the backend.
2. **Machine Learning Model**
   - Feature engineering.
   - Gradient Boosting implementation and optimization.
   - Evaluation metrics: Accuracy, Precision, Recall.

---

## **Sudhanshu Gulhane**
1. **Parquet File Conversion and Setup**
   - Data cleaning and transformation.
   - Implementation of columnar storage using Parquet.
   - Creating column-based Hive tables for query optimization.
2. **Logical Query and API Response Time Optimization**
   - Improving query structures with Common Table Expressions.
   - Enhancing API response times through backend optimizations.

---

## **Rachit Prajapati**
1. **Testing**
   - End-to-end system testing to ensure smooth operation.
2. **Usability Testing**
   - Ensuring the frontend is user-friendly and interactive.
   - API testing across multiple companies for response consistency.
3. **Documentation**
   - Quick guide for Hive and HDFS setup.
   - Finalizing the project report and presentation materials.
4. **Integration**
   - Integration of Dockerized Hive, HDFS, FastAPI, and React.

# How to Run the Project

## Prerequisites
1. Ensure you have the following installed:
   - Python
   - Docker
   - Node.js (You can alternatively use bun, pnpm, etc.)
<br>

## Step 1: Start the Hive Server
### Step 1.1: Start Hive Server
Go to `docker-hive` directory, and run `docker-compose up -d` to run the containers

### Step 1.2: Get HDFS ready
Run to connect to the namenode
```bash
docker exec -it <namenode_container_id> /bin/bash
```

then run inside the name-node container
```bash
hdfs dfs -mkdir -p /user/hive/warehouse
hdfs dfs -chmod -R 777 /user/hive/warehouse
```

### Step 1.3: Transfer your data (.csv/.ssv/.tsv) into HDFS
Run to copy data from your local to your `hive-server` container
```bash
docker cp /path/to/yourfile.csv <hive-server_container_id>:/tmp/yourfile.csv
```

### Step 1.4: Login to hive-container and put the copied data into HDFS
Run in a new terminal
```bash
docker exec -it <hive-server_container_id> /bin/bash
```

Put the data in HDFS
```bash
hdfs dfs -mkdir -p /user/hive/warehouse/your_table
hdfs dfs -put /tmp/yourfile.csv /user/hive/warehouse/your_table/
```

### Step 1.4: Create the table in Hive
First connect to the hive interface
```bash
$ hive
```

Create the table by running the SQL query. Make sure to edit the query as per your need.
```sql
CREATE EXTERNAL TABLE your_table (
    column_1 DATE,
    column_2 FLOAT,
    column_3 FLOAT,
    column_4 FLOAT,
    column_5 FLOAT
)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES (
    "serialization.format" = ",",
    "field.delim" = ","
)
STORED AS TEXTFILE
LOCATION '/user/hive/warehouse/your_table/'
TBLPROPERTIES ("skip.header.line.count"="1");
```

### Step 1.5: Connect to the hive server from backend server
After doing the step 1.4 your table and data set up. You can connect to the hive server after this. You can use the following python snippet for reference.

```python
from pyhive import hive

conn = hive.Connection(
    host='localhost', 
    port=10000, 
    username='your_user', 
    database='default'
)

cursor = conn.cursor()
cursor.execute('SELECT * FROM discover_findata LIMIT 10')
results = cursor.fetchall()

for row in results:
    print(row)
```

## Step 2: Start the Backend Server
Go to the `stock-movement-forecaster`

Make sure that the dependencies are installed
```bash
pip install fastapi uvicorn cachetools pyhive scikit-learn joblib
```

Run the server
```bash
uvicorn main:app --reload
```

## Step 3: Start the Frontend Server
Go to the `stock-movement-forecaster-app`

Make sure that the dependencies are installed
```bash
npm install
```

Run the web app
```bash
npm run dev
```
