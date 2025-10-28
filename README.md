# Hands-on L11: AWS Core Services (S3, Glue, CloudWatch, Athena)

AWS (Amazon Web Services) is a cloud platform that offers a wide range of services for storing, computing, and managing data in the cloud. Amazon S3 helps users store and retrieve data efficiently, while IAM (Identity and Access Management) ensures secure control over who can access AWS resources. CloudWatch is used to monitor applications and system performance, providing real-time logs and alerts. Together, these services help organizations build secure, scalable, and reliable cloud solutions.

---

**Amazon S3**

Amazon S3 (Simple Storage Service) is a cloud-based storage service that allows users to store and retrieve large amounts of data securely over the internet. It is highly scalable, meaning it can handle everything from small files to massive datasets. S3 also offers features like versioning, data backup, and encryption to ensure data safety and reliability. This makes it commonly used for websites, applications, backup solutions, and data analytics.

**Steps to create an Amazon S3 bucket:**

- Log in to the **AWS Management Console** and type '**S3**' in the search box, click on **S3**.

- Click on **Create bucket**.

- Enter a **unique bucket name** and choose the **AWS Region** where you want the bucket to be hosted, and select the Bucket type to be **General purpose**.

- Click **Create bucke**t at the bottom, and your S3 bucket will be created successfully.

- Click on your created Bucket and then **create folders** - **raw** and **processed**

- Inside **raw** folder, **upload the CSV file** named "**Amazon Sale Report.csv**" taken from **https://www.kaggle.com/datasets/thedevastator/unlock-profits-with-e-commerce-sales-data**

**Below shown are the screenshots of my S3 buckets created and the dataset stored in the raw folder:**

<img width="1470" height="837" alt="image" src="https://github.com/user-attachments/assets/35b21bce-00c2-4104-9514-9647bd17659a" />


<img width="1470" height="837" alt="image" src="https://github.com/user-attachments/assets/2c9cee4a-87f5-4801-84b7-46e8fc7be525" />


<img width="1470" height="837" alt="image" src="https://github.com/user-attachments/assets/81450568-f2b1-4ff7-9e42-34453d0c94a5" />

---

**IAM Roles**

IAM roles in AWS are used to grant temporary access permissions to users, applications, or services without needing to share long-term credentials. They define what actions are allowed or denied on specific AWS resources. Roles are often used when one AWS service needs to interact securely with another service. This ensures secure and controlled access management across your cloud environment.

**Steps to create an IAM role with S3 and Glue access for the crawler**

- Type **IAM** in the search box of the AWS Management Console and click on the first option.

- Click Roles and select **Create role**.

- Choose **AWS service** as the Trusted Entity Type and select **Glue** as the use case, then click **Next**.

- Attach the **AmazonS3FullAccess**, **AWSGlueServiceRole** and **AWSGlueConsoleFullAccess** policies.

- Give the role **a name** and **create the role**.

**Shown below is the screenshot of my IAM role created:**

<img width="1470" height="837" alt="image" src="https://github.com/user-attachments/assets/3c87d495-70bd-4e57-a0cc-38edbc7c9c3d" />

---

**Crawlers**

AWS Glue Crawlers automatically scan data stored in services like S3 to determine its structure and format. They then create or update tables in the AWS Glue Data Catalog, making the data ready for querying and analysis. Crawlers help eliminate manual schema creation and keep metadata up to date as new data arrives. This makes data preparation easier and faster in analytics workflows.

CloudWatch Logs is a service in AWS that collects, monitors, and stores log data from applications, services, and system resources. It helps track performance, troubleshoot issues, and analyze application behavior in real time. You can set alerts to notify you when specific events or errors occur. This makes it useful for maintaining visibility and reliability in cloud environments.

**Creating a crawler in Glue Studio to auto-load the raw data into tables and capturing Cloud Watch logs**

- Type **AWS Glue** in the search box of the AWS Management Console and click on the first option.

- Click on **Crawlers** from the left side of the pane

- Click **Create crawler **and provide a **unique name**.

- Click on **Add a data source (your S3 bucket)** and select the **Browse S3** option.

- Choose the **path for the raw folder from your S3 bucket**, then click on **Add an S3 data source**, and click on **Next**

- Select the **IAM role created from the previous step** and click on **Next**

- Choose or create a Glue Data Catalog database where table metadata will be stored. In my case, I selected **"output_db"** from the dropdown

