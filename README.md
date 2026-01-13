# EY_ByTech – Multi-Agent RFP Automation Workflow (n8n)

This repository contains an automated **Multi-Agent RFP Processing Workflow** designed for **n8n**.  
It ingests RFP text, extracts structured metadata, evaluates technical requirements, matches SKUs, fetches pricing, computes margins, and generates a sales summary—end-to-end.

This README explains **how to import, configure, and run the workflow** using the included `rfp_multiagent_workflow.json`.

---

## Features

- Accept RFPs via HTTP Webhook
- AI-powered metadata extraction (RFP Analyst Agent)
- Automatic technical match scoring (Technical Agent)
- SKU lookup from Google Sheets
- Automated pricing computation & margin calculation
- Sales-ready executive summary generation
- Fully structured JSON responses
- Modular design using n8n + LangChain

---

## 🚀 Getting Started

### 1️⃣ Import the Workflow

1. Open **n8n**
2. Go to **Menu → Import**
3. Upload the file: `rfp_multiagent_workflow.json`
4. Save the workflow.

---

## 2️⃣ Required Credentials

This workflow uses two credentials.

---

### 🔐 A. OpenRouter API Key (for LLMs)

Required for:

- RFP Analyst
- Technical Agent
- Pricing Agent
- Sales Agent

**Steps:**

1. Get a free API key from https://openrouter.ai
2. In n8n → **Credentials → OpenRouter API**
3. Create a new credential:
   - Set your API key
   - Name it **OpenRouter account**

---

### 🔐 B. Google Sheets OAuth2 (for SKU & Pricing DB)

Workflow uses your Google Sheets as two databases:

- `SKU_DB`  
- `PRICING_DB`

**To configure:**

1. Go to Google Cloud Console
2. Enable **Google Sheets API**
3. Create an **OAuth Client ID**
4. Set redirect URL: [https://api.n8n.cloud/oauth2/callback](https://api.n8n.cloud/oauth2/callback)
5. In n8n → Credentials → **Google Sheets OAuth2 API**
6. Authenticate with your Google account
7. Name the credential: `Google Sheets account`

---

## 3️⃣ Prepare Required Google Sheets

The workflow references the following spreadsheet ID: `1NeadX4RZpm-I-ZpUkRV-O084zuEb033v_WYOMxel6rg`

If you use your own spreadsheet, ensure the sheet structure and column names match exactly.

---

### 📄 SKU_DB Sheet (for Technical Agent)

| sku_id | product_name | category | segment | key_specs |
|--------|--------------|----------|---------|-----------|

`key_specs` should be valid JSON or readable text.

---

### 📄 PRICING_DB Sheet (for Pricing Agent)

| sku_id | base_cost | min_margin_pct | standard_margin_pct |
|--------|-----------|-----------------|---------------------|

---

## 4️⃣ Webhook Setup

Once workflow is activated, the webhook becomes live at: `POST /rfp-analyse`

To activate:

1. Click on the **Webhook node**
2. Copy the **Production URL**
3. Click **Activate Workflow**

Important: Make sure to use the Production URL, not the Test URL.

---

### Sample Request

```json
{
  "rfp_text": "RFP for lighting equipment supply...",
  "project_name": "Lighting Upgrade",
  "client": "ABC Group",
  "deadline": "2026-01-31",
  "execution_id": "demo-001"
}
```

This JSON payload can be sent to the webhook URL to trigger the workflow.
