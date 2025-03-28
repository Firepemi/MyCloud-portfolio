# MyCloud-portfolio
Design and Implementation of a Data Analytic Platform (DAP) on AWS for the City of Vancouver

📝Project Description
This project showcases the steps in migrating the City of Vancouver into AWS and developing a Data Analytic Platform (DAP) hosted on Amazon Web Services (AWS) to support the City of Vancouver’s transition to a cloud-based analytics infrastructure. The dataset analyzed includes business licenses issued by the city of Vancouver. We could integrate the DAP process developed using Drawio and various tools made available by AWS. From data ingestion to cleaning and profiling, cataloging, summarization, and ETL pipeline creation to further analyze the dataset. 

The implementation involves:

✅ Data ingestion using Amazon S3

✅ Data profiling and cleaning with AWS Glue DataBrew

✅ Metadata organization via AWS Glue Data Catalog

✅ Data summarization and transformation with AWS Glue Visual ETL

✅ Each step includes configuration, screenshots, and justification for choices, aiming for scalability, accuracy, and performance.
 Project Objective
To design and implement a scalable, secure, cost-effective cloud-based Data Analytics Platform on AWS for the City of Vancouver. This platform will enable descriptive analytics on employee compensation data by automating the processes of data ingestion, cleaning, cataloging, and summarization using AWS-native services, thereby supporting data-driven decision-making.
🗂️ Dataset Description
Source: Vancouver Open Data Portal The dataset used in this project is titled “Employee Remuneration and Expenses (Earning Over $75,000)”, sourced from the City of Vancouver Open Data Portal.

This dataset contains annual business license records for the City of Vancouver, detailing information on various businesses operating within the city. It includes key attributes such as business names, types, license status, issue and expiry dates, employee count, and license fees paid. The dataset supports transparency in municipal operations and enables data-driven insights into economic activity, regulatory compliance, and neighborhood-level business trends. It is particularly useful for analyzing business density, licensing patterns, and sectoral distribution across Vancouver’s local areas.
A brief summary of the Business License dataset:

Total Records: 506

Total Columns: 25

Purpose: Contains business license data for the City of Vancouver.

Key Columns:
BusinessName, BusinessTradeName: Names of businesses.

BusinessType, BusinessSubType: Type/category of business activity.

Status: License status (e.g., Issued, Pending).

IssuedDate, ExpiredDate: License validity period.

Number of Employees: Number of employees per business.

FeePaid: Amount paid for the license.

City, Province, PostalCode, LocalArea: Location details.

Geom, geo_point_2d: Geolocation data (partially missing).
⚙️ METHODOLOGY
The methodology for this project follows a structured Extract-Transform-Load (ETL) approach using AWS-native services. The aim was to build a robust and scalable Data Analytic Platform (DAP) that supports descriptive analytics for the City of Vancouver’s Business license dataset.
🔽 1. DATA INGESTION
Tool Used: Amazon S3

The Amazon s3 bucket, the simple storage service, is a scalable, secure, and high-performance service offered by Amazon web services that store and retrieve any type/amount of data from any web or server into the Amazon cloud. This is the tool used for data ingestion into the s3 bucket. I collated my dataset (Business License) from the city of Vancouver data portal and imported it into Excel to properly format it to ensure data accuracy in the s3 bucket. Afterward, I created a raw bucket for the city of Vancouver (COV) using Amazon s3 and uploaded my dataset. I have attached a screenshot of data successfully uploaded to the COV raw bucket in s3. 

