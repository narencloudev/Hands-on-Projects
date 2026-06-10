# AWS Event-Driven Architecture using API Gateway, Lambda, EventBridge and SQS

## Project Overview

This project demonstrates an event-driven architecture on AWS using:

* API Gateway
* AWS Lambda
* Amazon EventBridge
* Amazon SQS

The application receives client information through an API Gateway endpoint. Lambda processes the request and publishes a custom event to EventBridge. EventBridge evaluates the event using rules and routes the message to the appropriate SQS queue based on the client type.

---

## Architecture

```text
Client (Postman)
        │
        ▼
API Gateway
        │
        ▼
Lambda Function
        │
        ▼
EventBridge Event Bus
        │
 ┌──────┴──────┐
 ▼             ▼
Gold Rule    Silver Rule
 ▼             ▼
SQS Gold     SQS Silver
```

---

## Services Used

| Service     | Purpose                                     |
| ----------- | ------------------------------------------- |
| API Gateway | Receives HTTP requests                      |
| Lambda      | Processes requests and publishes events     |
| EventBridge | Routes events using rules                   |
| SQS         | Stores messages for asynchronous processing |
| IAM         | Grants permissions                          |
| CloudWatch  | Logging and monitoring                      |

---

## Step 1: Create SQS Queues

Created two Standard SQS queues:

### Gold Queue

```text
sqs-gold
```

### Silver Queue

```text
sqs-silver
```

Purpose:

* Gold client messages go to Gold queue
* Silver client messages go to Silver queue

---

## Step 2: Create EventBridge Event Bus

Created a custom EventBridge Event Bus:

```text
my-event-bus
```

Purpose:

Receives custom events from Lambda and routes them to targets.

---

## Step 3: Create EventBridge Rules

### Gold Rule

Pattern:

```json
{
  "source": ["lambda-client"],
  "detail-type": ["client-details"],
  "detail": {
    "client-type": ["gold"]
  }
}
```

Target:

```text
sqs-gold
```

---

### Silver Rule

Pattern:

```json
{
  "source": ["lambda-client"],
  "detail-type": ["client-details"],
  "detail": {
    "client-type": ["silver"]
  }
}
```

Target:

```text
sqs-silver
```

---

## Step 4: Create Lambda Function

Python Runtime:

```text
Python 3.13
```

Lambda Code:

```python
import json
import boto3

eventbridge_client = boto3.client('events')

eventbus_name = 'my-event-bus'

def lambda_handler(event, context):

    body = json.loads(event['body'])

    client_name = body['client-name']
    client_number = body['client-number']
    client_type = body['client-type']

    event_detail = {
        'client-name': client_name,
        'client-number': client_number,
        'client-type': client_type
    }

    response = eventbridge_client.put_events(
        Entries=[
            {
                'Source': 'lambda-client',
                'DetailType': 'client-details',
                'Detail': json.dumps(event_detail),
                'EventBusName': eventbus_name
            }
        ]
    )

    print("Event sent to EventBridge:", response)

    return {
        'statusCode': 200,
        'body': json.dumps('Event sent successfully!')
    }
```

Purpose:

* Receives request from API Gateway
* Creates custom event
* Publishes event to EventBridge

---

## Step 5: IAM Permissions

By default Lambda cannot publish events to EventBridge.

Added the following permission to the Lambda execution role:

```json
{
  "Effect": "Allow",
  "Action": "events:PutEvents",
  "Resource": "arn:aws:events:REGION:ACCOUNT_ID:event-bus/my-event-bus"
}
```

Purpose:

Allows Lambda to send events to EventBridge.

---

## Step 6: Create API Gateway

Created:

```text
REST API
```

Resource:

```text
/clients
```

Method:

```text
POST
```

Integration:

```text
Lambda Proxy Integration
```

Purpose:

Allows users to submit client information via HTTP requests.

---

## Testing

### Gold Client Request

```json
{
  "client-name": "Company XXX",
  "client-number": "10234",
  "client-type": "gold"
}
```

Result:

```text
Message delivered to sqs-gold
```

---

### Silver Client Request

```json
{
  "client-name": "Company YYY",
  "client-number": "55422",
  "client-type": "silver"
}
```

Result:

```text
Message delivered to sqs-silver
```

---

## Issue Faced During Implementation

### Problem

Gold messages were successfully delivered.

Silver messages were not reaching the Silver SQS queue.

### Incorrect Rule Pattern

```json
{
  "source": ["lambda-client"],
  "detail-type": ["client-type"],
  "detail": {
    "client": ["silver"]
  }
}
```

### Root Cause

EventBridge requires exact matching.

Lambda was sending:

```json
{
  "source": "lambda-client",
  "detail-type": "client-details",
  "detail": {
    "client-type": "silver"
  }
}
```

Rule pattern fields did not match the event payload.

### Fix

Updated the Silver Rule pattern:

```json
{
  "source": ["lambda-client"],
  "detail-type": ["client-details"],
  "detail": {
    "client-type": ["silver"]
  }
}
```

After updating the rule, messages were successfully routed to the Silver queue.

---

## Key Learnings

* Event-driven architecture
* EventBridge Event Bus
* EventBridge Rules
* Event Pattern Matching
* API Gateway Integration
* Lambda Integration
* SQS Queues
* IAM Permissions
* CloudWatch Logging
* Troubleshooting EventBridge Rules

---

## Benefits of EventBridge

Without EventBridge:

```text
Lambda
 ├─ if gold
 ├─ if silver
 └─ if platinum
```

Business logic grows inside Lambda.

With EventBridge:

```text
Lambda
   │
   ▼
EventBridge
   ├─ Gold Rule
   ├─ Silver Rule
   └─ Platinum Rule
```

Benefits:

* Decoupled architecture
* Better scalability
* Easier maintenance
* Faster Lambda execution
* Easy to add new event consumers

---

## Outcome

Successfully implemented an event-driven architecture where:

* API Gateway receives requests
* Lambda publishes events
* EventBridge routes events
* SQS queues receive messages based on client type

This project demonstrates practical experience with AWS serverless services and event-driven application design.
