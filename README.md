# gcp-logs-to-aws
How to Send Logs From GCP to AWS
## Architecture
![alt text](https://github.com/levi-x00/gcp-logs-to-aws/blob/main/arch.jpg?raw=true)
## Usage
1. Create Log Sink and Pub/Sub Topic.
2. Create Cloud Function triggered with created Pub/Sub topic with this requirements and code
```
# Function dependencies, for example:
# package>=version
requests
```
```
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