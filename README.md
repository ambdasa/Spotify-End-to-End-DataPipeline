# Spotify-End-to-End-DataPipeline

🎧 Spotify ETL Pipeline on AWS

This project demonstrates an automated ETL (Extract, Transform, Load) pipeline using the Spotify API and AWS services. The pipeline extracts music data from Spotify, processes it, and loads it into Amazon S3, making it queryable via Amazon Athena.

---

## 🗺️ Architecture Overview
flowchart LR
    A[Spotify API] --> B[Python Script]
    B --> C[CloudWatch]
    C --> D[Lambda (Extract)]
    D --> E[S3 (Raw Data)]
    E --> F[Lambda (Transform)]
    F --> G[S3 (Transformed Data)]
    G --> H[Glue Crawler]
    H --> I[Glue Data Catalog]
    I --> J[Athena]

🧩 Components

### 🔹 Extract

* **Spotify API**: Source of music data such as tracks, artists, and albums.
* **Python Script**: Contains logic to authenticate and pull data from Spotify API.
* **Amazon CloudWatch**: Triggers extraction Lambda function on a daily schedule.
* **AWS Lambda (Extraction)**: Executes Python script and writes raw JSON data to Amazon S3.
  
![image](https://github.com/user-attachments/assets/a45755fc-c062-4665-a059-3b46c6bb9e51)

![image](https://github.com/user-attachments/assets/d6db1830-eb55-4b21-9e0b-cf239882e75f)



### 🔹 Transform

* **AWS Lambda (Transformation)**: Processes raw JSON into a structured format (e.g., CSV or Parquet).
* **Amazon S3 (Transformed Data)**: Stores the cleaned and structured data for querying.

![image](https://github.com/user-attachments/assets/9387f90d-54cb-4083-8c89-636ae21f402c)

![image](https://github.com/user-attachments/assets/2a43a2ec-a2b3-49cf-b14d-89c8556384bf)



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
     
![image](https://github.com/user-attachments/assets/3b39f53c-f300-43af-abf5-246c88e8eb61)
     
![image](https://github.com/user-attachments/assets/6e94a178-7c03-4294-a12e-973907bc4d96)

3. **Set Up S3 Buckets**

   * Create two buckets (or prefixes): one for raw data and one for transformed data.

     ![image](https://github.com/user-attachments/assets/b38e0b58-1dd4-4711-86cf-acbaf45857a4)


4. **Configure Glue**

   * Set up a Glue Crawler targeting the transformed data location.
   * Create or update a Glue Data Catalog table.

     ![image](https://github.com/user-attachments/assets/4c306704-8d7c-4c65-839e-25e921aac119)


5. **Run Athena Queries**

   * Connect to the Glue Catalog from Athena.
   * Use SQL to analyze your Spotify data.
  
     ![image](https://github.com/user-attachments/assets/4d399530-1912-4cde-83a1-0d5b16fb0fcf)

     ![image](https://github.com/user-attachments/assets/d5cd467e-da15-4c95-845f-f5d60c807a91)


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
