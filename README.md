
# DeSQL: Interactive Debugging of SQL in Data-Intensive Scalable Computing

## 1. Pre-requisites

Ensure that `docker` is installed and functioning on your system. If it's not installed, please download Docker Desktop from [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/).

These instructions have been verified with:
- **MacOS:** ProductVersion: 11.2.3, BuildVersion: 20D91
- **Docker:** 20.10.22, build 3a2c30b

Additionally, you'll need:
- Python3
- Ports 4040 and 8080 available for Spark UI and Spark Master UI, respectively.

## 2. Creating the Docker Image

### Clone the Repository

```sh
git clone https://github.com/sabaat/desql-artifacts.git
```

### Clean Up

Remove any system-generated files to prevent interference with Docker builds:

```sh
find . -name '.DS_Store' -type f -delete
```

### Build the Docker Image

Navigate to the `spark-sql-debug` directory and build the Docker image:

```sh
cd spark-sql-debug
docker build -t my-custom-spark:latest -f dist/kubernetes/dockerfiles/spark/Dockerfile .
```

> **NOTE:** If you encounter permission issues, use `sudo` with Docker commands.

### Start Containers

Use Docker Compose to start the containers:

```sh
docker compose up -d
```

> **TROUBLESHOOTING:** If Docker Compose fails, restart Docker:

```sh
sudo systemctl restart docker
```

## 3. Running DeSQL

### Submit a Spark SQL Query

Submit a Spark SQL job to the DeSQL container. Replace `query5.sql` with any other query file as needed:

```sh
docker exec -it spark-local-container /opt/spark/bin/spark-submit \
--class DeSqlPackage.SQLTest.SQLTest \
--master "local[*]" \
--conf "spark.some.config.option=some-value" \
/opt/spark/app/desqlpackage_2.12-0.1.0-SNAPSHOT.jar /opt/spark/queries/query5.sql
```

> **Expected Observation:** DeSQL will start, and you can observe the logs in the console. Once DeSQL starts, access the DeSQL UI at `http://localhost:4040/debugger/`. The UI displays sub-queries of the original query along with their data as processed within Spark computations. Additionally, it presents the query execution plan, with clickable green nodes for nodes with available sub-queries, to view the node's respective subquery and data.
```

