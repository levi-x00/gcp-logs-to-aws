# gcp-logs-to-aws
How to Send Logs From GCP to AWS
## Architecture
![alt text](https://github.com/levi-x00/gcp-logs-to-aws/blob/main/arch.jpg?raw=true)
## Usage
1. Create Log Sink with the following pattern and Pub/Sub as the destination, and create a new Pub/Sub topic from here.
```
logName="projects/xtr-gcc-dev/logs/cloudaudit.googleapis.com%2Factivity"
protoPayload.methodName:"compute.firewalls"
```
2. Create Cloud Function triggered with created Pub/Sub topic with this requirements and code
```
# Function dependencies, for example:
# package>=version
requests
```
```py
import base64
import os
import requests
import json

def hello_pubsub(event, context):
    """Triggered from a message on a Cloud Pub/Sub topic.
    Args:
         event (dict): Event payload.
         context (google.cloud.functions.Context): Metadata for the event.
    """
    pubsub_message = base64.b64decode(event['data']).decode('utf-8')
    proto_payload = json.loads(pubsub_message)['protoPayload']

    if 'response' in proto_payload:
        endpoint = os.environ['URL']
        r = requests.post(url=endpoint, data=pubsub_message)
        print(pubsub_message)
    else:
        print("no response in 'protoPayload'")
```
3. Create lambda function with this code
```py
import json

def lambda_handler(event, context):
    print(json.dumps(event))
    return {
        'statusCode': 200,
        'body': json.dumps('logs printed!')
    }
```
4. Create API Gateway with POST method, name the resource, deploy it.
5. Create a firewall rule in GCP, then try to edit the rules to generate some events.