- Review the settings and click **Create**, then **run the crawler** to view the **CloudWatch logs**.

**Below shown are the screenshots of my crawlers created and the CloudWatch logs:**

<img width="1470" height="837" alt="image" src="https://github.com/user-attachments/assets/4240b792-4d37-4b34-9faa-3a0a91bf8214" />

<img width="1470" height="837" alt="Cloud Watch screenshot" src="https://github.com/user-attachments/assets/c21cced6-8e30-45b0-b7f4-89b8c528c1df" />

---

**Athena**

Amazon Athena is an interactive query service in AWS that allows you to analyze data directly in Amazon S3 using standard SQL. It is serverless, meaning there is no need to set up or manage any infrastructure. You only pay for the amount of data scanned by your queries, making it cost-efficient. Athena integrates seamlessly with the AWS Glue Data Catalog to reference tables and schemas. This makes it easy to run analytics and generate insights quickly without complex data pipelines.

**Configuring Athena for Queries**

- Type **Athena** in the search box of the Amazon Management Console and click on the first option.

- If this is your first time using Athena, set up the **query result location** as the **path of the processed folder in the Amazon S3 bucket.**

- Select **output_db** as the database from the dropdown.

- You are now ready to type your queries and view results

**Below is the screenshot of my Athena editor:**

<img width="1470" height="837" alt="image" src="https://github.com/user-attachments/assets/9a4cfbf5-1dda-4bd6-928c-941945d4f695" />

---

**Queries to Run in Athena**

1. This query first calculates the total sales amount for each date in the year 2022. Then, it generates a running (cumulative) total of sales over time, ordered by date. Finally, it displays the first 10 results with both daily and cumulative sales.

<img width="1470" height="837" alt="Query 1" src="https://github.com/user-attachments/assets/0d6e8205-ac8e-49ed-ba7b-090a071668ed" />

<img width="1470" height="837" alt="Query 1_results" src="https://github.com/user-attachments/assets/8826c3b7-0ce4-4b9a-95bc-d35aabe78fb4" />

2. This query groups the data by shipping state and calculates the total profit (sum of amounts) for each state. It then sorts the states in descending order of profit. Finally, it displays the top 10 states with the highest total profit.

<img width="1470" height="837" alt="Query 2" src="https://github.com/user-attachments/assets/e25786e2-4611-4bff-adfd-217d0b6a4172" />

<img width="1470" height="837" alt="Query 2_results" src="https://github.com/user-attachments/assets/0f84777f-bc9e-4d25-a9fb-6a5e4d670761" />

3. This query compares sales for each product category with and without promotions. It calculates how much revenue comes from items sold under promotions and how much comes from items sold without them. It then computes the difference (profit or loss) between the two and sorts categories from the highest to the lowest difference. Finally, it shows the top 10 categories based on this profit/loss value.

<img width="1470" height="837" alt="Query 3" src="https://github.com/user-attachments/assets/3006eecc-7935-4048-8aad-7491da0945bd" />

<img width="1470" height="837" alt="Query 3_results" src="https://github.com/user-attachments/assets/c2612fe6-a951-4409-ad13-860307cbce02" />

4. This query finds the top-performing products within each category. It calculates the total profit for each product (SKU) and ranks them in descending order within their respective category. It then selects only the top 3 ranked products per category and displays the first 10 results from that filtered list.

<img width="1470" height="837" alt="Query 4" src="https://github.com/user-attachments/assets/ca128bf5-0afc-4a2b-9c7a-af09a31f9971" />

<img width="1470" height="837" alt="Query 4_results" src="https://github.com/user-attachments/assets/4e06a5be-1dc5-4b20-88a3-595f5635e055" />

5. This query calculates total sales for each month and then measures how sales changed from one month to the next. It computes the month-over-month growth rate using the difference divided by the previous month’s sales. Finally, it lists the monthly sales along with their growth rates in chronological order, showing the first 10 results.

<img width="1470" height="837" alt="Query 5" src="https://github.com/user-attachments/assets/bc477e8e-2c32-4a9b-8a95-2595d4db33fb" />

<img width="1470" height="837" alt="Query 5_results" src="https://github.com/user-attachments/assets/63657691-4fd6-4baf-a93a-61053a69bba7" />

---

**Challenges faced**

This assignment was smoothly executed without facing any major issues. One tip for executing the assignment smoothly is to configure the AWS Management Console correctly with the proper region selected.

---















