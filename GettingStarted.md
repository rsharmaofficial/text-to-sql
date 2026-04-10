#  Microsoft Azure — Function Call Dynamic Query
## Hands-On Lab Guide

> **Total Labs:** **4** | **Estimated Duration:** **3.5–4 Hours**

---

##  Lab Structure Overview

| Lab | Title | Duration |
|-----|-------|----------|
| Lab 1 | Provisioning Azure SQL Server & Database | 55 mins |
| Lab 2 | Creating Azure OpenAI Service & App Service | 60 mins |
| Lab 3 | Configuring Security & Managed Identity | 45 mins |
| Lab 4 | Deploying the Application & Testing | 60 mins |

---

## Resource Naming Reference

Use these names throughout all labs:

| Resource | Name to Use |
|----------|-------------|
| Resource Group | `textsql-rg` |
| SQL Server | `textsql-sqlserver` |
| SQL Database | `textsqldb` |
| OpenAI Service | `textsql-openai` |
| OpenAI Deployment | `gpt-4-1` |
| App Service Plan | `textsql-asp` |
| App Service | `textsql-webapp` |
| Managed Identity | `textsql-identity` |
---

---
### Three-Stage AI Pipeline
 
```
User Question  →  SQL Generation (GPT-4.1)  →  Answer Synthesis  →  Natural Language Response
```
 
| Stage | Description |
|-------|-------------|
| 1. Natural Language Understanding | User types a question in plain English |
| 2. SQL Generation | Azure OpenAI GPT-4.1 generates the appropriate SQL query |
| 3. Answer Synthesis | Query results are formatted into a natural language response |
 
---
 
##  Azure Architecture
 
| Azure Resource | Purpose | Configuration |
|----------------|---------|---------------|
| Azure App Service | Hosts the Python FastAPI web application | Linux, Python 3.11, Startup script |
| Azure OpenAI Service | GPT-4.1 for SQL generation and answer formatting | GPT-4.1 deployment, Token-based auth |
| Azure SQL Database | Stores relational data (customers, products, orders) | SalesLT schema, AAD auth enabled |
| Azure SQL Server | Manages the SQL Database instance | AAD-only authentication |
| User Managed Identity | Passwordless authentication between services | Assigned to App Service |
| App Service Plan | Compute resources for the App Service | Linux hosting plan |
---
 
##  Application Flow
 
```
Step 1  →  You opens the web app URL in any browser
Step 2  →  You types a question (e.g., "Give me top 5 selling products")
Step 3  →  App Service sends the question to Azure OpenAI GPT-4.1
Step 4  →  GPT-4.1 generates the correct SQL query based on the database schema
Step 5  →  App Service executes the SQL query against Azure SQL Database
Step 6  →  Database returns the raw results to the application
Step 7  →  App Service sends the results back to GPT-4.1 for formatting
Step 8  →  GPT-4.1 returns a clear, human-readable natural language answer
Step 9  →  You sees the final answer displayed on the web page
```
 
---
 
##  Lab Objectives
 
### Primary Objectives
 
- Design and deploy a complete cloud-native AI web application on Microsoft Azure from scratch
- Integrate Azure OpenAI GPT-4.1 with Azure SQL Database to create a natural language query interface
- Implement enterprise-grade security using Azure Managed Identity and RBAC
- Deploy a Python FastAPI application to Azure App Service with automated startup and dependency management
- Configure Azure SQL Database with Azure Active Directory authentication for passwordless access
 
### Security Objectives
 
- Eliminate all hardcoded credentials and API keys from application code
- Assign `Cognitive Services OpenAI User` role to Managed Identity for OpenAI access
- Configure Azure SQL Database to accept Managed Identity token-based authentication
- Implement read-only query validation to prevent unauthorized data modification
- Apply the principle of least privilege across all Azure resource permissions
 
---
 
##  Prerequisites
 
### Good to Have Knowledge About:
 
- Basic understanding of cloud computing concepts
- Familiarity with Python programming (beginner to intermediate level)
- Basic knowledge of SQL (SELECT statements, JOINs)
- Understanding of REST APIs and HTTP methods
- Familiarity with Azure services (App Service, SQL Database, OpenAI) is a plus but not required

                        Happy Learning!