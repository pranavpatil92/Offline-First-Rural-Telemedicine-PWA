# Aether

### Offline-First Rural Telemedicine PWA

> **Healthcare continuity without continuous connectivity.**

Aether is an **offline-first rural telemedicine Progressive Web Application (PWA)** designed to support frontline healthcare workers operating in areas with intermittent or unavailable internet connectivity.

The platform enables healthcare workers to **capture patient information, perform assessments, securely store records locally, synchronize data when connectivity returns, receive doctor feedback, manage referrals, and complete follow-ups** without depending on continuous internet connectivity.

---

## 🚨 Problem

Frontline healthcare workers such as **ASHA, ANM, and CHW workers** often operate in rural areas where internet connectivity can be weak, intermittent, or completely unavailable.

Conventional cloud-dependent healthcare applications can become unreliable in these environments because:

* Patient information cannot reliably reach doctors in real time.
* Cloud-dependent workflows may stop working when the internet is unavailable.
* Synchronization can result in stale or conflicting clinical information.
* Sensitive patient information may remain exposed on field devices.
* Referral workflows can break because doctor feedback does not reliably reach the frontline worker.

The core problem is therefore not simply the lack of telemedicine.

> **It is the lack of reliable care continuity when connectivity is unavailable.**

---

# 💡 Solution

Aether uses an **offline-first architecture** that allows frontline healthcare workers to continue working even when there is no internet connection.

### Core workflow

```text
Capture
   ↓
Assess
   ↓
Secure
   ↓
Sync
   ↓
Doctor Review
   ↓
Priority Referral
   ↓
Follow-up
   ↓
Feedback
```

When offline, patient information is stored locally on the device.

When connectivity returns, the system synchronizes pending records with the backend.

---

# 🏗️ System Architecture

```text
                    ┌─────────────────────────┐
                    │      Aether PWA         │
                    │ React + TypeScript      │
                    │ Vite + Tailwind         │
                    └────────────┬────────────┘
                                 │
                 ┌───────────────┴───────────────┐
                 │                               │
           OFFLINE MODE                    ONLINE MODE
                 │                               │
                 ▼                               ▼
      ┌────────────────────┐          ┌────────────────────┐
      │ IndexedDB + Dexie   │          │     Supabase       │
      │                    │          │                    │
      │ Patients           │          │ PostgreSQL         │
      │ Assessments        │          │ Authentication     │
      │ Referrals          │          │ Database APIs      │
      │ Follow-ups         │          │                    │
      │ Feedback           │          └─────────┬──────────┘
      │ Sync Queue         │                    │
      │ Conflicts          │                    │
      └──────────┬─────────┘                    │
                 │                              │
                 └──────────► Sync Engine ◄─────┘
                              │
                    Retry / Backoff
                    Conflict Detection
                    Bidirectional Sync
                              │
                              ▼
                       Doctor Review
                              │
                              ▼
                        Referral
                              │
                              ▼
                         Follow-up
                              │
                              ▼
                          Feedback
```

---

# ✨ Key Features

## 📱 Offline-First PWA

Healthcare workers can continue core workflows even when the device has no internet connectivity.

* Offline application availability
* Local patient data persistence
* Offline patient registration
* Offline assessments
* Network status detection
* Automatic synchronization when connectivity returns

---

## 💾 Local Data Storage

Aether uses:

* **IndexedDB**
* **Dexie.js**

Patient records and operational data can be persisted locally before synchronization with the cloud backend.

---

## 🔄 Smart Synchronization

A persistent synchronization queue handles records created while offline.

The synchronization system supports:

* Pending records
* Batch synchronization
* Retry and exponential backoff
* Synchronization status tracking
* Conflict detection
* Bidirectional synchronization

### Sync flow

```text
Offline Record
     ↓
IndexedDB
     ↓
Sync Queue
     ↓
Internet Returns
     ↓
Sync Engine
     ↓
Supabase
     ↓
PostgreSQL
```

---

## ⚠️ Healthcare-Aware Conflict Detection

Aether does not blindly rely on simple last-write-wins behavior for clinically significant data.

Example:

```text
Device A
SpO₂ = 89

Device B
SpO₂ = 95

       ↓

CONFLICT DETECTED
       ↓
Clinically Significant?
       ↓
YES
       ↓
Doctor Review
```

Non-significant conflicts can be handled through appropriate merge/resolution logic.

---

## 🧠 AI-Assisted Clinical Support

Aether includes an AI assistance layer using **Google AI Studio / Gemini**.

The AI can provide:

* Structured patient summaries
* Risk indicators
* Advisory observations
* Priority assistance

The system follows a human-in-the-loop model:

```text
Patient Data
     ↓
Clinical Rules + AI Assistance
     ↓
Risk / Priority
     ↓
Doctor Review
     ↓
Final Clinical Decision
```

> **AI is advisory. Clinical decisions remain with the clinician.**

AI is not intended to function as an autonomous diagnostic system.

---

## 🛡️ Local Data Security

Sensitive locally stored health information is protected using:

* Web Crypto API
* AES-GCM encryption

The application is designed around secure handling of sensitive patient information on field devices.

---

# 👥 User Roles

## 👩‍⚕️ Health Worker — ASHA / ANM / CHW

The frontline worker can:

* Register patients
* Capture symptoms
* Record medical history
* Record vital signs
* Perform local assessment
* View risk indicators
* Continue working offline
* View synchronization status
* Receive doctor feedback
* Track referrals
* Manage follow-ups

---

## 👨‍⚕️ PHC / Remote Doctor

The doctor can:

