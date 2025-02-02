# AWS-streaming-job :cloud::snake:
## __Basic hands-on project using AWS services S3 and Glue, Python and Power BI__

In this project I decided to use S3 and Glue services from AWS, to transform some files containing information about streaming services platforms, to later create a dashboard in Power BI using the information.
I decided to use this dataset which contains information about some important streaming platform services:
https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download 

Thanks a lot to Sjivam Bansal for creating this valuable source of information to work on :blush:

## __1. Creating a bucket with the info__ :books:
S3 is one of the most important services from AWS. It allow users to store unlimited amount of information in a bucket. A bucket is a public resource in the cloud, it provides object-based storage, where data is stored inside S3 buckets in distinct units called objects instead of files. A bucket is a container of files, it belongs to S3 environment.

I uploaded the files into the bucket:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/4d7ed7ce-4c37-45b2-a947-4b5bdc539eeb)


## __2. Creating a Glue ETL__ :factory:

Once I have it uploaded, I have to transform data. The transformation I want to do is remove the commas, generate columns, and in the first column where the identifiers are of the style s1,s2,s3, simply leave the numeric id without the 's'.

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/d3604277-6c9e-4f18-9d77-b9d38c652449)

I removed the s, I converted the first identifier column to type INT and I added the streaming service company as a new column:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/d2c0ea33-e92a-4108-af66-91c05c0f57f0)

I did all this transformation using a SQL query transformation step that provides Glue, putting this code in the step was enough:

`select CAST(substring(show_id, 2) AS INT) AS id ,type,title,director,cast,country,date_added,release_year,rating,duration,listed_in,description,  '{supplier_name}' as supplier from myDataSource`

Finally the job looks like this:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/7d9e167e-c431-452e-be4b-555de97194a5)

Then I can see the outputs in the results folder as I configured before in the job:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/9b1d66bb-98d9-4394-b4eb-94a3532814ba)

The transformation went well:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/44a9812b-5358-4138-bb01-bba3af3785f2)

## __3. Download info to desktop__ :snake:

Then I need to create an Access key to access to resources from AWS. In this case I want to download manually the files, when I wanted, so I created a short Python script that provides me this functionality. But for do that, I used my access key provided by AWS.

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/2f410aa7-d1e8-4ab4-ac64-6d1cd0e6d7ac)

The Python script:

```
import boto3
import os

ACCESS_KEY = 'ACCESS_KEY'
SECRET_KEY = 'SECRET_KEY'
bucket_name = 'bucketname'
files = [
    'results/amazonPrime.csv',
    'results/disneyPlus.csv',
    'results/hulu.csv',
    'results/netflix.csv'
]
local_path = 'C:\\Users\\location'

# Crear una sesión de boto3
session = boto3.Session(
    aws_access_key_id=ACCESS_KEY,
    aws_secret_access_key=SECRET_KEY
)

# Crear un cliente S3
s3_client = session.client('s3')

if not os.path.exists(local_path):
    os.makedirs(local_path)
try:
    for file_path in files:
        file_name = file_path.split('/')[-1]
        s3_client.download_file(bucket_name, file_path, os.path.join(local_path, file_name))
        print(f"Archivo descargado: {file_name}")
        
  print("Todos los archivos se descargaron exitosamente.")
    
except Exception as e:
    print(f"Error al descargar archivos: {e}")
```

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/d87b88c5-33e5-4449-981c-b8d713526cc3)

## __4. Power BI Dashboard__ :bar_chart:

Then I created a basic charts in Power BI and measures to find some insights from this dataset. Once I have loaded the information to Power BI I did a basic schema to connect the datasets with a table calendar autogenerated in Power BI:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/d36784a3-b3b5-4284-a29f-78860ea740e1)

Then I created some basic cards to see the information:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/296e0cfd-fdbf-49bc-86f9-e96ce2152037)

Here I can see that Netflix and Amazon Prime have the majority of TV shows. It's something logic, because they are the oldest streaming platforms. Hulu and Disney Plus are newer.
In this datasets, I can see that we have nearly 3 movies for every TV series available on the platforms. There is much more movie content than TV series content overall.

The filter of year is usefull, it allows me to explore over the years, for example, in 2015 I can say that Netflix was much stronger than now. It had almost the half of the shows.

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/fbac6d10-0027-40e7-87b2-de96a06623da)

Then I decided to have a top 5, which series has the most quantity of seassons? According to this dataset, which only considers information from 2021 and earlier, this is the top 5 series with the most seasons.

I also wanted to know which countries produce the most shows, considering both movies and series. So, I generated a top 10, and it's clear that the United States is the leader. We can see that at the end of the ranking, there are a significant number of shows co-created between two countries.

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/37d3e4f2-c78e-48c4-9b1a-6ef78d606b51)

Finally, I would like to know which categories have the most shows. So I created a created an auxiliar table and a measure to see that.

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/d100a054-3f96-4601-8028-dbde70353907)

Clearly, the drama genre is the leader, followed by a relatively even distribution among the next genres.


### Thank you

Thank you to Lucy Wang for your valuable Youtube videos that allowed me to understand some approaches in terms of consuming and transforming data.

The Spark code used for the Glue transformation and the final Power BI are located in the repository. :blush:
