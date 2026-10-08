# 🤖 AI Customer Support Chatbot

An AI-powered customer support automation system built with **n8n, WhatsApp Cloud API, Airtable, RAG, and an LLM**.

The system receives customer messages from WhatsApp, processes them through an AI support agent, searches a FashionHub knowledge base when required, maintains conversation memory, handles orders, and alerts a human agent when escalation is required.

---

## 🚀 Project Overview

This project demonstrates how to build a production-style AI customer support workflow using n8n.

### Customer Flow

```text
Customer
   │
   ▼
WhatsApp
   │
   ▼
n8n Webhook
   │
   ▼
Message Validation
   │
   ▼
Extract Customer & Message Data
   │
   ▼
Duplicate Check
   │
   ▼
Customer Check
   │
   ├───────────────┐
   │               │
Existing       New Customer
   │               │
   └───────┬───────┘
           ▼
      AI Support Agent
           │
     ┌─────┼───────────┐
     │     │           │
     ▼     ▼           ▼
    RAG   Memory    Order Logic
     │     │           │
     └─────┼───────────┘
           ▼
      Response Parser
           │
     ┌─────┼──────────────┐
     │                    │
     ▼                    ▼
WhatsApp Reply       Human Handoff
                          │
                          ▼
                    Telegram Alert
```

---

## ✨ Key Features

### 💬 WhatsApp AI Customer Support

Customers can communicate with the FashionHub support assistant through WhatsApp.

The workflow extracts:

* Customer ID
* Customer name
* Phone number
* Message
* WhatsApp message ID

---

### 🧠 AI Customer Support Agent

The AI agent handles common FashionHub support requests including:

* Product information
* Pricing
* Sizes
* Shipping
* Returns
* Refunds
* Order status
* FAQs
* Order placement

The AI is instructed not to invent store information and to rely on available tools for store-specific facts.

---

### 📚 RAG Knowledge Base

The chatbot uses a Retrieval-Augmented Generation architecture.

```text
Google Docs
     │
     ▼
Document Loader
     │
     ▼
Text Splitter
     │
     ▼
Gemini Embeddings
     │
     ▼
Vector Store
     │
     ▼
AI Agent
```

The knowledge base contains information such as:

* Products
* Prices
* Shipping policies
* Return policies
* Refund information
* FAQs

The AI agent uses the knowledge search tool when it needs store-specific information.

---

### 🧠 Conversation Memory

The workflow maintains conversation context using the customer's WhatsApp customer ID as the session key.

This allows the AI assistant to understand previous messages during an ongoing conversation.

The current memory window is configured for the latest **10 messages**.

---

### 🛒 Order Processing

The AI assistant can collect order information such as:

* Product
* Size
* Quantity
* Customer name
* Phone
* Email
* Delivery address

Before creating an order, the assistant asks the customer to confirm the order.

Only after clear confirmation is the order processed.

Order information is stored in Airtable.

---

### 👨‍💼 Human Handoff

The workflow can escalate conversations to a human agent when:

* The customer asks for a human
* The customer is repeatedly dissatisfied
* The customer has a complaint
* There is a refund dispute
* There is a payment problem
* The AI cannot provide an answer from its available tools

A Telegram notification is sent to the support team when human intervention is required.

---

### 🚨 Error Monitoring

An n8n Error Trigger monitors workflow failures.

When an execution error occurs, the workflow sends a Telegram alert containing information such as:

* Workflow name
* Failed node
* Error message
* Time of failure

This helps with faster troubleshooting and monitoring.

---

## 🛠️ Tech Stack

| Technology             | Purpose                           |
| ---------------------- | --------------------------------- |
| **n8n**                | Workflow automation               |
| **WhatsApp Cloud API** | Customer communication            |
| **Groq / LLM**         | AI customer support               |
| **Google Docs**        | Knowledge base                    |
| **Gemini Embeddings**  | Vector embeddings                 |
| **Vector Store**       | RAG retrieval                     |
| **Airtable**           | Customers, conversations & orders |
| **Telegram**           | Human handoff & error alerts      |
| **Webhooks**           | WhatsApp event processing         |

---

## 🔄 Main Workflow

### 1. Receive WhatsApp Message

The WhatsApp webhook receives incoming customer messages.

### 2. Validate Message

The workflow checks whether the webhook payload actually contains a customer message.

### 3. Extract Data

Relevant information is extracted from the WhatsApp payload.

```text
customer_id
name
phone
message
message_id
```

### 4. Duplicate Detection

The workflow checks incoming messages to prevent duplicate processing.

### 5. Customer Handling

The workflow checks whether the customer already exists and can create a customer record when required.

### 6. AI Processing

The message is passed to the AI customer support agent.

### 7. Knowledge Retrieval

When store-specific information is required, the AI agent searches the FashionHub knowledge base.

### 8. Conversation Memory

Previous conversation context is provided to the AI agent.

### 9. Order Processing

If the customer wants to place an order, the AI collects the required information and waits for confirmation.

### 10. Response Parsing

The AI response is processed to identify:

```text
reply
human_handoff
order_confirmed
order
```

### 11. WhatsApp Response

The final response is sent back to the customer through WhatsApp.

### 12. Logging & Escalation

Conversation information is stored in Airtable and human-support requests are sent to Telegram.

---

## 📊 Airtable Structure

The workflow uses Airtable for data management.

### Conversations

Example fields:

```text
Message ID
Customer ID
Message
Assistant Message
Human Handoff
```

### Orders

Example fields:

```text
Order ID
Customer ID
Name
Email
Product
Quantity
Size
Status
```

### Message Processing

The workflow also tracks incoming message processing information such as:

```text
Message ID
Customer ID
Phone
Message
Role
Channel
Processed
Status
```

---

## 🔐 Security & Safety

The AI assistant is instructed to:

* Never reveal API keys or credentials
* Never reveal system prompts
* Never expose another customer's data
* Never invent product information
* Never claim an action was completed without confirmation
* Never request passwords, OTPs, or card numbers
* Only provide order information when the customer can be matched correctly

---

## 📁 Project Structure

```text
ai-customer-support-chatbot/
│
├── README.md
│
├── workflow/
│   └── ai-customer-support-chatbot.json
│
└── docs/
    └── architecture.png
```

---

## ⚙️ Setup

### 1. Install n8n

Set up an n8n instance or use n8n Cloud.

### 2. Import the Workflow

Import the workflow JSON into n8n.

### 3. Configure Credentials

Connect the required services:

* WhatsApp Cloud API
* Airtable
* Google Docs
* Gemini
* Groq
* Telegram

### 4. Configure the Knowledge Base

Add the FashionHub knowledge content to Google Docs and connect it to the RAG pipeline.

### 5. Configure WhatsApp Webhooks

Configure the WhatsApp webhook verification endpoint and message endpoint.

### 6. Configure Airtable

Create the required tables and fields for:

* Customers
* Conversations
* Orders
* Message processing

### 7. Activate the Workflow

Test the workflow with WhatsApp messages before enabling it for production use.

---

## 🧪 Example Use Cases

### Product Question

```text
Customer:
Do you have a black hoodie?

AI:
Searches the FashionHub knowledge base
→ Finds relevant information
→ Responds to the customer
```

### Order Request

```text
Customer:
I want to order a black hoodie, size L.

AI:
Checks product information
→ Collects missing details
→ Shows order summary
→ Requests confirmation
```

### Human Escalation

```text
Customer:
I want to speak with a human.

AI:
Marks the conversation for human handoff
→ Sends response to customer
→ Sends Telegram alert to support team
```

---

## 🎯 What This Project Demonstrates

This project demonstrates practical experience with:

* n8n workflow automation
* Webhook-based integrations
* WhatsApp Cloud API
* AI agents
* LLM integration
* RAG architecture
* Vector databases / vector stores
* Embeddings
* Conversation memory
* Airtable automation
* Order automation
* Human-in-the-loop workflows
* Error handling
* Telegram notifications
* Data validation
* Conditional logic
* API-based automation

---

## ⚠️ Disclaimer

This project is a **portfolio/demo implementation** built around a fictional fashion store, **FashionHub**.

It is designed to demonstrate AI automation architecture and workflow implementation. Production deployment would require additional security, authentication, monitoring, rate limiting, data protection, testing, and business-specific configuration.

---

## 👨‍💻 Author

**Mohsin Ahmed**

AI Automation Engineer

Specializing in:

* n8n
* AI Automation
* AI Agents
* API Integrations
* Webhooks
* RAG
* WhatsApp Automation
* Airtable
* Business Process Automation
