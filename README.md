# Parallel-processing-ETL-pipeline-with-Airflow-on-AWS
## Commands & Guideline use in project
- sudo apt update
- sudo apt install python3-pip
- sudo apt install python3.10-venv
- python3 -m venv airflow_venv

- source airflow_venv/bin/activate

- sudo pip install pandas
- sudo pip install s3fs
- sudo pip install fsspec
- sudo pip install apache-airflow
- sudo pip install apache-airflow-providers-postgres

EC
security groups -> edit inbound rule -> add rule postgresql anywhere ip4

RDS !!!
check security groups -> edit inbound rule 
postgres anywhere ipv4 port 5432 source 0.0.0.0/0

for more detail read below
1. https://www.postgresql.org/download/linux/ubuntu/
2. https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_PostgreSQL.S3Import.html

- psql -h rds-db-eris-yml-1.civit5x12ncs.ap-southeast-1.rds.amazonaws.com -p 5432 -U postgres -W

- CREATE EXTENSION aws_s3 CASCADE;

- sudo apt install awscli

- access key
- aws configure

- aws iam create-policy \
    --policy-name postgresS3Policy-yml-1 \
    --policy-document '{"Version": "2012-10-17", "Statement": [{"Sid": "s3import", "Action": ["s3:GetObject", "s3:ListBucket"], "Effect": "Allow", "Resource": ["arn:aws:s3:::airflow-parallel-eirs-yml", "arn:aws:s3:::airflow-parallel-eirs-yml/*"]}]}'

- aws iam create-role \
    --role-name postgresql-S3-Role-yml-1 \
    --assume-role-policy-document '{"Version": "2012-10-17", "Statement": [{"Effect": "Allow", "Principal": {"Service": "rds.amazonaws.com"}, "Action": "sts:AssumeRole"}]}'

- aws iam attach-role-policy \
    --policy-arn arn:aws:iam::247542112822:policy/postgresS3Policy-yml-1 \
    --role-name postgresql-S3-Role-yml-1

- aws rds add-role-to-db-instance \
   --db-instance-identifier rds-db-eris-yml-1 \
   --feature-name s3Import \
   --role-arn arn:aws:iam::247542112822:role/postgresql-S3-Role-yml-1 \
   --region ap-southeast-1

- airflow standalone

check inbound rule for port 8080

add 2 connection in Airflow
for prosgres and weathermap_api

## Overview in Airflow
![Alt text](<Overview Parallel dag.JPG>)
## Detail in group_a DAGs
![Alt text](<Detail group_a dag.JPG>)