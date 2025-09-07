#  🚀 Run PySpark in Docker with JupyterLab

This guide shows you how to launch a **PySpark-enabled JupyterLab** environment inside Docker, run Spark jobs on your datasets, and persist notebooks without installing Spark locally.


## 📦 Prerequisites


- [Docker Desktop](https://www.docker.com/products/docker-desktop) installed and running
- A dataset file (e.g., `order_items.csv`)


## 📂 Setup Project Folder

- Create a folder structure for your project:

```bash
mkdir pyspark-lab
cd pyspark-lab
mkdir data notebooks
```

## ▶️ Start PySpark with Docker

- On macOS/Linux

```bash
docker run --name sparklab -it --rm \
  -p 8888:8888 -p 4040:4040 \
  -v "$PWD/data:/home/jovyan/data" \
  -v "$PWD/notebooks:/home/jovyan/work" \
  jupyter/pyspark-notebook:latest
```
- On Windows (PowerShell)

```bash
docker run --name sparklab -it --rm `
  -p 8888:8888 -p 4040:4040 `
  -v ${PWD}/data:/home/jovyan/data `
  -v ${PWD}/notebooks:/home/jovyan/work `
  jupyter/pyspark-notebook:latest

```
- On Windows (CMD)

```bash

docker run --name sparklab -it --rm ^
  -p 8888:8888 -p 4040:4040 ^
  -v %cd%\data:/home/jovyan/data ^
  -v %cd%\notebooks:/home/jovyan/work ^
  jupyter/pyspark-notebook:latest

```
## 🌐 Access JupyterLab
- After starting, the terminal will print a URL like:

```bash

http://127.0.0.1:8888/lab?token=abcdef123...

```
- Open it in your browser
- Your files will appear under:
     - /home/jovyan/data → mapped to data/ on host
     - /home/jovyan/work → mapped to notebooks/ on host



- The Spark UI will be available at http://localhost:4040

## ✅ Verify Spark is Running
```bash
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()
print("Spark Version:", spark.version)

```

## 📊 Load Dataset Example
```bash
products = spark.read.parquet(f"{BRONZE_PATH}/products")
    categories = spark.read.parquet(f"{BRONZE_PATH}/categories")
    inventory = spark.read.parquet(f"{BRONZE_PATH}/inventory")
    reviews = spark.read.parquet(f"{BRONZE_PATH}/reviews")
    orders = spark.read.parquet(f"{BRONZE_PATH}/orders")
    order_items = spark.read.parquet(f"{BRONZE_PATH}/order_items")
    suppliers = spark.read.parquet(f"{BRONZE_PATH}/suppliers")
    product_suppliers = spark.read.parquet(f"{BRONZE_PATH}/product_suppliers")
```
## ⚡ Optional: Docker Compose
- Instead of typing the long command, create a docker-compose.yml:
- Run
```bash
docker compose up
```

- Stop
```bash
docker compose down
```