# 🚀 Azure Queue Storage Project
Hands-on project demonstrating asynchronous messaging using Azure Queue Storage with Python producer-consumer model.

---

# 🎯 Project Overview

This project demonstrates how to use **Azure Queue Storage** for:

* Asynchronous message processing
* Decoupling applications
* Background job execution

---

# 🏗️ Architecture

![Image](https://learn.microsoft.com/en-us/azure/architecture/patterns/_images/queue-based-load-leveling-function.png)

![Image](https://andrewlock.net/content/images/2024/azure_storage_queue_banner.webp)

![Image](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/integration/media/enterprise-integration-message-broker-events.svg)

![Image](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/images/web-queue-worker-logical.svg)

### Flow:

1. User/Application sends message → Queue
2. Queue stores message
3. Worker/Consumer reads message
4. Process task (e.g., email, log, order)

---

# 🧰 Services Used

| Service                | Purpose           |
| ---------------------- | ----------------- |
| Azure Storage Account  | Stores queue      |
| Azure Queue Storage    | Message queue     |
| Azure VM / App Service | Worker processing |
| Azure CLI              | Deployment        |

---

# ⚙️ Step 1: Create Storage Account

```bash
az group create --name queue-rg --location eastus

az storage account create \
  --name mystoragequeue12345 \
  --resource-group queue-rg \
  --location eastus \
  --sku Standard_LRS
```

---

# 📬 Step 2: Create Queue

```bash
az storage queue create \
  --name myqueue \
  --account-name mystoragequeue12345
```

---

# 📩 Step 3: Send Message (Producer)

```bash
az storage message put \
  --queue-name myqueue \
  --content "Hello from Azure Queue!" \
  --account-name mystoragequeue12345
```

---

# 📥 Step 4: Receive Message (Consumer)

```bash
az storage message get \
  --queue-name myqueue \
  --account-name mystoragequeue12345
```

---

# 🐍 Step 5: Python Producer & Consumer

## 📌 Install SDK

```bash
pip install azure-storage-queue
```

---

## 🔹 Producer Code

```python
from azure.storage.queue import QueueClient

conn_str = "YOUR_CONNECTION_STRING"
queue_name = "myqueue"

queue_client = QueueClient.from_connection_string(conn_str, queue_name)

message = "Order ID: 12345"
queue_client.send_message(message)

print("Message sent!")
```

---

## 🔹 Consumer Code

```python
from azure.storage.queue import QueueClient

conn_str = "YOUR_CONNECTION_STRING"
queue_name = "myqueue"

queue_client = QueueClient.from_connection_string(conn_str, queue_name)

messages = queue_client.receive_messages()

for msg in messages:
    print("Processing:", msg.content)
    queue_client.delete_message(msg)
```

---

# 💡 Real-Time Use Cases

* 🛒 Order Processing System
* 📧 Email Notification Service
* 📊 Background Data Processing
* 🔄 Microservices Communication

---

# 🔐 Security Best Practices

* Use **Connection Strings from Key Vault**
* Enable **Private Endpoint**
* Use **SAS Tokens instead of full keys**
* Rotate access keys regularly

---

# 📁 Project Structure

```
azure-queue-storage-project/
│
├── producer/
│   └── send_message.py
│
├── consumer/
│   └── receive_message.py
│
├── terraform/ (optional)
│   └── main.tf
│
├── README.md
```

---

# 🔥 Advanced Enhancements

* Add **Azure Function (Queue Trigger)**
* Integrate with **AKS Worker Pods**
* Use **Terraform for IaC**
* Add **Retry Logic + Dead Letter Queue**
* CI/CD using GitHub Actions

---

# 🧪 Interview Questions (Bonus)

* What is Azure Queue Storage?
* Difference: Queue vs Service Bus?
* Max message size? (64 KB)
* Visibility timeout concept?
* Poison message handling?

---
