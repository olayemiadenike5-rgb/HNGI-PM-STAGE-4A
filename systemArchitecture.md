# System Architecture – Finora

##  Overview
Finora is a smart money management system designed to provide users with real-time budgeting insights.  

## Architecture Layers

### 1. Frontend
- Framework: React.js  
- Styling: Tailwind CSS  
- Purpose: Provides a clean, intuitive dashboard for users to view balances, expenses, and insights.  
 Key Modules:
  - Dashboard: Displays income, spending, and goal progress.
  - Budget Planner: Lets users create and prioritize expenses.
  - Authentication Page: Handles login, signup, and password reset.

Data Flow:
Frontend communicates with the backend via RESTful API calls (HTTPS) to fetch and update data.

### 2. Backend
- Framework: Node.js with Express  
- Purpose: Acts as the main logic engine and handles data processing, authentication, and communication with external APIs (like Plaid for bank integration).  
- Key Responsibilities;
  - User authentication 
  - Transaction categorization
  - Expense prioritization algorithm
  - Budget calculation and Spend tracking
  - API integration with financial data sources



### 3. Database
- Technology: PostgreSQL (Relational SQL Db)  
- Purpose: Stores user profiles, transactions, categorized expenses, and goal data in relational tables to ensure data integrity, scalability, and consistency across financial records.

Core Tables	
users - 	Stores user credentials, preferences, and profile details.
transactions - Logs all income and expense transactions linked to users.
expense_categories - Defines, store  and manages categories and priorities for expenses. 
budgets -	Tracks monthly budgets per user, including income and allocated spending.



Tables Schema (simplified):

<pre> sql -- USERS CREATE TABLE users ( id SERIAL PRIMARY KEY, name VARCHAR(100), email VARCHAR(100) UNIQUE NOT NULL, password_hash TEXT NOT NULL, email_verified BOOLEAN DEFAULT FALSE, monthly_recurring_intake NUMERIC(12, 2), current_balance NUMERIC(12, 2), created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ); -- BUDGETS CREATE TABLE budgets ( id SERIAL PRIMARY KEY, user_id INT REFERENCES users(id) ON DELETE CASCADE, month DATE NOT NULL, target_monthly_leftover_income NUMERIC(12, 2) NOT NULL, created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ); -- EXPENSE CATEGORIES CREATE TABLE expense_categories ( id SERIAL PRIMARY KEY, name VARCHAR(100) NOT NULL, cost NUMERIC(12, 2) NOT NULL, priority VARCHAR(20) CHECK (priority IN ('High', 'Medium', 'Low')) ); -- TRANSACTIONS CREATE TABLE transactions ( id SERIAL PRIMARY KEY, user_id INT REFERENCES users(id) ON DELETE CASCADE, category_id INT REFERENCES expense_categories(id), amount NUMERIC(12, 2) NOT NULL, type VARCHAR(20) CHECK (type IN ('income', 'expense')), created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP );  </pre>

Communication flow
[User Interface] = [REST API] = [Backend Logic] = [Database]
                         
   (User Input)          (Data Processing)

Technical Feasibility
-Scalability: Node.js and Postgres support large datasets and real-time updates.
-Security: Uses HTTPS for secure connections. Sensitive data (like tokens) stored securely.
-Maintainability: Modular structure allows easy feature additions (e.g., AI insights in future).
-Performance: Asynchronous APIs (Node.js) enable fast response times and reduced load.
