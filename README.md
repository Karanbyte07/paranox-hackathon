PARANOX 2.0 - Intelligent Expense Management Dashboard
Paranox 2.0 is a modern, single-page web application designed for intuitive expense tracking, submission, and management. It provides a clean, data-rich dashboard for users to monitor spending and a streamlined workflow for managers to approve expenses, all powered by an intelligent backend designed to automatically flag non-compliant submissions.

This project is currently a fully interactive frontend prototype built with HTML, Tailwind CSS, and JavaScript.

Features (Frontend)
Single-Page Application (SPA): A seamless user experience with no page reloads. All content is dynamically rendered within a single index.html file.

Interactive Dashboard: Features data visualizations, including charts for monthly spending patterns and expense distribution, powered by Chart.js.

Full Expense Workflow: Includes mock pages for submitting new expenses, viewing an approval queue, managing company policies, and analyzing spending trends.

Responsive Design: A clean, modern UI that works across all screen sizes, built with Tailwind CSS.

Authentication Screens: Dedicated UI for user login and registration.

How to Run
This is a self-contained frontend application. No build process or server is required to run it.

Download the Code: Make sure you have the index.html file.

Open in Browser: Simply open the index.html file in any modern web browser (like Chrome, Firefox, or Edge).

That's it! You can navigate through the entire application from this single file.

Backend & Machine Learning Architecture (Proposed)
While the current version is a frontend prototype, it is designed to connect to a powerful backend. The core of this backend is a machine learning model that provides real-time compliance feedback.

Proposed Tech Stack
API Framework: Node.js with Express.js for building a fast and scalable REST API.

Database: PostgreSQL for structured relational data (users, expenses, policies).

Authentication: JWT (JSON Web Tokens) for secure, stateless user authentication.

ML Model Hosting: A dedicated Python service using a framework like FastAPI or Flask, hosted as a serverless function (e.g., AWS Lambda, Google Cloud Run) for scalability.

Receipt Processing: An OCR (Optical Character Recognition) service like Google Cloud Vision API to extract text and data from uploaded receipt images.

How the Backend Logic Will Work
Expense Submission: A user fills out the expense form and uploads a receipt image. The frontend sends this data to the backend API (/api/expenses/submit).

OCR Processing: The backend receives the image and sends it to the OCR service. The service scans the image and returns structured text data, identifying the vendor, date, line items, and total amount.

Data Enrichment: The backend enriches the OCR data. It cross-references the vendor name to assign a category (e.g., "Starbucks" -> "Food & Drink"), fetches the submitting employee's role, and pulls all relevant company policies (e.g., the spending limit for the "Food & Drink" category for that employee's role).

ML Model Inference: This complete, structured data "feature set" is sent to the machine learning model's API endpoint.

Compliance Check: The model analyzes the data and returns a prediction:

status: "Compliant" or "Flagged"

confidence_score: A percentage indicating the model's certainty.

reason_code: If flagged, a code explaining why (e.g., AMOUNT_OVER_LIMIT, UNUSUAL_VENDOR, POTENTIAL_DUPLICATE).

Database Storage: The expense details, along with the model's prediction and reason, are saved to the PostgreSQL database.

Response to Frontend: The backend sends the final result back to the frontend, which updates the UI to show the expense status (e.g., with a "Pending" or "Flagged" tag).

The Machine Learning Model: Anomaly Detection
The core intelligent feature is a model trained to detect anomalies and policy violations in expense submissions.

Model Objective: To classify each expense as either "Compliant" or "Flagged".

Model Type: A Gradient Boosting Classifier (like XGBoost or LightGBM) is an excellent choice. It's powerful, fast, and provides good explainability, which is crucial for understanding why an expense was flagged.

Training on Synthetic Data:
To make the model effective from day one (without waiting to collect thousands of real expenses), we will train it on a large, high-quality synthetic dataset.

The process for generating this data would be:

Define Core Rules: Codify the company's expense policies (e.g., "Software engineers can spend up to $50 on meals per day," "Flights must be booked in economy class").

Generate Compliant Data: Create thousands of "perfect" expense entries that clearly follow these rules. This forms the baseline of normal behavior.

Generate Anomalies (Non-Compliant Data): Intelligently create examples of expenses that should be flagged. This is the most critical step and includes:

Boundary Violations: Expenses just slightly over the limit (e.g., $51 for a $50 meal limit).

Categorical Mismatches: Expenses from unusual vendors for a specific employee role (e.g., a marketing manager expensing a server rack).

Temporal Anomalies: Expenses at odd times (e.g., a large office supply purchase at 2 AM on a Saturday).

Potential Duplicates: Entries with very similar amounts and vendors submitted close together.

Unusual Frequencies: An employee suddenly submitting meal expenses three times a day, every day.

By training on this rich, synthetic dataset, the model learns to identify not just obvious rule-breaking but also subtle patterns that suggest a non-compliant expense, making the Paranox 2.0 platform truly intelligent.