* View incoming cases
* Review patient information
* Review medical history
* Review symptoms and vitals
* Review clinical rule findings
* Review AI assistance
* Resolve clinically significant conflicts
* Make the final clinical decision
* Create priority referrals
* Provide feedback
* Manage follow-up decisions

---

## 🏥 Referral Workflow

The system supports a closed-loop referral process:

```text
Health Worker
      ↓
Doctor Review
      ↓
Priority Referral
      ↓
Referral Hospital
      ↓
Feedback
      ↓
Health Worker
      ↓
Follow-up
```

This prevents the referral workflow from becoming a one-way process.

---

## 🏛️ Health Administration

The administration interface provides system-level visibility into metrics such as:

* Patient registrations
* Pending reviews
* Referrals
* Follow-ups
* Synchronization activity
* Conflict activity

### Proposed measurable outcomes

* Offline Capture Rate
* Sync Success Rate
* Encounter Completion Time
* Data Completeness
* Referral Follow-up Rate
* Conflict Resolution Rate

> These are proposed metrics to be measured, not claimed achieved results.

---

# 🧰 Tech Stack

### Frontend

* React
* TypeScript
* Vite
* Tailwind CSS
* shadcn/ui

### Progressive Web App

* Service Worker
* Workbox

### Offline Storage

* IndexedDB
* Dexie.js

### Backend

* Supabase

### Database

* PostgreSQL

### Authentication

* Supabase Auth

### Security

* Web Crypto API
* AES-GCM

### AI

* Google AI Studio / Gemini

### Clinical Safety

* Deterministic Rule Engine
* AI Assistance
* Human Clinical Review

### Development

* Antigravity CLI for debugging, optimization, refactoring, and development assistance

---

# 🔐 Architecture Principles

Aether is built around five major principles:

### 1. Offline-First

The frontline workflow should not depend on continuous connectivity.

### 2. Conflict-Aware

Clinical data conflicts should be detected rather than blindly overwritten.

### 3. Safety-First

Deterministic clinical rules provide a safety layer around AI assistance.

### 4. Human-in-the-Loop

AI assists clinicians; it does not replace clinical decision-making.

### 5. Closed-Loop Care

Referral and follow-up information must return to the frontline healthcare worker.

---

# 📊 Proposed Impact

Aether aims to support:

### 🏥 Care Continuity

Digital patient care can continue despite connectivity loss.

### 👩‍⚕️ Worker Efficiency

Reduced dependence on paper records, duplicate entry, and manual synchronization.

### 📡 Connectivity Resilience

Support for intermittent and low-bandwidth environments.

### 🚑 Referral Coordination

Priority-based referral with doctor feedback and follow-up.

### 🔐 Patient Privacy

Protection of sensitive offline health records.

### 🧠 Clinical Safety

Local red-flag detection combined with human-reviewed AI assistance.

---

# 🚀 Getting Started

## Prerequisites

* Node.js
* npm
* A Supabase project
* Google AI Studio / Gemini API credentials

## Installation

```bash
git clone <your-repository-url>

cd aether

npm install
```

## Environment Variables

Create a `.env` file:

```env
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_supabase_anon_key
VITE_GEMINI_API_KEY=your_gemini_api_key
```

Never commit API keys or other secrets to GitHub.

## Run Development Server

```bash
npm run dev
```

The application will be available at:

```text
http://localhost:5173
```

## Production Build

```bash
npm run build
```

---

# 🧪 Core Demonstration

The main prototype demonstration is:

```text
1. Health Worker Login
          ↓
2. Register Patient
          ↓
3. Record Symptoms + History + Vitals
          ↓
4. Turn Internet OFF
          ↓
5. Continue Working Offline
          ↓
6. Data Saved Locally
          ↓
7. Record Added to Sync Queue
          ↓
8. Internet Returns
          ↓
9. Smart Synchronization
          ↓
10. Doctor Receives Case
          ↓
11. Doctor Reviews Case
          ↓
12. Doctor Makes Decision
          ↓
13. Priority Referral
          ↓
14. Follow-up
          ↓
15. Feedback Reaches Health Worker
```

---

# ⚠️ Risk & Mitigation

| Risk                           | Mitigation                                 |
| ------------------------------ | ------------------------------------------ |
| Connectivity instability       | Offline-first architecture                 |
| Sync conflicts                 | Conflict detection                         |
| Local device data exposure     | AES-GCM encryption                         |
| AI false positives / negatives | Deterministic safety rules + doctor review |
| Automation bias                | AI remains advisory                        |
| Sync failures                  | Retry / exponential backoff                |

---

# 🔮 Future Scope

Potential future expansion includes:

* Additional language support
* Voice-based frontline interaction
* Broader referral workflows
* Expanded follow-up workflows
* Scaling to additional low-connectivity healthcare environments

---

# 📚 References

The project concept and architecture are informed by:

1. Ayushman Bharat Digital Mission — Government of India
2. ICMR — Ethical Guidelines for AI in Biomedical Research & Healthcare, 2023
3. WHO — Ethics and Governance of AI for Health
4. Nayak et al. — ASHA Assist India, 2026
5. Pisipati et al. — Offline and Cross-Platform Healthcare PWA, 2025
6. Josephe et al. — PWAs for Low/No Connectivity Areas, IEEE, 2023
7. MDN Web Platform Documentation

---

# 🏆 Hackathon

**Project:** Aether
**Category:** Offline-First Rural Telemedicine
**Core Theme:** Healthcare continuity without continuous connectivity

---

## Core Message

> **Capture offline. Assess locally. Secure data. Synchronize intelligently. Enable clinician review. Close the referral loop.**
