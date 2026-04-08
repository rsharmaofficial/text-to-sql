# 🧪 LAB 2 — Creating Azure OpenAI Service & App Service

## Overview

In this lab, you will provision the two remaining Azure services: Azure OpenAI (the AI engine that converts natural language to SQL) and Azure App Service (the web server that hosts your Python application). You will also configure all required environment variables on the App Service.

## Objectives
- Create an Azure OpenAI Service resource
- Deploy the GPT-4.1 model
- Create an App Service Plan
- Create an App Service configured for Python 3.11
- Set all required environment variables

## Estimated Duration
⏱ 60 Minutes

---

## Task 2.1 — Create Azure OpenAI Service

**Description:**
Azure OpenAI Service gives you access to OpenAI's GPT-4.1 model through Microsoft's secure cloud. This is the AI brain of your application — it reads your question, understands your database schema, and generates the correct SQL query automatically.

### Steps

**Step 1:** In the Azure Portal search bar, type **OpenAI** and select it.
![](./Media/Lab2/image1.png)

**Step 2:** On the next screen make sure you have selected **Azure OpenAI(1)** Then click on **Create(2)** and select **Azure OpenAI(3)**.
![](./Media/Lab2/image2.png)

**Step 3:** On the **Basics** tab, fill in:
- **Subscription:** Your subscription
- **Resource Group:** `textsql-rg`
- **Region:** `West US` (best GPT-4.1 availability)
- **Name:** `textsql-openai`
- **Pricing Tier:** `Standard S0`
![](./Media/Lab2/image3.png)


**Step 4:** Click **Next** on the Network tab (leave defaults — All networks).

**Step 5:** Click **Next** on the Tags tab.

**Step 6:** Click **Review + Create**, then click **Create**.

**Step 7:** Wait for deployment (2–3 minutes), then click **Go to resource**.


![](./Media/Lab2/image4.png)

> ✅ **Verify:** OpenAI resource shows status **Active**

---

## Task 2.2 — Deploy GPT-4.1 Model

**Description:**
Creating the Azure OpenAI resource alone is not enough — you must deploy a specific AI model inside it. You will deploy GPT-4.1, which will be the model used by your application to generate SQL queries and format natural language answers.

### Steps

**Step 1:** Inside `textsql-openai`, click **Go to Foundary Portal**.

```This opens the Foundary Portal in a new tab, which is where you manage your OpenAI models and deployments.```

**Step 2:** Under the shared resources section in the left menu, click on **Deployments**.

**Step 3:** Click **+ Deploy model(1)**, then select **Deploy base model(2)**.

![](./Media/Lab2/image5.png)

**Step 4:** In the model list, search for **gpt-4.1(1)** and select **gpt-4.1(2)**.
![](./Media/Lab2/image6.png)

**Step 5:** Click **Confirm(3)**.

**Step 6:** On the deployment dialog, Click on **Customize** fill in:
- **Deployment name:** `gpt-4-1`
- **Deployment Type:** Global Standard
- **Model version:** Latest available
- **Tokens per minute (TPM):** 10K or higher

![](./Media/Lab2/image7.png)

**Step 6:** Click **Deploy** and wait for status to show **Succeeded**.
![](./Media/Lab2/image8.png)


> ✅ **Verify:** Model `gpt-4-1` shows state **Succeeded** in deployments info

---

## Task 2.3 — Note Your OpenAI Credentials

**Description:**
You will need the OpenAI endpoint URL and API key when configuring your App Service environment variables. Save these values now.

### Steps

**Step 1:** Inside `textsql-openai`, go to **Resource Management → Keys and Endpoint** in the left menu.

**Step 2:** Copy and save the following:
- **Endpoint:** `https://textsql-openai.openai.azure.com/`
- **KEY 1:** (copy the full key value)

![](./Media/Lab2/image9.png)


> 📝 **Save these values — you will use them in Task 2.6**

---

## Task 2.4 — Create App Service Plan

**Description:**
The App Service Plan defines the compute resources — CPU, RAM, and pricing tier — that power your web application. Think of it as the "server" that your App Service runs on.

### Steps

**Step 1:** In the Azure Portal search bar, type **App Service plans** and select it.

![](./Media/Lab2/image10.png)

**Step 2:** Click **+ Create**.

