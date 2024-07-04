Knowledge Graph for Books
This project creates and visualizes a knowledge graph for books based on a CSV file containing information about book titles, authors, publication years, and publishers.

Prerequisites
Python 3.x
pandas
networkx
matplotlib
You can install the required packages using:
pip install pandas networkx matplotlib

                                                                    Script Details
Overview
The script provides three main functionalities:

Create and visualize the entire knowledge graph:

It creates a directed graph with relationships between book titles, authors, publication years, and publishers.
Create and visualize a knowledge graph for selected entities:

Users can select two entities (e.g., title and author) to create and visualize a knowledge graph for their relationships.
Create and visualize a knowledge graph for a specific example:

Users can select a category (e.g., author) and a specific example (e.g., a particular author's name) to create and visualize a knowledge graph for all books related to that example


                                                                        Code Explanation
Imports and Data Loading
import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt

# Load the CSV file into a pandas DataFrame
df = pd.read_csv('filtered_combined_file.csv', dtype={'Year-Of-Publication': str})
This section imports the necessary libraries and loads the CSV file into a pandas DataFrame. The dtype parameter ensures that the 'Year-Of-Publication' column is treated as a string.


Function to Create and Draw the Knowledge Graph for a Specific Example
def create_knowledge_graph_for_example(category, example):
    # Filter the DataFrame for the specified example in the given category
    filtered_df = df[df[category] == example]

    # Initialize a directed graph
    G = nx.DiGraph()

    # Add nodes and edges for the relationships between all entities in the filtered DataFrame
    for index, row in filtered_df.iterrows():
        title = row['Book-Title']
        author = row['Book-Author']
        year = row['Year-Of-Publication']
        publisher = row['Publisher']

        G.add_node(title, type='Book-Title')
        G.add_node(author, type='Book-Author')
        G.add_node(year, type='Year-Of-Publication')
        G.add_node(publisher, type='Publisher')

        G.add_edge(title, author, relationship='written by')
        G.add_edge(title, year, relationship='published in')
        G.add_edge(title, publisher, relationship='published by')

    # Draw the graph
    pos = nx.spring_layout(G)
    edge_labels = nx.get_edge_attributes(G, 'relationship')

    plt.figure(figsize=(15, 15))
    nx.draw(G, pos, with_labels=True, node_size=3000, node_color='skyblue', font_size=10, font_weight='bold')
    nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels, font_color='red')
    plt.title(f'Knowledge Graph for {category}: {example}')
    plt.show()

This function filters the DataFrame for the specified example in the given category and creates a directed graph with nodes and edges representing the relationships between book titles, authors, publication years, and publishers. It then draws the graph using matplotlib.

User Interaction
# Mapping of user-friendly names to DataFrame column names
entity_map = {
    "1": "Book-Title",
    "2": "Book-Author",
    "3": "Year-Of-Publication",
    "4": "Publisher"
}

# Prompt user for input
print("Choose a category to create a knowledge graph for a specific example:")
print("1. Title")
print("2. Author")
print("3. Year")
print("4. Publisher")

category_choice = input("Enter the number for the category: ")

if category_choice not in entity_map:
    print("Invalid choice.")
    exit()

category = entity_map[category_choice]
example = input(f"Enter the example for {category}: ")

# Create and draw the knowledge graph for the specified example
create_knowledge_graph_for_example(category, example)


This section prompts the user to choose a category and enter an example for that category. It then calls the create_knowledge_graph_for_example function to create and draw the knowledge graph for the specified example.
