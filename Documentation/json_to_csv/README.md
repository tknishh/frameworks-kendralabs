# Solidatus JSON to Neo4j CSV Converter: GHG Protocol

## Introduction

This guide provides step-by-step instructions for importing JSON files exported from Solidatus and converting them into CSV files that can be directly imported into Neo4j to create a graph database of the GHG (Greenhouse Gas) Protocol. The GHG Protocol is an internationally recognized standard for accounting and managing greenhouse gas emissions.

This converter tool is designed to simplify the process of transforming Solidatus JSON files into a format compatible with Neo4j, allowing you to efficiently create a graph database to analyze and visualize GHG emissions data.

## Prerequisites

Before starting the conversion process, ensure that you have the following prerequisites:

1. Solidatus JSON Files: Export the GHG Protocol data from Solidatus in JSON format.
2. Neo4j Database: Set up a Neo4j database to import the converted CSV files.

## Steps for Conversion

Follow these steps to convert Solidatus JSON files into Neo4j-compatible CSV files:

### Step 1: Install Required Dependencies

To perform the conversion, you need to install the following dependencies:

1. Python: Ensure that Python is installed on your system.
2. Solidatus Library: Install the Solidatus library using the following command:

   ```
   pip install solidatus
   ```

3. Pandas Library: Install the Pandas library using the following command:

   ```
   pip install pandas
   ```

### Step 2: Prepare Solidatus JSON Files

Export the GHG Protocol data from Solidatus in JSON format. Make sure you have the necessary JSON files representing the entities and relationships of the GHG Protocol data.

### Step 3: Run the Conversion Script

1. Download the "Solidatus JSON to Neo4j CSV Converter" script from the repository or copy the script code provided below:

   ```
   import json
   import pandas as pd

   # Load Solidatus JSON data
   solidatus_json_file = 'path_to_solidatus_json_file.json'
   with open(solidatus_json_file, 'r') as file:
       solidatus_data = json.load(file)

   # Convert entities to CSV
   entities_df = pd.DataFrame(solidatus_data['entities'])
   entities_df.to_csv('entities.csv', index=False)

   # Convert relationships to CSV
   relationships_df = pd.DataFrame(solidatus_data['relationships'])
   relationships_df.to_csv('relationships.csv', index=False)

   ```

2. Modify the script to specify the correct file paths for the Solidatus JSON file and the desired output CSV file names.

3. Open a terminal or command prompt, navigate to the directory where the script is located, and execute the following command:

   ```
   python solidatus_json_to_csv_converter.py
   ```

   Make sure to replace `solidatus_json_to_csv_converter.py` with the actual filename if you renamed it.

4. The script will convert the Solidatus JSON files into CSV files named `entities.csv` and `relationships.csv`.

### Step 4: Import CSV Files into Neo4j

1. Start your Neo4j database server and access the Neo4j browser interface.

2. In the Neo4j browser, execute the following Cypher queries to import the CSV files:

   ```
   LOAD CSV WITH HEADERS FROM 'file:///entities.csv' AS row
   CREATE (n:Entity)
   SET n = row
   ```

   ```
   LOAD CSV WITH HEADERS FROM 'file:///relationships.csv' AS row
   MATCH (from:Entity {id: row.from_entity_id})
   MATCH (to:Entity {id: row.to_entity_id})
   CREATE (from)-[:RELATIONSHIP_TYPE]->(to)
   ```

   Replace `Entity` and `RELATIONSHIP

_TYPE` with the appropriate node labels and relationship types based on your GHG Protocol data.

3. Once the queries are executed, the data from the CSV files will be imported into your Neo4j graph database.

## Conclusion

By following this guide, you have successfully converted Solidatus JSON files into CSV files that are ready to be imported into Neo4j to create a graph database of the GHG Protocol. You can now leverage the power of Neo4j's graph database to analyze, query, and visualize your GHG emissions data efficiently.