The raw dataset (CSV format) was sourced from the City of Vancouver Open Data Portal and uploaded into an Amazon S3 bucket named "cov-raw-firepemi."
Data ingestion is the foundational step in developing the Data Analytic Platform (DAP) for the City of Vancouver. It involves importing raw data into a cloud environment for processing and analysis.
![S3 raw](https://github.com/user-attachments/assets/0e25c08c-c86b-4966-a650-c3fddb5550ad)
The above screenshot shows the dataset business license successfully ingested into the Amazon s3 bucket folder cov-raw-firepemi with url s3://cov-raw-firepemi/business liscense.csv. This step is necessary to allow AWS access to the dataset for further analysis. After uploading my dataset into my s3 bucket on Amazon, the next step was to create another storage folder in the s3 bucket called transfer. 

2. 🔍 DATA PROFILING
   
Tool used: AWS Glue DataBrew

AWS Glue DataBrew is a tool that Amazon web services provide to help users visually profile and clean their data set. This AWS data brew transforms raw datasets into structured data by identifying data quality issues, providing solutions, and making it suitable for probably analytics uses. In this step, using the Amazon Glue DataBrew, I created a project named "cov-busi-license-prj-firepemi" to profile the dataset. See the implementation of the job created below
![GDB ](https://github.com/user-attachments/assets/b01a325c-f632-421a-a7a5-67f92751798a) 
The above screenshot displays the data profiling results done using the AWS glue databrew. Identifying 71% valid cells and 100% valid rows. The main use of these steps is to further structure the business license dataset while identifying data quality issues in this dataset.

🧹 3. DATA CLEANING

Tool Used: AWS Glue DataBrew

To ensure the dataset was accurate, consistent, and ready for analysis, data cleaning was performed using AWS Glue DataBrew. After profiling the Employee Remuneration and Expenses dataset to understand its structure and quality, a reusable DataBrew recipe was created to automate the cleaning process. This involved standardizing column formats, resolving data type inconsistencies (such as converting numeric fields to appropriate formats), and removing duplicates and unnecessary entries. The cleaned data was then output to a transformed S3 bucket. This step was critical in the data pipeline to support reliable downstream processing and meaningful analysis.

🛠️ Cleaning Process Overview
Once the dataset "Business license.csv" was successfully imported into DataBrew, a custom cleaning recipe was developed to apply a series of transformation steps. These steps were recorded under the "cov-busi-license-prj-firepemi," allowing for reusability and version control across future datasets. See below the cleaning project successfully created.
![project](https://github.com/user-attachments/assets/59050c00-d9b3-4746-9639-4a93478285cf)
<img width="1470" alt="Clean recipe" src="https://github.com/user-attachments/assets/2239a0f7-a69e-4291-8886-d9da333cea14" />
The screenshots added to show a recipe job created with the name "cov-busi-license-prj-firepemi" and a clean job with the name "cov-busi-license-cln-firepemi"

**Key transformations and adjustments included:**

🔢 1. Data Type Standardization
To ensure that my business license dataset columns are uniform, I renamed folder year in the dataset to file year and geo_point_2d to GeoPoint2d.

The conversions help prevent errors or misinterpretations in arithmetic operations, especially during aggregations (e.g., sum, average) and visualization rendering.

✅ 2. Validation of Data Integrity
After data type conversion, each field was thoroughly validated to ensure values fell within expected ranges. The validation confirmed the dataset’s high integrity, with no missing values, nulls, or NaNs detected. Additionally, no duplicate records were found, eliminating the need for further deduplication.

🧪 3. Schema Conformity Check
The schema was aligned to match what would be defined in the AWS Glue Data Catalog later.

📦 4. Execution and Storage
A DataBrew job named "cov-busi-liscence-cln-firepemi" was executed to apply the cleaning recipe to the dataset.

Upon successful execution, the cleaned dataset was saved to a designated transformation S3 bucket (city-van-data-trf-emma) for further processing.

The job log and outputs were reviewed to ensure a successful transformation with no job errors.

🔁 5. Reusability and Automation
The cleaning recipe was designed to be modular and reusable, allowing it to be automatically applied to similar datasets in the future. This approach streamlines the cleaning process, minimizes manual intervention, and reduces the risk of errors. The same recipe can be adapted with minimal adjustments for business license datasets from other years or departments.

This data-cleaning step ensures the dataset meets essential standards for quality and consistency, forming a reliable foundation for accurate cataloging, querying, visualization, and reporting. It also guarantees that downstream processes—such as data summarization and dashboard integration—are built on trustworthy, well-prepared data.

🧹 3. DATA CATALOGING
Tool Used: AWS Glue 

Data Catalog

AWS Glue is a serverless integration tool that makes it easier to discover, prepare, move, and integrate data for analytics. This step aims to extract data using a crawler in the AWS glue platform to move the dataset into tables by using ETL to create a pipeline for data summarization. We use s3 bucket and AWS glue in this step. S3 to create a new bucket called "cov-crw-firepemi," and in the glue platform, we created a database for this catalog.

🧱 Steps in the Data Cataloging Process
📁 1. Creating a Glue Database
A new AWS Glue database named "cov-data-catalog-firepemi" was created to initiate the cataloging process. This database is a logical container for storing metadata associated with one or more tables. Establishing a dedicated database promotes organized data management, supports modular scalability, and enables clear separation of datasets.

📌 This setup allows datasets to be logically grouped by project, data source, or year.
📌 Creating the database is a foundational step for managing table structures and schema definitions within the Glue Data Catalog.

# 🔍 2. Registering the Cleaned Dataset Once the database was created, the cleaned dataset—stored in the transformation S3 bucket "cov-trf-firepemi" was registered as a table.

The table schema includes details such as:

Column names (e.g., folder year, business type, business trade name, unit, issued date, expired date, number of employees)

Data types (e.g., String, Integer)

Data location (S3 path)

This registration makes the dataset queryable via SQL-like interfaces in tools like AWS Athena, ensuring schema consistency across tools.

# 🔄 3. Creating and Configuring a Crawler To automate the metadata extraction process, a Glue Crawler was created with the identifier "cov-crw-firepemi"

The crawler was configured to:

Scan the S3 path where the cleaned dataset is stored

Detect the schema automatically (column types, formats, partitions)

Populate or update the metadata in the Glue Data Catalog

This automation removes the need for manual schema definition and supports future data changes (e.g., schema evolution or new file uploads).

After running the crawler, a metadata table was created and successfully linked to the transformation S3 bucket.

# 🧠 4. Why Data Cataloging Matters

This step transforms the cleaned dataset from simple storage to a structured, accessible resource:

Enables schema validation and facilitates data discovery across AWS services.

Ensures consistent data definitions for use with querying and reporting tools.

Serves as a bridge between raw data in S3 and analytical services like Athena (for SQL-based queries) and QuickSight (for interactive dashboards).

IAM roles and Glue policies support features like partitioning, versioning, and fine-grained access control.

By cataloging the dataset, it becomes not only stored but also well-organized and readily accessible—empowering technical and non-technical users to easily explore and analyze the data.
![Database](https://github.com/user-attachments/assets/2750cde1-8191-4657-992f-8d62a8eb2942)
<img width="1470" alt="Screenshot 2025-03-27 at 9 25 05 PM" src="https://github.com/user-attachments/assets/92629199-110c-4891-8335-c20d913e8cb9" />
![Tables](https://github.com/user-attachments/assets/c8f9122d-aa0e-4145-b9c6-7d32285159f2)

📊 5. DATA SUMMARIZATION

Tool Used: AWS Glue Visual ETL

This step aims to summarize the dataset using metrics such as average, maximum, count, minimum, and so on to further analyze our data to better gain insights into the data information to help make data-informed decisions as a data team. The main AWS tools used in this step include AWS glue and s3 bucket. For this step, an ETL job was created to visually analyze the data to achieve our desired result. AWS Glue Visual ETL (Extract, Transform, Load) was employed to perform these summarization tasks through a user-friendly interface without writing code. The outputs were stored in a curated S3 bucket named "cov-cur-firepemi," representing the final, analysis-ready dataset. 

# 🧱 Process Overview
•	Extract business: The prepared data was extracted from the database ‘cov-data-catalog-firepemi and table ‘cov-busi-license-trf-system’

•	Change Schema: This process was used to delete unpreferred columns like business type and unit, as they were repetitive information.

•	Filter business license: This was used to filter rows showing companies with employees greater than 15 and business status matching issues.

•	Summarize business license: The data was grouped using the issued date to categorize the maximum expired date and the average fee paid for the business license.

•	Add date to the summarized business license: The report date was added to the summarized business license data from the previous step.

•	Convert to LTZ: The data added to the summarized business license was converted to LTZ (Vancouver local time zone)

•	Prepare to Load: The local time zone version was maintained in this step, the schema was reviewed, and the titles were adjusted to ensure consistency.

•	Load System and Load Users: The steps were done to ensure the file is stored in an easily

•	accessible manner to users and systems, with various outputs like CSV and parquet. As well as partitioning to group datasets.
The summarized datasets were extracted in two formats.

Parquet (with Snappy Compression): Used for internal system processing. Offers better performance for large-scale queries. Partitioned by Year for optimized querying in tools like Athena and Redshift Spectrum

CSV (Uncompressed): Ideal for human consumption or simple external use. Easily downloadable and readable in Excel or basic data tools. Both output formats were stored in the curated S3 bucket (city-van-data-cur-emma), completing the data transformation pipeline.
Sure! Here's a polished version of that section:

### ✅ Benefits of Summarization  
- **Reduces complexity** by condensing detailed records into high-level insights, making analysis more straightforward.  
- **Improves performance** by enabling faster queries and reporting through data compression and partitioning.  
- **Enhances visualization readiness** by preparing the data for tools like Amazon QuickSight or other external dashboards.

<img width="1470" alt="Screenshot 2025-03-27 at 9 36 26 PM" src="https://github.com/user-attachments/assets/f2f6fb46-0bc1-4756-a98a-23adedb61aa0" />

<img width="443" alt="image" src="https://github.com/user-attachments/assets/320dfd60-ca98-4cf1-8130-4d1c0f9f5205" />

<img width="467" alt="image" src="https://github.com/user-attachments/assets/f35a8e24-bfcc-4827-9f68-e7a19f835b6f" />

The  screenshots show the complete Visual ETL pipeline and the result of summarized datasets optimized for both system integration and user-facing analysis.

The successful design and deployment of the Data Analytics Platform (DAP) on AWS for the City of Vancouver showcases the power of cloud-native tools in delivering scalable, secure, and cost-effective analytics solutions. By leveraging services such as Amazon S3, AWS Glue DataBrew, AWS Glue Data Catalog, and AWS Glue Visual ETL, the project implemented a robust and structured ETL pipeline that transforms raw municipal data into meaningful insights.

Each phase—from data ingestion to summarization—was carefully executed to ensure data integrity, performance efficiency, and readiness for analysis. The platform enables descriptive analytics on employee remuneration and expense data while laying a strong foundation for future capabilities such as real-time analytics, advanced dashboards, and predictive modeling.

Ultimately, this project underscores how public sector organizations can harness cloud technologies to enhance data accessibility, support informed decision-making, and promote transparency—paving the way for a more data-driven and responsive government.

6. DATA ANALYSIS

Data analytics is evaluating, organizing, and interpreting raw datasets to draw conclusions that aid in a better-informed decision-making process. The significant steps involved in data analytics are collecting, cleaning, analyzing, and interpreting results. We have previously collected data from the business license extracted from the City of Vancouver data portal in CSV format, which we have also cleaned using the AWS Glue. We want to analyze the data set and possibly interpret its results. 
We are doing descriptive analytics for the data analytics, which explores what happened. Because it summarizes past data to understand what happened in the process of business license issuing in Vancouver by the City. The data being explored is historical data, and we will explain what happened based on the data set.
<img width="1470" alt="Business_Q1" src="https://github.com/user-attachments/assets/33f7a7c7-5577-4009-a410-1e0399f1d75e" />
<img width="1470" alt="Business_Q2" src="https://github.com/user-attachments/assets/0dfebc5c-f9ec-4f71-aab2-120385656840" />
<img width="1470" alt="Business_Q3" src="https://github.com/user-attachments/assets/5ed0b072-41b2-41b2-8a2f-99775868aaef" />
<img width="1470" alt="Business_Q3 part 2" src="https://github.com/user-attachments/assets/53deefb2-b916-47ee-b14a-73994ec7fbaa" />

The attached screenshots show the response to three business questions: 
-What is the average fee paid for the business license issued?
-what is the minimum average fee paid?
-What is the average number of employees for various businesses?

Why was it implemented?

i.	Data Analytics- This process allows us to do data analytics of our data without having to physically install SQL or any big data analytic platform.
ii.	Easy integration- Athena integrates well with other AWS tools like s3, where it automatically stores query responses.
iii.	Data-informed decision-making- Using the SQL service provided easily by AWS Athena, we can evaluate business questions to better inform future decision-making for the city of Vancouver data team.

🔐 DATA SECURITY

Data security is the process that goes into ensuring data privacy and confidentiality from unauthorization, theft, corruption, or possible loss. In the business world today, data security and privacy are a must for all organizations and are required of them to ensure clients' data protection. The data security goal is to ensure transparency, integrity, and availability of data, which can be done with encryption, decryption, access controls (giving specific full access and limited access to some), and authentication, such as user signing in with secure login details unique to specific users.  There are various data security laws in various countries. The Health Insurance Portability and Accountability Act (HIPAA) mentions a few in Canada, which are the PIPEDA (Personal Information Protection and Electronic Documents Act), the Personal Information Protection Act British Columbia (PIPA BC), and the General Data Protection Regulation (GDPR). Amazon Web Services has built-in tools to ensure users provide data security to their information, such as KMS (Key Management Services), which is used to create a key to provide data encryption and decryption. The KMS allows us to create either a Symmetric or asymmetric key. A symmetric key provides one key for encryption and decryption, while an asymmetric one provides separate keys for encryption and decryption. However, for the purpose of this implementation, a symmetric key was created. 
<img width="1470" alt="Screenshot KEY" src="https://github.com/user-attachments/assets/6857bf23-9bc4-4f6e-bf82-3e9fd83b3311" />
The key was created for the city of Vancouver business license dataset, ensuring encryption and decryption of our dataset. KMS provides data security by deploying this key with industry-standard systems. Also, the key created is for datasets stored in the Amazon S3 bucket, and these buckets have versioning enabled to support these procedures as well, and replication rules are put in place to replicate datasets stored in our s3 bucket. The replication rule provides backup for our datasets.
<img width="1470" alt="Decryption Cur " src="https://github.com/user-attachments/assets/61e8686e-6e3e-479f-903f-7525d5e9935b" />
<img width="1470" alt="Decrption screenshot" src="https://github.com/user-attachments/assets/dd6345ac-15da-45db-87ef-1ff47646b4d9" />
<img width="1470" alt="Bucket   decrption Trf" src="https://github.com/user-attachments/assets/f721754d-c1b0-4a22-8ffb-ebe637c95fd9" />
<img width="1470" alt="Buckey versioning Cur" src="https://github.com/user-attachments/assets/d30f5e73-2eef-4f28-8e85-bb65d2ed817e" />
<img width="1470" alt="Bucket versioning" src="https://github.com/user-attachments/assets/0da15d22-1bde-45ae-9b55-932a790169ff" />
<img width="1470" alt="Replication rule (Transfer)" src="https://github.com/user-attachments/assets/4d264a34-43c6-462f-ba94-194ec0ffc0f6" />
<img width="1470" alt="Replication rule (Raw)" src="https://github.com/user-attachments/assets/063deee7-e0f7-4359-8221-d2abc3b4122c" />
<img width="1470" alt="Replication rule (Curated)" src="https://github.com/user-attachments/assets/32fdf3e0-ced0-4ffe-8354-f39dc68b0306" />

The screenshots from the AWS Console showcase key security configurations implemented across the platform, including IAM roles, S3 bucket policies, encryption settings, versioning status, and replication rules created for all our buckets created in s3 for cov-busi-license namely, cov-raw-firepemi, cov-trf-firepmi, and cov-cur-firepemi.
These measures adhere to cloud security best practices and align with the City of Vancouver’s requirements for safeguarding the confidentiality, integrity, and availability of sensitive employee data. By employing a layered security strategy, the platform ensures comprehensive protection at every stage—enhancing trust, compliance, and enterprise readiness.

🧭 DATA GOVERNANCE

In AWS, data governance is about data quality and integrity. Ensuring that the data used for analysis is accurate, consistent, and reliable across our cloud domain. For this procedure in AWS, the AWS Glue was used to implement this for the city of Vancouver data team. A visual ETL was created to extract, transform, and load the profiled data in its targeted location. This process easily integrates with other AWS tools like the s3 bucket, making extracting datasets intended for this purpose easy. An ETL pipeline was created to profile the city of Vancouver business list dataset.

📚 Metadata Management with AWS Glue Data Catalog

Central to the governance strategy is using the AWS Glue Data Catalog to manage and organize metadata—the descriptive information about each dataset. For every dataset ingested and processed, key metadata such as column definitions, data types, data sources, and schema versions was meticulously documented. This comprehensive metadata enhances transparency and enables users across various roles—analysts, engineers, auditors—to efficiently discover, understand, and utilize the data.
As a centralized data inventory, the Data Catalog functions as a powerful discovery hub. It helps prevent data duplication, supports lineage tracking, and fosters secure, collaborative data use across departments and teams.

🧪 Data Quality Checks and Governance Rules

The Data quality control process uses uniqueness, completeness, and freshness to evaluate this dataset. I evaluated the Completeness of the dataset using the "status” column with >= 0.95, the data uniqueness evaluating the “number of employees” column at <20, and lastly, data freshness evaluating the “issued date” at > 30 days. I further set the quality with a passed and failed feature, ensuring that the transform data is distributed into a different folder called quality_check created in the transfer bucket (cov-trf-firepemi) using the set metric passed and failed as these sub-folders were created inside the quality check folder.

🔄 Automated ETL Pipeline

An automated ETL (Extract, Transform, Load) pipeline was developed using AWS Glue to streamline data processing and minimize manual errors:

- **Extraction Phase:**  
  Data is retrieved from a designated Amazon S3 bucket containing cleaned datasets. This stage ensures a consistent and repeatable process for data access and preparation.

- **Transformation Phase:**  
  Extracted data undergoes further cleaning, enrichment, and validation based on established governance rules. AWS Glue jobs apply these transformations, ensuring fields conform to the defined data quality standards.

- **Loading Phase:**  
  Once validated, datasets are automatically categorized into structured S3 folders:  
  - ✅ **Passed folder:** Contains data that meets all quality requirements and is ready for summarization or analysis.  
  - ❌ **Failed folder:** Contains data that did not pass validation checks and requires further review or remediation.

This automated pipeline ensures reliable, scalable, and governance-aligned data processing across the platform.

⚙️ Governance Benefits

✅ **Consistency**  
Standardized schemas and transformation rules promote uniformity across all datasets, ensuring reliable and repeatable processes.

✅ **Trustworthiness**  
Only validated, high-quality data progresses to the analysis layer, enhancing confidence in insights and decision-making.

✅ **Auditability**  
Built-in data lineage and versioning capabilities provide a clear history of changes, supporting transparency and regulatory compliance.

✅ **Automation**  
Minimizes manual effort accelerates processing and enables seamless scalability across growing data volumes.

✅ **Readiness for Advanced Analytics**  
A strong governance foundation enables more sophisticated use cases, including predictive analytics, machine learning pipelines, and real-time dashboarding.

<img width="1470" alt="Visual ETL" src="https://github.com/user-attachments/assets/66f8a4b0-811c-45e8-a4ef-8cb060aa4d64" />
<img width="1470" alt="Passed screenshot" src="https://github.com/user-attachments/assets/cb8ae10b-de00-429a-a58c-49237e7bfb27" />
<img width="1470" alt="Failed screenshot" src="https://github.com/user-attachments/assets/b5bdad64-88d1-488b-a0ca-82f6885a67d3" />

The screenshots above are from the AWS Console, which demonstrates how governance is enforced across the platform—through detailed metadata management, the application of validation rules, and structured, folder-based output segmentation in Amazon S3.
By embedding governance directly into the platform’s architecture, the City of Vancouver can trust the system’s ability to deliver reliable, traceable, and actionable insights. This approach ensures responsible data stewardship while supporting the long-term scalability and sustainability of the analytics platform.

9. 📡 DATA MONITORING

Data monitoring is the process of tracking, observing, and monitoring resources and applications on AWS on our cloud premises. To properly implement data monitoring in AWS, we will be using AWS CloudWatch and Cloud Trail. CloudWatch collects data across AWS resources to give visibility into system-wide performance and allows users to set alarms, automatically react to changes, and gain a unified view of operational health. With CloudWatch, we can create a dashboard that visually monitors our bucket size bytes while we set a metric for limit, so we get notified when we exist our set limit, also the dashboard displays resources usage and lastly, we set an alarm using CloudWatch for threshold.

📊 Real-Time Monitoring with Amazon CloudWatch

Amazon CloudWatch was configured to continuously monitor and track the performance of critical components within the platform, with a primary focus on AWS Glue jobs and Amazon S3 buckets.

#### 🔧 AWS Glue Jobs  
Key metrics and logs monitored include:  
- Job execution status (success or failure)  
- ETL run durations  
- Error logs and exceptions  
- Retry attempts and failure rates  
- Step-level start and end timestamps

#### 🗂️ Amazon S3 Buckets  
Monitored metrics and insights:  
- Bucket size and object count over time  
- Storage cost trends and growth analysis  
- Alerts for unauthorized access or abnormal data write volumes

#### 🚨 CloudWatch Alarms  
Custom alarms were implemented to proactively detect and flag:  
- Glue job failures  
- ETL jobs exceeding defined runtime thresholds  
- Ingestion delays or missed scheduled executions  
- Approaching storage limits in critical S3 buckets

These alarms are integrated with Amazon SNS (Simple Notification Service) to send real-time alerts, enabling rapid response and minimizing potential downtime or data pipeline delays. This proactive monitoring approach strengthens the reliability and operational resilience of the platform.

🔐 User Activity Tracking with AWS CloudTrail

AWS CloudTrail was enabled to provide comprehensive visibility into user and service-level activity across the platform. CloudTrail ensures transparency, accountability, and traceability within the AWS environment by capturing a detailed record of API calls and console actions.

#### Key Benefits:
- **Auditability:** Every interaction with AWS resources—whether through the console, SDK, or CLI—is logged, supporting audit and compliance requirements.
- **Security Monitoring:** Tracks access to sensitive services such as AWS Glue, S3, and IAM, helping detect unauthorized or unusual activity.
- **Operational Insight:** Helps troubleshoot issues by providing event history and contextual information about changes made to the infrastructure.
- **Governance Support:** Reinforces governance policies by enabling teams to verify who accessed or modified data, when, and how.

CloudTrail logs are securely stored in Amazon S3 and can be further analyzed using services like Amazon Athena for advanced querying or integrated with CloudWatch for real-time monitoring and alerts. This level of transparency ensures that the City of Vancouver maintains full control over platform activity, supporting secure and responsible cloud operations.

<img width="1470" alt="alarm" src="https://github.com/user-attachments/assets/07239c1d-484d-4c64-af2f-e2f81673a415" />
<img width="1470" alt="Trail" src="https://github.com/user-attachments/assets/429cd3d9-3bcc-4251-b2fb-2e6acc5727d6" />

### ✅ Benefits of Data Monitoring

- **Early Issue Detection:** Proactively identify and address errors before they affect users or disrupt downstream processes.  
- **Performance Optimization:** Monitor job durations and resource usage to fine-tune ETL performance and enhance processing efficiency.  
- **Security & Compliance:** Maintain full visibility into user actions and system events to uphold accountability and meet governance standards.  
- **Data Freshness Assurance:** Ensure timely data ingestion so stakeholders can consistently access up-to-date and reliable information.

🏗️ Platform Architecture Diagram

Figure: A line graph chart showing the maximum number of employees and average fee paid
![busi](https://github.com/user-attachments/assets/9136c6ef-ae6e-4680-bf02-a9a52e5b98b3)

