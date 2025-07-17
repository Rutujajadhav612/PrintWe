# PrintWe
PrithWe is a full-stack web application built on the PERN stack (PostgreSQL, Express, React, Node.js) designed to help households and businesses calculate their carbon footprints. This project was developed as a college project to promote environmental awareness and action.

Table of Contents
Introduction
Features
Technologies Used
Installation
Usage
Motivation
License
Introduction
PrithWe allows users to calculate their carbon footprint based on various parameters. It provides detailed insights and suggestions on how to reduce their carbon emissions. The name "PrithWe" is derived from "Prithvi" (Earth) and "We," emphasizing collective action and collaboration.
Features
User authentication and authorization
Carbon footprint calculator for households and businesses
Detailed reports and donut chart representation.
Responsive design for all devices
User-friendly interface
Technologies Used
Frontend: React.js, Tailwind CSS
Backend: Node.js, Express.js
Database: PostgreSQL
Hosting: Render
Version Control: Git, GitHub
Installation
Follow these steps to set up the project locally:

Clone the repository: 
git clone https://github.com/yourusername/prithwe.git

Navigate to the project directory:
cd prithwe

Install backend dependencies:
npm install

Install frontend dependencies:
cd ./client
npm install

Set up environment variables:
Create a .env file in the server directory and add your database details:
PG_USER=your_user_name
PG_HOST=your_host_name
PG_DATABASE=your_database_name
PG_PASSWORD=your_password
PG_PORT=5432
SESSION_SECRET=your_secret
MAIL=your_mail
APP_PASSWORD=your_app_password
ADMIN_MAIL=admin_mail
ADMIN_PASS=admin_password
GEMINI_API_KEY=your_api_key
OAUTH_CLIENT_ID=your_client_id
OAUTH_SECRET=your_secret

After that paste the below code and run it.
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    google_id VARCHAR(255),
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    type VARCHAR(50) NOT NULL,
    otp VARCHAR(20) DEFAULT '000000',
    otp_timestamp BIGINT,
    isVerified BOOLEAN DEFAULT false,
   created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE contact (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255),
    message TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE household_common (
    id SERIAL PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    electricity_usage DECIMAL,
    water_usage DECIMAL,
    waste_generation DECIMAL,
    gas_cylinder DECIMAL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE business_common (
    id SERIAL PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    electricity_usage DECIMAL,
    water_usage DECIMAL,
    paper_consumption DECIMAL,
    waste_generation DECIMAL,
    fuel_consumption DECIMAL,
    business_travel DECIMAL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE family_members (
    id SERIAL PRIMARY KEY,
    household_common_id INT REFERENCES household_common(id),
    name VARCHAR(255),
    private_vehicle DECIMAL,
    public_vehicle DECIMAL,
    air_travel DECIMAL,
    veg_meals DECIMAL,
    non_veg_meals DECIMAL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE recommendations (
    id SERIAL PRIMARY KEY,
    user_id UUID,
    household_common_id INT,
    business_common_id INT,
    recommendation TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (household_common_id) REFERENCES household_common(id),
    FOREIGN KEY (business_common_id) REFERENCES business_common(id)
);

You will see tables for the database are created automatically.

Start the backend server:
cd ./server
node index.js

Start the frontend development server:
cd ./client
npm run dev

Open your browser and navigate to http://localhost:5173.

Getting OAuth Client ID and Secret

Steps to follow -
Go to the Google Developers Console. Click Select a project ➝ New Project ➝ the Create button.
Enter your Project name ➝ click the Create button.
Click OAuth consent screen in the left side menu ➝ choose User Type ➝ click the Create button.
