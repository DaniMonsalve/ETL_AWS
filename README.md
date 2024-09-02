This repository contains the code for an automated data pipeline that extracts property data from Zillow's API, transforms the data, and loads it into Amazon Redshift. The pipeline is fully orchestrated using Apache Airflow and leverages AWS services like S3, Lambda, and Redshift.

## Pipeline Overview

The flow of data through the pipeline can be summarized in the following steps:

1. **Extract Zillow Data**:
   - The pipeline starts by calling the Zillow API, retrieving property data in JSON format.
  
     <img width="942" alt="Graph Airflow" src="https://github.com/user-attachments/assets/0b7ff7fe-93d9-4855-8b65-e9540b635e4b">


2. **Load to S3 (Landing Zone)**:
   - The raw JSON data is stored in a designated S3 Landing Zone bucket for temporary staging.
  
     <img width="943" alt="tsk_extract_zillow_data_var" src="https://github.com/user-attachments/assets/0634dc3b-5162-4c58-afc6-78644fba4074">
     <img width="817" alt="etl-zillow-bucket" src="https://github.com/user-attachments/assets/9a98ba70-28c8-4862-8a62-2b22b1a23ff3">


3. **Lambda Function 1**:
   - Triggered automatically when the JSON data lands in the S3 bucket. Copies the data to another S3 bucket.
  
     <img width="940" alt="tsk_load_to_s3" src="https://github.com/user-attachments/assets/4b706beb-db96-497d-aa12-187e6d85a89a">
     <img width="823" alt="copy-raw-json-bucket-data" src="https://github.com/user-attachments/assets/b1872bf0-5ac0-4990-aa4e-524598520e0b">


4. **Lambda Function 2**:
   - Triggered after the copy process, transforms the JSON data into CSV format and stores it in a final S3 bucket.

     <img width="940" alt="tsk_is_file_in_s3_available" src="https://github.com/user-attachments/assets/f0f7024a-fc84-41f3-80bd-1f16560329c8">
     <img width="824" alt="clean-data-csv-bucket" src="https://github.com/user-attachments/assets/6dea66ee-a2c1-45cd-a70a-3531e455c632">


5. **Load into Amazon Redshift**:
   - The CSV file is loaded into Amazon Redshift for further querying and analysis.
  
     <img width="944" alt="tsk_transfer_s3_to_redshift" src="https://github.com/user-attachments/assets/5fac12ff-1878-4d17-9fb3-929211226a1e">
     <img width="959" alt="Table_Redshift" src="https://github.com/user-attachments/assets/f696e3bf-de40-4de3-8c77-dfe11611f534">


6. **Orchestration with Apache Airflow**:
   - All steps in this pipeline are orchestrated by Apache Airflow. Airflow sensors continuously monitor the S3 buckets and trigger appropriate tasks when data arrives. It ensures task dependencies are respected and manages retries and failures.

##  Configuration requirements:

    sudo apt update 
    sudo apt install python3-pip 
    sudo apt install python3.10-venv 
    python3 -m venv ETL_env 
    source ETL_env/bin/activate 
    pip install --upgrade awscli 
    sudo pip install apache-airflow 
    airflow standalone 
    pip install apache-airflow-providers-amazon 

## CONFIGURATION.md

Check CONFIGURATION.md file for more information on service configuration process.


  
