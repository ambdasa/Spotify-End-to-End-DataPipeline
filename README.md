# Spotify-End-to-End-DataPipeline

🎧 Spotify ETL Pipeline on AWS

This project demonstrates an automated ETL (Extract, Transform, Load) pipeline using the Spotify API and AWS services. The pipeline extracts music data from Spotify, processes it, and loads it into Amazon S3, making it queryable via Amazon Athena.

---

## 🗺️ Architecture Overview
Spotify API → Python Script → CloudWatch → Lambda (Extract) → S3 (Raw Data)
                                             ↓
                                   Lambda (Transform) → S3 (Transformed Data)
                                                             ↓
                                                       Glue Crawler
                                                             ↓
                                                  Glue Data Catalog → Athena
🧩 Components

### 🔹 Extract

* **Spotify API**: Source of music data such as tracks, artists, and albums.
* **Python Script**: Contains logic to authenticate and pull data from Spotify API.
* **Amazon CloudWatch**: Triggers extraction Lambda function on a daily schedule.
* **AWS Lambda (Extraction)**: Executes Python script and writes raw JSON data to Amazon S3.

### 🔹 Transform

* **AWS Lambda (Transformation)**: Processes raw JSON into a structured format (e.g., CSV or Parquet).
* **Amazon S3 (Transformed Data)**: Stores the cleaned and structured data for querying.

### 🔹 Load

* **Amazon S3 Trigger**: Detects new objects in the transformed data bucket.
* **AWS Glue Crawler**: Scans the transformed data to infer schema.
* **AWS Glue Data Catalog**: Stores metadata about the dataset.
* **Amazon Athena**: Enables SQL querying over the transformed data stored in S3.

---

## 🔄 Workflow Steps

1. **Trigger**: Amazon CloudWatch runs the extract Lambda function daily.
2. **Extract**:

   * Lambda calls the Spotify API.
   * Fetches and stores raw data in S3.
3. **Transform**:

   * Raw data in S3 triggers a transformation Lambda function.
   * Data is cleaned and formatted, then saved to a separate S3 location.
4. **Load**:

   * A new object in the transformed S3 bucket triggers a Glue Crawler.
   * The crawler updates the Glue Data Catalog with the schema.
   * Athena can now query this data using SQL.

---

## 📁 Project Structure

spotify-etl-pipeline/
├── lambda/
│   ├── extract_lambda.py        # Extracts data from Spotify API
│   └── transform_lambda.py      # Transforms raw data
├── utils/
│   └── spotify_client.py        # Handles Spotify authentication and requests
├── s3/
│   ├── raw/                     # Stores raw JSON data
│   └── transformed/            # Stores structured data (CSV, Parquet)
├── glue/
│   └── crawler_config.json      # Configuration for AWS Glue Crawler
├── athena/
│   └── queries.sql              # Sample SQL queries for analytics
└── README.md                    # Project documentation

## 🚀 Deployment Instructions

1. **Spotify API Setup**

   * Create a Spotify Developer account.
   * Generate a client_id and client_secret.
   * Store securely (e.g., AWS Secrets Manager or Lambda environment variables).

2. **Deploy Lambda Functions**

   * Package and upload extract_lambda.py and transform_lambda.py.
   * Add necessary IAM permissions.
   * Configure triggers (CloudWatch for extract, S3 object create for transform).

3. **Set Up S3 Buckets**

   * Create two buckets (or prefixes): one for raw data and one for transformed data.

4. **Configure Glue**

   * Set up a Glue Crawler targeting the transformed data location.
   * Create or update a Glue Data Catalog table.

5. **Run Athena Queries**

   * Connect to the Glue Catalog from Athena.
   * Use SQL to analyze your Spotify data.

---

## 📊 Example Use Cases

* Analyze most played artists over time.
* Discover genre trends from your playlists.
* Track popularity changes of tracks.

---

## 📎 Requirements

* AWS Account
* Spotify Developer Account
* Python 3.9+
* AWS CLI configured

---

## 📚 References

* [Spotify for Developers](https://developer.spotify.com/)
* [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/)
* [AWS Glue Documentation](https://docs.aws.amazon.com/glue/)
* [Amazon Athena Documentation](https://docs.aws.amazon.com/athena/)

Happy Coding!
