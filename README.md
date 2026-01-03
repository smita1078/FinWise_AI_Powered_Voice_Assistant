
# 📞 AI-Powered Multilingual Voice Bot (Dialogflow CX + Cloud Functions)

An AI-powered, multilingual voice bot built using **Dialogflow CX**, **Google Cloud Functions**, and **Cloud SQL**.
The system supports **DTMF-based flows**, **AI conversational flows**, **scam reporting**, **bank branch lookup**, and an **NGO connect feature**, with all interactions logged for continuous system improvement.

---

## 🚀 Features

### 🔹 Telephony-Based Voice Bot

* End users call the bot via **Phone Gateway**
* Language selection at the beginning (English, Hindi, Marathi, German)
* Works with both **DTMF input** and **AI-powered conversations**

### 🔹 Dual Interaction Modes

1. **Normal Flow (DTMF-based)**

   * Menu-driven responses
   * Predictable and fast navigation
2. **AI-Powered Conversational Flow**

   * Natural language understanding
   * Context-aware responses using Dialogflow CX

### 🔹 Scam Reporting Module

* Users can report suspicious calls or messages
* Automatically classifies:

  * `scam_type` (Phishing, Loan Scam, Job Scam, etc.)
  * `risk_level` (High / Medium / Low)
* Logs results into **Cloud SQL**
* Used to analyze scam trends and improve the system

### 🔹 Bank Branch Lookup

* Lookup bank branches using:

  * Bank name (supports aliases)
  * Pincode
* Data sourced from CSV/XLSX files
* City is resolved from pincode
* Supports multilingual responses using Google Translate API

### 🔹 NGO Connect Feature

* After language selection, users are asked:

  > “Would you like to connect with an NGO for further help?”
* If selected:

  * User is informed that an NGO representative will contact them shortly
  * NGO partner later reaches out to the end user
* Supported in **all languages**

### 🔹 Analytics & Continuous Learning

* All interactions are logged in **Cloud SQL**
* Logged data is used to:

  * Identify trends
  * Understand user interests
  * Improve conversation design
  * Retrain and enhance AI flows

> *(Instead of “live analytics”, the system focuses on trend-based learning and optimization.)*

---

## 🧱 Architecture Overview

```
End User (Phone Call)
        ↓
Phone Gateway
        ↓
Dialogflow CX Agent
        ├── DTMF Flow
        ├── AI Conversational Flow
        ├── Scam Reporting Flow
        ├── NGO Connect Flow
        ↓
Cloud Functions (Node.js)
        ↓
Cloud SQL (MySQL)
        ↓
Trend Analysis & AI Training
```

---

## 📂 Repository Structure

```
.
├── cloud-functions/
│   ├── lookupBranches.js
│   ├── logScamReport.js
│   ├── utils/
│   │   ├── loadCSV.js
│   │   ├── findCityByPincode.js
│   │   └── bankalias.js
│   ├── translate.js
│   └── package.json
│
├── bank-data/
│   ├── SBI.xlsx
│   ├── HDFC.csv
│   └── ...
│
├── dialogflow-cx-export/
│   ├── agent.json
│   ├── flows/
│   ├── playbooks/
│   └── intents/
│
├── pincode-to-city.csv
├── README.md
└── .gitignore
```

---

## 🛠 Tech Stack

* **Dialogflow CX** – Conversational AI & voice flows
* **Google Cloud Functions** – Backend logic (Node.js 18)
* **Cloud SQL (MySQL)** – Persistent logging & analytics
* **Google Translate API** – Multilingual responses
* **Phone Gateway / Telephony Provider**
* **CSV / XLSX Data Sources**

---

## ⚙️ Cloud Function Deployment

### Deploy Function

```bash
gcloud functions deploy lookupBranches \
  --runtime nodejs18 \
  --entry-point lookupBranches \
  --trigger-http \
  --memory 512MB \
  --region asia-south1 \
  --allow-unauthenticated
```

### Redeploy from ZIP

```bash
unzip function.zip
cd function-folder
gcloud functions deploy lookupBranches --region asia-south1
```

---

## 🧩 Dialogflow CX Integration

* Webhooks connected using **OpenAPI tools**
* Session parameters passed:

  * `bank_name`
  * `pincode`
  * `user_language`
  * `scam_type`
  * `risk_level`
* Exported agent can be imported directly into Dialogflow CX

---

## 🌍 Supported Languages

* English (`en`)
* Hindi (`hi`)
* Marathi (`mr`)
* German (`de`)

Language detection and responses are handled dynamically via session parameters.

---

## 🤝 NGO Partner Integration (Future-Ready)

* Pluggable NGO partner system
* Supports multiple NGOs
* Can be extended to:

  * SMS follow-ups
  * Ticketing systems
  * CRM integrations

---

## 🔐 Security & Access

* Cloud Functions secured via IAM
* No hardcoded secrets (recommended: Secret Manager)
* Database access restricted via VPC / authorized networks

---


