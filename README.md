# Demos for Unlock insights with AWS GenAI services (re:Invent BOA303)

This repository is complementary to breakout session on "BOA303 - Unlock insights with AWS GenAI services" at [re:Invent 2023](https://reinvent.awsevents.com/)

🎥 Watch the session [here](https://bit.ly/BOA303-reinvent-23)

In these demos, you will explore how RAG ( Retrieval-Augmented Generation ) can enhance AI models by leveraging external data sources to provide context-aware answers and unlock insights.

Learn 2 ways to use your data with GenAI models:

1️⃣ DEMO 1: [Deploying Llama2 with Sagemaker, CodeWhisperer + Kendra for data retrieval](#demo-1---rag-approach-with-amazon-kendra)

2️⃣ DEMO 2: [Using Claude 2 with Bedrock + Vector DB for data retrieval](#demo-2---rag-approach-with-vectordb)


## DEMO 1 - RAG approach with Amazon Kendra

### Prerequisites:

1. Follow [instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-updated-launch.html#studio-updated-launch-prereq) to setup Amazon SageMaker Studio prerequisites

2. Follow [instructions](https://docs.aws.amazon.com/codewhisperer/latest/userguide/sagemaker-setup.html) to enable Amazon CodeWhisperer extension for SageMaker Studio


### Setup for Demo 1

#### 1. Create S3 Bucket and download data

- Open Amazon S3 console. Click **Create a bucket**. Provide a unique name `reinvent-demo-yourname`. Keep other options as default. Click **Create bucket**. 
- Go to [https://www.macrotrends.net/countries/USA/united-states/inflation-rate-cpi](https://www.macrotrends.net/countries/USA/united-states/inflation-rate-cpi). Click **download historical data**.
- Download Starbucks annual report PDF.
- Upload downloaded csv and pdf files into S3 bucket.

#### 2. Amazon Kendra Index & data sources 

- Open Amazon Kendra console. Click **Create an index**. Provide index name. Under IAM Select **Create a new role (Recommended)**. Enter role name. Click **Next**, then **Next** in the follow-up screens, and Click **Create**.

<div align="center">
    <img src="images/image-01.jpg" width="600">
</div>


- Copy index name and save for future references. Wait for index to become active and click **Add data sources**.
- Find Amazon S3 Connector. Click **Add connector**. 

<div align="center">
    <img src="images/image-02.jpg" width="600">
</div>

- Provide Data Source name. Click **Next**. Create a new role or select an existing role.
- Click **Browse S3** and select the bucket that you created in Step 1. At the bottom select **Frequency Run on demand**. Click **Next**, then **Next** again then **Add data resources**.

<div align="center">
    <img src="images/image-03.jpg" width="600">
</div>

- Once the data source is created, click **Sync Now** button at the top.
- Let’s add one more data source. Click on **Data sources** on the left pane then click **Add data source**. Search for Web and pick **Web Crawler V2.0**.

<div align="center">
    <img src="images/image-04.jpg" width="600">
</div>

- Provide data source name and click **Next**. Copy URL from step 1 into **Source URLs**. Select **Create a new role (Recommended)** at the bottom, type name, and click **Next**. 

<div align="center">
    <img src="images/image-05.jpg" width="600">
</div>

- Keep default options and select Frequency **Run on demand**. Click **Next**, then **Next** again then **Add data resources**.
- Once the data source is created, click **Sync Now** button at the top.


#### Demo 1 - Part 1: Notebook updates

1. Open SageMaker Studio and copy `Demo1 - RAG with SageMaker and Kendra.ipynb` notebook.

2. Install dependencies as needed

   ```bash
   !pip install —upgrade —quiet sagemaker
   !pip install langchain


3. Update s3_path with path to your bucket created in prerequisites. Update if needed csv file name

<div align="center">
    <img src="images/image-06.jpg" width="600">
</div>


#### Demo 1 -  Part 2: Notebook updates

1. Copy your SageMaker Endpoint and replace in the code. In SageMaker Console, go to Sidebar > Home > Deployment dropdown > Endpoints — copy the name of the LLM and paste in the 3rd row below

<div align="center">
    <img src="images/image-07.jpg" width="600">
</div>


#### Demo 1 - Part 3: Notebook updates

1. Update Kendra IndexID in the code and AWS region.


 Run your notebook and ask questions!





## DEMO 2 - RAG approach with VectorDB 

### Prerequisites:

1. Follow [instructions](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-updated-launch.html#studio-updated-launch-prereq) to setup Amazon SageMaker Studio prerequisites

2. Follow [instructions](https://docs.aws.amazon.com/codewhisperer/latest/userguide/sagemaker-setup.html) to enable Amazon CodeWhisperer extension for SageMaker Studio

3. Follow [instructions](https://docs.aws.amazon.com/bedrock/latest/userguide/model-access.html) to request access to Antropic Claude model inside Amazon Bedrock

4. Create a free account with [pinecone.io](https://www.pinecone.io/) and create index “demoindex”.
    1. Click indexes on the left pane then click button Create index.
    2. Give an index the name “demoindex”. Configure dimensions 1536, metric cosine, then click Create index. Save index name and environment name in your notes. We will use it later. 
    3. Go to API keys on the left pane and copy API key. Save it. We will use it later.

<div align="center">
    <img src="images/image-09.jpg" width="600">
</div>


### Demo 2 - Part 1:Bedrock Configuration

1. Open SageMaker Studio and copy `Demo2 - RAG with Bedrock and Vector DB.ipynb` notebook.

2. Create a `keys.env` file and add your Pinecone account information. Save this file in the same folder as your notebooks.

   ```env
   PINECONE_API_KEY=your_pinecone_API_key
   PINECONE_ENVIRONMENT=your_pinecone_env

3. Install dependencies as needed

    ```bash
    !pip install -U \
    langchain==0.0.306 \
    boto3 \
    botocore \
    pypdf==3.15.2 \
    pinecone-client==2.2.4 \
    apache-beam==2.50.0 \
    datasets==2.14.5 \
    tiktoken==0.4.0 —force-reinstall —quiet



Execute commands in the notebook.