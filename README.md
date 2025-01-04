# Medicine-Recommendation-system
# app.py
import mysql.connector

# Establish the connection to the MySQL server
connection = mysql.connector.connect(
    host="localhost",         # The host where your MySQL server is running (localhost for local machine)
    user="root",              # Your MySQL username (usually 'root' for local installations)
    password="",  
    database="medicine recommendation system"  # The name of your database
)

# Check if the connection is successful
if connection.is_connected():
    print("Connected to MySQL server successfully.")
    
    # Create a cursor object to interact with the database
    cursor = connection.cursor()
    
    # Example: Query the structure of a table (e.g., 'users' table)
    cursor.execute("SHOW TABLES;")  # Show all tables in the database
    tables = cursor.fetchall()
    
    print("Tables in the database:")
    for table in tables:
        print(table[0])  # Table names are in the first column of the result
    
    # Example: Query the 'users' table (replace with actual table name)
    cursor.execute("SELECT * FROM users_table LIMIT 5;")  # Adjust the table name as necessary
    rows = cursor.fetchall()
    
    # Print the first 5 rows of the 'users' table (or any table you query)
    print("First 5 rows from 'users_table' table:")
    for row in rows:
        print(row)
    
    # Close the cursor and connection
    cursor.close()
    connection.close()
else:
    print("Failed to connect to MySQL server.")

from flask import Flask, request, jsonify
from flask_cors import CORS

app = Flask(_name_)
CORS(app)  # Enable Cross-Origin Resource Sharing

# Medicine database
medicine_database = {
    "paracetamol": ["fever", "headache", "body pain"],
    "ibuprofen": ["inflammation", "pain", "fever"],
    "amoxicillin": ["infection", "bacterial infection"],
    "cetirizine": ["allergy", "runny nose", "sneezing", "cough"],
    "aspirin": ["headache", "fever", "pain"],
    "loratadine": ["allergy", "itching", "rash"],
    "metformin": ["diabetes"],
    "omeprazole": ["acid reflux", "stomach ulcer"],
}

# Function to recommend medicine based on symptoms
def recommend_medicine(symptoms):
    recommended_medicines = []
    for medicine, conditions in medicine_database.items():
        if any(symptom in conditions for symptom in symptoms):
            recommended_medicines.append(medicine)
    return recommended_medicines

@app.route("/recommend", methods=["POST"])
def recommend():
    data = request.get_json()
    symptoms = data.get("symptoms", "").lower().split(",")
    symptoms = [symptom.strip() for symptom in symptoms]

    if not symptoms:
        return jsonify({"error": "No symptoms provided."}), 400

    recommendations = recommend_medicine(symptoms)

    return jsonify({"medicines": recommendations})

if _name_ == "_main_":
    app.run(debug=True)
    
