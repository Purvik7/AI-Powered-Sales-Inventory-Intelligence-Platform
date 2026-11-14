# AI-Powered-Sales-Inventory-Intelligence-Platform

RetailPulse – AI-Powered Sales & Inventory Intelligence Platform

Turning retail data into smarter business decisions.

RetailPulse is an end-to-end retail analytics application that helps businesses understand their sales performance, identify top-performing products, analyse customer activity and estimate future product demand using machine learning.

I built this project to bring together data analytics, machine learning and software development in one practical application. Instead of creating a standalone ML model, the goal is to build a complete workflow — from processing raw retail data to presenting useful business insights through a web dashboard.

Project Overview

Retail businesses generate large amounts of transaction data every day. However, understanding sales trends, identifying important products and estimating future demand can be challenging without proper analysis.

RetailPulse addresses this problem by combining Python-based data processing, SQL analytics, machine learning and a web-based dashboard.

The application currently supports:

- Sales and revenue analysis
- Product performance analysis
- Daily sales trends
- Customer and transaction summaries
- Data cleaning and preprocessing
- Machine learning-based demand forecasting
- Sales-derived inventory alerts
- Interactive dashboard and REST APIs

Features

1. Sales Analytics

- View total revenue and transaction counts.
- Analyse daily sales performance.
- Identify sales trends over time.
- Understand product-level revenue contribution.

2. Product Performance

- Identify top-selling products.
- Compare products based on revenue and units sold.
- Analyse individual product performance.

3. Demand Forecasting

- Uses historical product sales to estimate future demand.
- Creates time-based features such as previous sales and rolling averages.
- Uses a Random Forest Regression model.
- Evaluates predictions using MAE and RMSE.

4. Inventory Insights

- Identifies products with relatively low historical sales activity.
- Provides basic sales-derived inventory alerts.
- Establishes a foundation for future stock-level and reorder management.

Note: The current UCI dataset contains sales transactions, not actual warehouse stock records. Therefore, the current alerts are based on sales activity and should not be interpreted as real-time stock monitoring.

5. Web Dashboard

- Displays business metrics in one place.
- Presents sales and product analytics.
- Connects the frontend with backend REST APIs.
- Provides an interface for accessing forecasting results.

Technology Stack

Area| Technologies
Programming| Python
Data Processing| Pandas, NumPy
Machine Learning| Scikit-learn
Model| Random Forest Regressor
Backend| FastAPI, Pydantic
Database| SQLite, PostgreSQL
Frontend| React, Vite
Data Analysis| SQL
Testing| Pytest
Containerization| Docker, Docker Compose
Version Control| Git, GitHub

System Architecture

              UCI ONLINE RETAIL DATASET
                         |
                         v
                DATA PREPROCESSING
                   Python / Pandas
                         |
                         v
                    DATABASE
                SQLite / PostgreSQL
                         |
              +----------+----------+
              |                     |
              v                     v
        SQL ANALYTICS          ML PIPELINE
              |                     |
              |              Random Forest
              |                     |
              |              Demand Forecast
              |                     |
              +----------+----------+
                         |
                         v
                    FASTAPI
                   REST APIs
                         |
                         v
                     REACT
                    DASHBOARD
                         |
                         v
                BUSINESS INSIGHTS

Dataset

This project uses the UCI Online Retail Dataset.

Source: https://archive.ics.uci.edu/dataset/352/online+retail

The dataset contains 541,909 transaction records from a UK-based online retailer, covering December 2010 to December 2011.

Important columns include:

- InvoiceNo – Transaction invoice number
- StockCode – Product identifier
- Description – Product description
- Quantity – Number of units purchased
- InvoiceDate – Transaction date and time
- UnitPrice – Price per unit
- CustomerID – Customer identifier
- Country – Customer country

Data Preprocessing

The raw dataset is cleaned before being stored in the database.

The preprocessing workflow includes:

1. Removing cancellation invoices.
2. Handling missing required values.
3. Removing invalid quantities and prices.
4. Converting date and numeric columns into appropriate formats.
5. Calculating revenue using quantity multiplied by unit price.
6. Loading the cleaned records into the database.

Dataset attribution: UCI Machine Learning Repository, Online Retail Dataset. Licensed under CC BY 4.0.

Machine Learning Approach

RetailPulse uses a Random Forest Regressor for product-level demand forecasting.

Feature Engineering

The model uses historical sales information to create features such as:

- Day of the week
- Month
- Previous day's sales (lag 1)
- Sales from seven days earlier (lag 7)
- Previous seven-day average sales

Model Evaluation

The model is evaluated using a chronological train/test split to avoid using future information when predicting past observations.

Evaluation metrics:

MAE – Mean Absolute Error

Measures the average absolute difference between actual and predicted demand.

RMSE – Root Mean Squared Error

Penalizes larger prediction errors more heavily.

Actual evaluation results should be generated by running the training script. No accuracy or performance figure is assumed in advance.

Database

The initial application uses a simple transaction-based database structure.

Transactions Table

Column| Description
id| Unique record identifier
invoice_no| Invoice number
stock_code| Product code
description| Product name
quantity| Units purchased
invoice_date| Purchase date
unit_price| Price per unit
customer_id| Customer identifier
country| Customer country
revenue| Calculated transaction revenue

SQLite is used for straightforward local development, while PostgreSQL is supported through Docker Compose.

API Endpoints

The backend is developed using FastAPI.

Method| Endpoint| Purpose
GET| "/"| API welcome message
GET| "/health"| Health check
GET| "/analytics/summary"| Overall sales summary
GET| "/analytics/top-products"| Top-performing products
GET| "/analytics/daily-sales"| Daily sales analysis
GET| "/inventory/alerts"| Sales-derived inventory alerts
POST| "/forecast"| Product demand forecasting

Interactive API documentation is available through FastAPI Swagger UI.

Project Structure

RetailPulse/
│
├── backend/
│   ├── main.py
│   ├── db.py
│   ├── models.py
│   ├── schemas.py
│   └── requirements.txt
│
├── data/
│   └── Online Retail.xlsx
│
├── ml/
│   └── train.py
│
├── scripts/
│   └── prepare_data.py
│
├── frontend/
│   ├── index.html
│   ├── package.json
│   ├── vite.config.js
│   └── src/
│       ├── main.jsx
│       └── style.css
│
├── sql/
│   └── analytics.sql
│
├── tests/
│   └── test_api.py
│
├── docs/
│   └── interview_notes.md
│
├── docker-compose.yml
├── .gitignore
└── README.md

Getting Started

Prerequisites

Install the following:

- Python 3.11
- Node.js and npm
- Git
- Docker Desktop (optional, for containerized setup)

1. Clone the Repository

git clone https://github.com/YOUR_USERNAME/RetailPulse.git
cd RetailPulse

Replace "YOUR_USERNAME" with your GitHub username.

2. Set Up Python Environment

python -m venv .venv

Activate the environment.

Windows:

.venv\Scripts\activate

Linux/macOS:

source .venv/bin/activate

3. Install Backend Dependencies

pip install -r backend/requirements.txt

4. Download the Dataset

Download the UCI Online Retail dataset:

https://archive.ics.uci.edu/dataset/352/online+retail

Place the downloaded Excel file inside the "data" folder and name it:

Online Retail.xlsx

5. Prepare the Dataset

From the project root:

python scripts/prepare_data.py

This script cleans the dataset and loads the processed transactions into the configured database.

6. Train the Machine Learning Model

python ml/train.py

The script trains the forecasting model and saves the trained model for use by the API.

7. Start the Backend

uvicorn backend.main:app --reload

Backend URL:

http://127.0.0.1:8000

API documentation:

http://127.0.0.1:8000/docs

8. Start the Frontend

Open another terminal:

cd frontend
npm install
npm run dev

Open the local URL displayed in the terminal, usually:

http://localhost:5173

9. Run Tests

From the project root:

pytest -q

10. Run Using Docker

Make sure the dataset is available in the "data" folder.

Run:

docker compose up --build

The Docker setup starts PostgreSQL, the FastAPI backend and the React development server.

Future Improvements

There are several features I would like to add as the project develops:

- Actual stock-level tracking and inventory updates.
- Product and supplier management.
- Purchase order management.
- Automated reorder recommendations.
- Improved demand forecasting models.
- Customer segmentation and purchasing behaviour analysis.
- User authentication and role-based access.
- Cloud deployment.
- Automated CI/CD using GitHub Actions.
- Model monitoring and periodic retraining.

What I Learned

Working on RetailPulse provides practical experience in:

- Processing and analysing real-world datasets.
- Writing SQL queries for business analysis.
- Designing and connecting a relational database.
- Developing REST APIs using FastAPI.
- Building and evaluating machine learning models.
- Integrating a React frontend with a Python backend.
- Using Docker to run application services.
- Writing basic automated tests.
- Organising and documenting a complete software project.

Author

Purvik H

B.E. – Artificial Intelligence & Machine Learning

- GitHub: https://github.com/Purvik7
- LinkedIn: https://linkedin.com/in/purvik-h-007b27245
- Email: hpurvik7@gmail.com

---

RetailPulse is a learning and portfolio project developed to explore how data analytics, machine learning and software engineering can be combined to solve practical retail business problems.