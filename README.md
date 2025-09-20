# OLAP
Digital Ad Campaign Tracker
📄 Project Overview
This is a web-based Performance Management System (PMS) designed to track and manage Digital Ad Campaigns. Built with Python, the application offers a user-friendly interface for setting goals, logging tasks, tracking progress, providing feedback, and generating business insights. The application uses a two-file architecture, separating the front-end user interface from the back-end database operations for clarity and maintainability.

✨ Core Features
Goal & Task Setting: Managers can create and assign goals to employees. Employees can then log tasks to work toward these goals.

Progress Tracking: The system allows both managers and employees to monitor the progress of goals and tasks. Managers have the exclusive ability to update a goal's status.

Feedback: A dedicated section for managers to provide written feedback on specific goals. An automated feedback system is implemented using database triggers.

Reporting: A comprehensive reporting section provides a historical view of an employee's performance, including all past and present goals and feedback.

Business Insights: The system provides actionable insights using aggregate functions like COUNT, SUM, AVERAGE, MIN, and MAX to analyze campaign performance.

💻 Technical Stack
Frontend: Streamlit

Backend: Python

Database: PostgreSQL

Database Connector: psycopg2

⚙️ Setup and Installation
1. Prerequisites
Python 3.x: Ensure Python is installed on your system.

PostgreSQL: Install and set up a PostgreSQL database instance.

2. Database Configuration
Before running the application, you must create a database and update the connection details in the backend_pms.py file.

Create a new database named pms_db.

Open backend_pms.py and replace "your_username" and "your_password" with your PostgreSQL credentials.

3. Install Dependencies
Open your terminal or command prompt and run the following command to install the required Python libraries:

Bash

pip install streamlit psycopg2-binary
4. Run the Application
With the database configured and dependencies installed, you can start the application from your terminal:

Bash

streamlit run frontend_pms.py
The application will open in your web browser.

📁 Application Structure
The application is split into two distinct files to promote clarity and maintainability:

frontend_pms.py: Handles the entire user interface and interaction logic using the Streamlit library. It calls the functions from the backend_pms.py file to perform all database-related tasks.

backend_pms.py: Manages all connections to the PostgreSQL database. It contains functions for CRUD (Create, Read, Update, Delete) operations, as well as functions for generating business insights.

📊 Database Schema
The following SQL queries are used to create the necessary tables for the application. The init_db() function in backend_pms.py will automatically run these if the tables do not exist.

SQL

-- Create the goals table
CREATE TABLE goals (
    goal_id SERIAL PRIMARY KEY,
    manager VARCHAR(255) NOT NULL,
    employee VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    due_date DATE NOT NULL,
    status VARCHAR(50) NOT NULL
);

-- Create the tasks table
CREATE TABLE tasks (
    task_id SERIAL PRIMARY KEY,
    goal_id INTEGER NOT NULL,
    description TEXT NOT NULL,
    due_date DATE NOT NULL,
    status VARCHAR(50) DEFAULT 'To Do',
    approval_status VARCHAR(50) DEFAULT 'Pending',
    FOREIGN KEY (goal_id) REFERENCES goals (goal_id) ON DELETE CASCADE
);

-- Create the feedback table
CREATE TABLE feedback (
    feedback_id SERIAL PRIMARY KEY,
    goal_id INTEGER NOT NULL,
    manager_name VARCHAR(255) NOT NULL,
    feedback_text TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (goal_id) REFERENCES goals (goal_id) ON DELETE CASCADE
);
🔒 CRUD & ACID Compliance Matrix
This matrix provides a comprehensive overview of how each major use case maintains data integrity and reliability by adhering to the principles of Atomicity, Consistency, Isolation, and Durability.

Use Case	Thread Operation	Tables	Atomicity	Consistency	Isolation	Durability
CREATE Goal	create_goal()	goals	✅ Yes	✅ Yes	✅ Yes	✅ Yes
CREATE Task	create_task()	tasks	✅ Yes	✅ Yes	✅ Yes	✅ Yes
CREATE Feedback	create_feedback()	feedback	✅ Yes	✅ Yes	✅ Yes	✅ Yes
READ Data	read_goals(), read_tasks(), read_all_feedback(), get_employee_performance()	goals, tasks, feedback	✅ N/A	✅ Yes	✅ Yes	✅ N/A
UPDATE Status	update_goal_status()	goals	✅ Yes	✅ Yes	✅ Yes	✅ Yes
DELETE Goal	delete_goal()	goals, tasks, feedback	✅ Yes
