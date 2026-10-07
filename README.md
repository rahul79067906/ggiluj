# ProofCarry AI — Proof-Carrying Data Analyst

> **"Every answer comes with proof."**

ProofCarry AI is an AI data analyst web application built for the **HackNex 2026** problem statement. It answers questions about messy, real-world data across multiple tables and documents by generating independently executable Python code, verifying calculations inside an AST-secured sandbox runtime, and actively refusing to answer unanswerable or trap questions.

---

## Key Features

1. **Proof-Carrying Verification Engine**:
   - Every numerical answer is backed by clean, reproducible Python Pandas code.
   - Independent sandbox execution validates that the code produces the exact calculation.
   - "Run verification again" button re-executes the code live and reports millisecond runtime and stdout.
2. **Never Trust Natural-Language AI Claims**:
   - If an AI claims one number (e.g., $1,250,000) but code execution returns another ($1,245,000), the discrepancy is caught and the verified result takes absolute precedence.
3. **Active Refusal of Unanswerable Questions**:
   - Never hallucinates missing dimensions. If a user asks for "Revenue in Europe" but the table only contains India, USA, and Japan, the system returns a visually prominent **CANNOT DETERMINE RELIABLY** card detailing why it stopped and recommending next steps.
4. **Data Quality Trap Detection**:
   - **Currency Mismatch**: Refuses to sum mixed currencies (USD, EUR, INR) without conversion rates.
   - **Missing Values**: Identifies null counts and adjusts calculations accordingly.
   - **Duplicate Records**: Detects repeated rows that could distort aggregates.
   - **Ambiguous Dates**: Flags ambiguous formats (e.g. `12/05/2025` where day and month are ambiguous).
5. **Multi-Table Join Support**:
   - Cross-references identifiers (e.g. `Customer_ID`, `Product_ID`) across loaded tables.
6. **HackNex 2026 Guided Demo Tour**:
   - Interactive 3-step walkthrough designed for evaluators:
     - **Step 1**: Basic numerical aggregation ("What is the total revenue?")
     - **Step 2**: Categorical ranking with audit trail ("Which product generated the highest revenue?")
     - **Step 3**: Trap refusal ("What was our revenue in Europe?")
7. **AST-Level Security Sandbox**:
   - Static analysis blocks `os`, `sys`, `subprocess`, `eval`, `open`, network requests, and mutating operations.

---

## Tech Stack

- **Frontend**: React 19, TypeScript, Vite, Tailwind CSS v4, Lucide Icons
- **Backend**: Python 3.12, FastAPI, Uvicorn, Pandas, OpenPyXL, Pydantic
- **Security & Execution**: AST-based static analysis sandbox, isolated Python execution scope
- **Database**: SQLite persistent audit history and dataset registry
- **Dual AI Engine**: Built-in semantic parser + optional Google Gemini 2.5 Flash

---

## Getting Started

### Prerequisites

- **Python 3.12+**
- **Node.js 20+** and **npm**

---

### Backend Setup

1. Open a terminal in `backend/`:
   ```bash
   cd backend
   ```
2. Create and activate a virtual environment:
   ```bash
   # Windows (PowerShell)
   python -m venv .venv
   .venv\Scripts\Activate.ps1
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. (Optional) Configure Gemini API Key:
   ```bash
   cp ../.env.example .env
   # Add your GEMINI_API_KEY if desired
   ```
5. Run the backend server:
   ```bash
   uvicorn main:app --host 127.0.0.1 --port 8000 --reload
   ```
   The backend API will be available at `http://127.0.0.1:8000`.

---

### Frontend Setup

1. Open a second terminal in `frontend/`:
   ```bash
   cd frontend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the Vite development server:
   ```bash
   npm run dev
   ```
   Open your browser at `http://localhost:5173`.

---

## Running Automated Tests

To run the backend test suite:
```bash
cd backend
.venv\Scripts\python -m pytest tests
```
Verified test coverage includes:
- AST safety policy enforcement (blocks `os`, `subprocess`, `eval`, `open`, allows `pandas`, `numpy`)
- Data quality profiling on messy CSV files
- Discrepancy detection between AI claim and verified code
- Refusal of unanswerable region queries
- Currency trap interception
- Pandas code synthesis and execution

---

## Live Demonstration Guide

1. Click **"Load Demo Data"** on the Dashboard or Data tab to load `demo_sales.csv`.
2. Click **"Launch Demo"** in the top navigation bar to open the 3-question guided walkthrough.
3. Test **Question 1**: *"What is the total revenue?"*
   - Observe real-time execution in the Sandbox Execution Terminal.
   - Inspect the verified numerical answer and execution checklist.
   - Click **"Run verification again"** to demonstrate live re-execution.
4. Test **Question 2**: *"Which product generated the highest revenue?"*
   - Observe grouped calculation (`MacBook Pro`) and the filtered row evidence preview.
5. Test **Question 3**: *"What was our revenue in Europe?"*
   - Observe active refusal: **CANNOT DETERMINE RELIABLY**.
   - Review the "Why we stopped" breakdown and recommendation.
6. Test **Currency Trap**: Click *"Can you calculate total revenue across all currencies?"*
   - Observe refusal due to currency dimensional mismatch without conversion rates.
