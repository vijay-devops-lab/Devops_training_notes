# Amazon SQS


## What is Full Form Of Amazon SQS?
The full form of ****"Amazon SQS is Simple Queue Service".**** It is fully managed by AWS it's self it will take care of all the aspects like hardware provisioning, maintenance, and scaling. AWS SQS provides communication in a distributed system or application. The applications which are running in the cloud will be distributed across the different servers they need to communicate with each other.

SQS standards for Simple Queue Service offer asynchronous massaged-based communication between two services. Before SQS the communication between the two services was based upon the API calls after the SQS the event producer will notify the consumer in an asynchronous manner.

![AWS-SQS](https://raw.githubusercontent.com/vijay-devops-lab/Devops_training_notes/refs/heads/main/assets/AWS-SQS.png)

The messages that are in the queue are in the form of Jason format and the queue is a holding pool of messages the limit of each message is 256 kilobytes per message. The publishers will publish the messages into the queue and the messages are processed or removed with the help of consumers.

---

## What is SQS (Amazon Simple Queue Service) In AWS?

Amazon Simple Queue Service (SQS) will let you send messages, store the messages, and receive messages between various software components at any amount, without losing of actual messages.  Also, without requiring some other services to be available. so, basically, Amazon SQS is a distributed queue system.

Queues are used to store textual information so it can be received and used by a consumer later on. The ****consumer**** is something that is getting a message from a queue. It can be anything that is able to make an [API](https://www.geeksforgeeks.org/software-testing/what-is-an-api/) call to SQS (application, microservice, human, etc....). Using this paradigm we implement decoupling. 

The ****decoupling**** allows the processing of incoming requests later on. So when the consumer is overloaded, it waits before getting another message. This way our applications become more fault-tolerant. Amazon Simple Queue Service(SQS) is a fully managed queue service in the [AWS cloud](https://www.geeksforgeeks.org/cloud-computing/introduction-to-amazon-web-services/).

## Step-By-Step Explanation To SQS Queues
 (Amazon Simple Queue Service)
Follow the below steps to create a SQS using the AWS console:

****Step 1:**** Open AWS console and type "SQS" in the search bar. And select it.To know how to create AWS account refer to the [Amazon Web Services (AWS) – Free Tier Account Set up.](https://www.geeksforgeeks.org/devops/amazon-web-services-aws-free-tier-account-set-up/)
![](https://raw.githubusercontent.com/vijay-devops-lab/Devops_training_notes/refs/heads/main/assets/aws-sqs-1.png)

**Step 2: Choose Queue Type**

- **Standard** → most common (default)    
- **FIFO** → strict order + `.fifo` suffix required    

👉 For beginners: **Standard Queue**

---

### Step 3: Configure Queue Settings

#### Basic

- **Queue name**: `my-app-queue`
    
- **Visibility timeout**: `30 seconds`
    
    > Time during which message is invisible after being received
    

#### Message settings

- **Message retention**: `4 days` (default)
    
- **Maximum message size**: `256 KB`
    
- **Delivery delay**: `0 seconds`
![](https://raw.githubusercontent.com/vijay-devops-lab/Devops_training_notes/refs/heads/main/assets/Screenshot%202026-01-06%20140956.png)


---

### Step 4: Dead Letter Queue (optional)

Helps when messages fail repeatedly.

1. Enable **Dead-letter queue**
    
2. Create/select another queue (e.g. `my-app-dlq`)
    
3. Set **Max receives**: `3`
    

---

### Step 5: Access Policy

For now:

- Choose **Basic**
    
- Later restrict access to IAM roles/services (Lambda, EC2, ECS)
    

Click **Create queue** ✅


---

## 3️⃣ Send Messages to SQS

### From AWS Console

1. Open the queue
    
2. Click **Send and receive messages**
    
3. Enter message body:
    

`{   "orderId": "1234",   "action": "process-payment" }`

4. Click **Send message**
    
![](https://raw.githubusercontent.com/vijay-devops-lab/Devops_training_notes/refs/heads/main/assets/Screenshot%202026-01-06%20141327.png)

---

## 4️⃣ Receive Messages from SQS (Console)

1. Click **Poll for messages**
    
2. Select message
    
3. Click **Delete** (important!)
    
![](https://raw.githubusercontent.com/vijay-devops-lab/Devops_training_notes/refs/heads/main/assets/Screenshot%202026-01-06%20141345.png)
⚠️ If you don’t delete it, SQS will resend it after visibility timeout.

![](https://raw.githubusercontent.com/vijay-devops-lab/Devops_training_notes/refs/heads/main/assets/Screenshot%202026-01-06%20141358.png)

---

## 5️⃣ IAM Permissions (Very Important)

Attach this policy to **EC2 / Lambda / ECS role**:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "sqs:SendMessage",
        "sqs:ReceiveMessage",
        "sqs:DeleteMessage",
        "sqs:GetQueueAttributes"
      ],
      "Resource": "arn:aws:sqs:REGION:ACCOUNT_ID:my-app-queue"
    }
  ]
}

```
