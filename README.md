# gcp-logs-to-aws
How to Send Logs From GCP to AWS
## Architecture
![alt text](https://github.com/levi-x00/gcp-logs-to-aws/blob/main/arch.jpg?raw=true)
## Usage
1. Create Log Sink and Pub/Sub Topic.
2. Create Cloud Function triggered with created Pub/Sub topic with this code
![alt text](https://github.com/levi-x00/gcp-logs-to-aws/blob/main/hello_pubsub.py?raw=true)