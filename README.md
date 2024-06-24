# AWS-streaming-job :cloud::snake:
## __Basic hands-on project using AWS services S3 and Glue, Python and Power BI__

In this project I decided to use S3 and Glue services from AWS, to transform some files containing information about streaming services platforms, to later create a dashboard in Power BI using the information.
I decided to use this dataset which contains information about some important streaming platform services:
https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download 

Thanks a lot to Sjivam Bansal for creating this valuable source of information to work on :blush:

## __1. Creating a bucket with the info__
S3 is one of the most important services from AWS. It allow users to store unlimited amount of information in a bucket. A bucket is a public resource in the cloud, it provides object-based storage, where data is stored inside S3 buckets in distinct units called objects instead of files. A bucket is a container of files, it belongs to S3 environment.

I uploaded the files into the bucket:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/4d7ed7ce-4c37-45b2-a947-4b5bdc539eeb)


## __2. Creating a Glue ETL__

Once I have it uploaded, I have to transform data. The transformation I want to do is remove the commas, generate columns, and in the first column where the identifiers are of the style s1,s2,s3, simply leave the numeric id without the 's'.

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/d3604277-6c9e-4f18-9d77-b9d38c652449)

I remove the s, convert the first identifier column to type INT and add the streaming service company as a new column:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/d2c0ea33-e92a-4108-af66-91c05c0f57f0)

Finally the job looks like this:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/7d9e167e-c431-452e-be4b-555de97194a5)

Then I can see the outputs in the results folder as I configured before in the job:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/9b1d66bb-98d9-4394-b4eb-94a3532814ba)

The transformation went well:

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/44a9812b-5358-4138-bb01-bba3af3785f2)

Then I need to create an Access key to access to resources from AWS. In this case

![image](https://github.com/emilianoregazzoni/AWS-streaming-job/assets/20979227/2f410aa7-d1e8-4ab4-ac64-6d1cd0e6d7ac)


