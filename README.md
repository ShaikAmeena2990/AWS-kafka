# Kafka-S3 Data Pipeline with AWS Glue and Athena
This project sets up a real-time streaming data pipeline using Apache Kafka on AWS EC2, stores data in Amazon S3, and analyzes it using AWS Glue Crawler and Athena.

# Project Overview
Kafka on EC2: Kafka cluster runs on EC2 instances for real-time data streaming.

Data Ingestion: Data produced to Kafka topics is consumed and stored in S3.

S3 Storage: Data is stored in partitioned format (e.g., JSON, Parquet, etc.).

Glue Crawler: Scans S3 to create/update Glue Data Catalog tables.

Athena: SQL-based analysis of streaming data stored in S3.

# Process
1. Real-time Data Streaming with Kafka
Kafka broker is deployed on an EC2 instance.

Producers send streaming data (e.g., JSON events) to Kafka topics.

Kafka ensures high-throughput, fault-tolerant data ingestion.

2. Data Persistence with S3
A Kafka Consumer (written in Python) listens to Kafka topics.

The consumer processes and uploads data to S3 in real-time.

Data is organized in partitioned folders by year/month/day.

3. Schema Discovery with AWS Glue Crawler
Glue Crawler automatically scans S3 data and infers schema.

It populates the AWS Glue Data Catalog for easy data discovery.

Enables schema-on-read, allowing seamless access to data without predefining schema.

4. Serverless Querying via Athena
Athena queries the Glue Data Catalog tables using standard SQL.

Supports ad-hoc analysis, BI dashboards, and real-time insights.

Athena reads directly from S3, eliminating the need for ETL.

5. Cloud-Native & Scalable
Leverages AWS services for scalability (S3, Glue, Athena).

Kafka provides decoupled ingestion; S3 offers cost-effective storage.

The pipeline can scale to handle large volumes of streaming data.

# Prerequisites
AWS EC2 with Kafka installed

S3 Bucket (e.g., s3://my-kafka-data-bucket)

AWS CLI configured (aws configure)

IAM Role with S3, Glue, and Athena permissions

# Setup Instructions
# Kafka on EC2
Install Kafka
sudo apt install default-jdk -y
wget https://downloads.apache.org/kafka/3.7.0/kafka_2.13-3.7.0.tgz
tar -xzf kafka_2.13-3.7.0.tgz
cd kafka_2.13-3.7.0

# Start Zookeeper and Kafka Broker
bin/zookeeper-server-start.sh config/zookeeper.properties

# Start Kafka Broker (new terminal)
bin/kafka-server-start.sh config/server.properties

# Create Kafka Topic
bin/kafka-topics.sh --create --topic my-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
# Produce Data
bin/kafka-console-producer.sh --topic my-topic --bootstrap-server localhost:9092
# Consume Data
bin/kafka-console-consumer.sh --topic my-topic --bootstrap-server localhost:9092 --from-beginning
