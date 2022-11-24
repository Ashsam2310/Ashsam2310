import boto3
from smtplib import SMTP
from datetime import datetime
import os
from email.message import EmailMessage

Key=[]
ses=boto3.client('ses',region_name='us-east-1a')
client=boto3.client('s3')
result = client.list_objects(Bucket='phanalytics-dev', Prefix = 'DQRT/Outbound/')
contents=result.get('Contents')  
for i in contents:
  k=i.get('Key')
  response = client.head_object(Bucket='phanalytics-dev', Key=k)
  size = response['ContentLength']
  if k[-1]!='/':
    data= client.get_object(Bucket='phanalytics-dev',Key=k)
    content = data['Body'].read().decode("utf-8")
    Key=k

mesg = EmailMessage()
mesg["From"] = "Ashish.Kumar-Samal@eversidehealth.com"
mesg["Subject"] = "find attachment"
mesg["To"] = "Ashish.Kumar-Samal@eversidehealth.com"
mesg.set_content("This is the message body")
mesg.add_attachment(content, filename=Key)                 