**Step 3:** Fill in:
- **Subscription:** Your subscription
- **Resource Group:** `textsql-rg`
- **Name:** `textsql-asp`
- **Operating System:** `Linux`
- **Region:** `West US`
- **Pricing Plan:** Click **Explore pricing plans** → Select **Basic B1**
![](./Media/Lab2/image11.png)


**Step 4:** Click **Review + Create**, then click **Create**.

> ✅ **Verify:** App Service Plan `textsql-asp` created with OS = Linux
![](./Media/Lab2/image12.png)
---

## Task 2.5 — Create App Service

**Description:**
The App Service is the actual web application host. It will run your Python FastAPI application 24/7 and serve it via a public HTTPS URL. All users will access your application through this URL.

### Steps

**Step 1:** In the Azure Portal search bar, type **App Services** and select it.
![](./Media/Lab2/image13.png)

**Step 2:** Click **+ Create**, then select **Web App**.

![](./Media/Lab2/image14.png)

**Step 3:** On the **Basics** tab, fill in:
- **Subscription:** Your subscription
- **Resource Group:** `textsql-rg`
- **Name(1):** `textsql-webapp`
- **Publish(2):** `Code`
- **Runtime Stack(3):** `Python 3.11`
- **Operating System:** `Linux`
- **Region(4):** `West US`
- **Linux Plan(5):** Select `textsql-asp`

![](./Media/Lab2/image15.png)


**Step 4:** Click **Next: Database** leave default → **Deployment** leave defaults → click **Next: Networking** leave defaults.


**Step 6:** Click **Review + Create**, then click **Create**.

**Step 7:** Wait for deployment, then click **Go to resource**.
![](./Media/Lab2/image16.png)

>App Service overview showing textsql-webapp URL: https://textsql-webapp.azurewebsites.net]


> ✅ **Verify:** App Service URL = `https://textsql-webapp.azurewebsites.net` and Status = Running

---

## Task 2.6 — Configure Environment Variables

**Description:**
Your Python application reads its configuration from environment variables — these are secure key-value pairs stored on the App Service. You will add all the connection details your app needs to talk to Azure OpenAI and Azure SQL.

### Steps

**Step 1:** Inside `textsql-webapp`, go to **Settings → Environment Variables** in the left menu.

**Step 2:** Click **+ Add** for each of the following variables one by one and click apply after adding each variable:
![](./Media/Lab2/image17.png)

| Variable Name | Value |
|---------------|-------|
| `OPENAI_API_KEY` | Your KEY 1 from Task 2.3 |
| `AZURE_OPENAI_ENDPOINT` | `https://textsql-openai.openai.azure.com/` |
| `AZURE_OPENAI_VERSION` | `2024-12-01-preview` |
| `AZURE_OPENAI_CHAT_DEPLOYMENT` | `gpt-4-1` |
| `SQL_SERVER` | `textsql-sqlserver.database.windows.net` |
| `SQL_DATABASE` | `textsqldb` |




**Step 3:** Click **Apply**, then click **Confirm** to save and restart the app.
![](./Media/Lab2/image18.png)

> ✅ **Verify:** All 6 environment variables appear in the list and are saved

---

## Task 2.7 — Set Startup Command

**Description:**
Azure App Service needs to know how to start your Python application. You will set the startup command that installs dependencies and launches the FastAPI server using Gunicorn.

### Steps

**Step 1:** Inside `textsql-webapp`, go to **Settings → Configuration(1)** in the left menu.

**Step 2:** Click the **Stack settings** tab.

**Step 3:** Find **Startup Command(3)** and enter:
```
bash startup.sh
```
![](./Media/Lab2/image19.png)

**Step 4:** Click **Apply**, then click **Continue**.

> ✅ **Verify:** Startup command saved as `bash startup.sh`

---

### ✅ Lab 2 Complete — Checklist

- [ ] Azure OpenAI `textsql-openai` created in East US
- [ ] GPT-4.1 model deployed with name `gpt-4-1` — Status = Succeeded
- [ ] OpenAI Endpoint and API Key copied and saved
- [ ] App Service Plan `textsql-asp` created — Linux, Basic B1
- [ ] App Service `textsql-webapp` created — Python 3.11, Linux
- [ ] All 6 environment variables added to App Service
- [ ] Startup command set to `bash startup.sh`

---

---