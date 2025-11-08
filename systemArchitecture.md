# System Architecture – Finora

##  Overview
Finora is a cloud-based smart money management system designed to provide users with real-time budgeting insights.  
It follows a three tier architecture
-Frontend (User Interface) 
-Backend (API & Logic Layer) 
-Database (Persistent Storage).

## Architecture Layers

### 1. Frontend
- Framework: React.js  
- Styling: Tailwind CSS  
- Purpose: Provides a clean, intuitive dashboard for users to view balances, expenses, and insights.  
 Key Modules:
  - Dashboard: Displays income, spending, and goal progress.
  - Budget Planner: Lets users set and prioritize expenses.
  - Goals Page: Allows saving and budgeting goal tracking.
  - Authentication Page: Handles login, signup, and password reset.

Data Flow:
Frontend communicates with the backend via RESTful API calls (HTTPS) to fetch and update data.

### 2. Backend
- Framework: Node.js with Express  
- Purpose: Acts as the main logic engine and handles data processing, authentication, and communication with external APIs (like Plaid for bank integration).  
- Key Responsibilities;
  - User authentication (via Firebase Auth or OAuth 2.0)
  - Transaction categorization
  - Expense prioritization algorithm
  - Budget calculation and goal tracking
  - API integration with financial data sources

Endpoints Example:
-POST /api/register
-POST /api/login
-GET /api/expenses
-POST /api/goals
-GET /api/insights

### 3. Database
- Technology: MongoDB (NoSQL)  
- Purpose: Stores user profiles, transactions, categorized expenses, and goal data.  
- Sample Collections:
  - users– stores user credentials and preferences.
  - transactions– logs of income and expenses.
  - budgets– monthly budgets linked to users.
  - goals – saving or spending targets.

Sample Schema (simplified):
```json
{
  "userId": "12345",
  "month": "2025-11",
  "income": 150000,
  "expenses": [
    { "category": "Rent", "amount": 60000, "priority": "High" },
    { "category": "Food", "amount": 30000, "priority": "Medium" }
  ],
  "goals": [
    { "goalName": "Emergency Fund", "target": 50000, "progress": 25000 }
  ]
}

Communication flow
[User Interface] = [REST API] = [Backend Logic] = [Database]
                         
   (User Input)          (Data Processing)

Technical Feasibility
-Scalability: Node.js and MongoDB support large datasets and real-time updates.
-Security: Uses OAuth2.0 and HTTPS for secure connections. Sensitive data (like tokens) stored securely.
-Maintainability: Modular structure allows easy feature additions (e.g., AI insights in future).
-Performance: Asynchronous APIs (Node.js) enable fast response times and reduced load.