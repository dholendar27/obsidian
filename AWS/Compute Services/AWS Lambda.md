---
date created: 2025-10-28 11:45
date updated: 2025-10-28 11:57
---

AWS lambda is a serverless computing service provided by the Amazon web services. Lambda functions helps the users to create functions, self contained application written in one of the supported languages and runtime, and upload them to AWS Lambda, Which executes the function in an efficient and flexible manner.

**Serverless** means you don’t have to think about:

- Servers
- Operating systems
- Scaling up/down
- Fault tolerance
- Maintenance

You **only pay for the compute time** your code actually uses.

## How AWS Lambda Works

Here’s the general workflow:

1. **Write Code (Lambda Function)**
   - Code can be written in languages like Python, Node.js, Java, Go, C#, Ruby, etc.
   - Example: a Python function that resizes an image or processes a log file.
2. **Upload or Deploy to AWS Lambda**
   - You can upload the code directly, or package it as a ZIP file / container image.
   - You define a “Handler” — the function entry point (like `lambda_function.lambda_handler`).
3. **Trigger (Event Source)**
   - Lambda functions run **in response to events**.
   - Example triggers:
     - API Gateway (HTTP request)
     - S3 (file upload)
     - DynamoDB Streams (data changes)
     - CloudWatch Events (scheduled cron job)
     - SNS/SQS (message notifications)
4. **Execution**
   - AWS automatically provisions a runtime environment, executes your function, and scales as needed.
   - If multiple events occur simultaneously, Lambda runs multiple instances in parallel.
5. **Automatic Scaling**
   - Each event triggers a separate function instance — scaling up automatically.
   - No need to configure autoscaling manually.
6. **Pay Only for What You Use**
   - You’re billed per:
     - **Number of requests** (invocations)
     - **Execution duration** (in milliseconds)
     - **Memory allocated**

### 1. Lambda Handler

The **handler** is the entry point for a Lambda function. It’s the method that AWS Lambda calls when the function is invoked.

- **Definition:** A handler is a specific function in your code that AWS Lambda can execute
- **Location:** You define it when you upload the function, e.g., `file_name.function_name`.
- **Signature:** It usually accepts two parameters:
  1. `event` – Data passed to the function when it is invoked (e.g., JSON from S3 or API Gateway).
  2. `context` – Provides runtime information, such as:
     - `context.aws_request_id` (unique ID for the request)
     - `context.get_remaining_time_in_millis()` (time left before timeout)
     - `context.memory_limit_in_mb` (memory allocated)

**Example in Python:**

```python
def lambda_handler(event, context):
    print("Event received:", event)
    return {
        'statusCode': 200,
        'body': 'Hello from Lambda!'
    }
```

- Here, `lambda_handler` is the handler.
- AWS will execute this function whenever the Lambda is triggered.

---

### 2. **Lambda Invocation Types**

Lambda functions can be invoked in two ways:

#### a) **Synchronous Invocation**

- AWS waits for the function to process the event and return a response.
- Example: API Gateway calls a Lambda function to respond to an HTTP request.

#### b) **Asynchronous Invocation**

- AWS queues the event for Lambda to process later.
- Example: S3 triggers a Lambda when a file is uploaded; Lambda processes it asynchronously.

---

### 3. **Event Sources**

Lambda can be triggered by **various AWS services**, including:

| Event Source                        | Example Use Case                            |
| ----------------------------------- | ------------------------------------------- |
| **S3**                              | Process images or files on upload           |
| **DynamoDB**                        | Update downstream systems when data changes |
| **API Gateway**                     | Handle HTTP requests                        |
| **CloudWatch Events / EventBridge** | Scheduled tasks (cron jobs)                 |
| **SNS / SQS**                       | Message processing, notifications, queues   |

Each event source passes **event data** to the handler. The `event` parameter in your handler contains all relevant details.

---

### 4. **Context Object**

The `context` object gives runtime information about the Lambda execution:

- `context.function_name` → Lambda function name
- `context.memory_limit_in_mb` → Configured memory
- `context.aws_request_id` → Unique ID for request
- `context.get_remaining_time_in_millis()` → Remaining execution time
- Useful for logging, error handling, or managing timeouts.

---

### 5. **Other Important Lambda Concepts**

#### a) **Environment Variables**

- Store configuration values separate from code.
- Example: API keys, database URLs.
- Access in Python: `os.environ['VARIABLE_NAME']`

#### b) **Layers**

- Reusable packages that you can attach to multiple Lambda functions.
- Helps avoid duplicating libraries across functions.

#### c) **Timeout and Memory**

- You configure **timeout** (max execution time) and **memory** allocation per function.
- Memory size affects CPU allocation; higher memory = faster execution in some cases.

#### d) **Deployment Packages**

- Zip files with your code and dependencies.
- Container images (up to 10 GB) are also supported for more complex applications.
