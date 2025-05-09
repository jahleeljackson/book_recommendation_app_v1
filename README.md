# Jahleel's Book Recommendation App

## Overview
I want to begin by saying that absolutely *zero percent* of the code in this repository has been AI generated. I am not a vibe coder. This project primarily served as a way for me to learn technologies that I was interested in, namely: 
1. Building Recommender systems
2. Packaging and deploying models for use by stakeholders
3. API integration
4. Using Flask

This is a web application running on a Flask server which returns 5-10 book recommendations after having 5 books (and their ratings) input by the user. 

### Note:
To see and download the actual code and data, click [here](https://drive.google.com/drive/folders/1b4ItnPy-4sOIAQugcUuAF8zEX2HIcpO0?usp=drive_link) to go the project directory in Google Drive (project too large for GitHub).

To view a demo of the project, click [here](https://youtu.be/vPV-n0q69PM)
## Dependencies
Ensure that the python and pip is already installed. After cloning the repository, run the *main.py* and navigate to http://127.0.0.1:5000.

```bash
pip install -U sentence-transformers flask pickle numpy pandas faiss-cpu pathlib requests fuzzywuzzy
```

## Tools used in this project
- Exploratory Data Analysis
- Feature Engineering
- User-based Collaborative Filtering
- Google Books API Integration
- Natural Language Processing

## A Little More In-Depth (if you care)
My thought process.
<img width="1208" alt="Screenshot 2025-05-08 at 6 14 54 PM" src="https://github.com/user-attachments/assets/270e8021-c61e-4c02-b7b3-79aacb888a5f" />

1. The web application (HTML/CSS) runs on a Flask server, containing a form with 5 text inputs and 5 select boxes (book title/author and ratings respectively). See below.

2. After submitting the books and ratings, the data is sent to the Flask app file where it is processed by a module I wrote *find_book.py*. The find_book module incorporate requests and fuzzywuzzy, two popular python libraries used for retrieving data from APIs and measuring text similarity respectively.

3. The books inputted from the user would be sent to the Google Books API, which would return a pretty verbose json object of information from various books.

4. The fuzzywuzzy library would be used to loop through the json object to find the closest matching books. This would ensure that whether a user made a typo in their input would not affect the recommendations-- and I'd be able to learn how to integrate APIs into applications.

5. After returning books from the Google Books API, the books and ratings would be transformed into embeddings. Using SBERT.net's sentence-transformer model, 'all-MiniLM-L6-v2', which can create semantic and contextual embeddings. For instance, given 'Harry Potter and the Sorceror's Stone by J.K. Rowling, the model creates an embedding that would be 'closer' in space to 'Percy Jackson and the Olympians by Rick Riordan' than to 'Atomic Habits by James Clear'.

6. These 5 embeddings are then turned into **one** user embedding using numpy's weighted average function-- weighted by the ratings inputted.

7. This user embedding is measured against other 20,000 other users whos' embeddings I created in *data_processing.ipynb*. The 5 most similar users are then returned. Two of the highest rated books are returned from each users (over 250,000 unique titles).

8. Lastly, duplicate books are removed from these resulting 10 books (which is why it ranges between 5-10 books).

The final result will look like this.
<img width="1208" alt="Screenshot 2025-05-08 at 8 04 23 PM" src="https://github.com/user-attachments/assets/dc4280e7-4f6c-4944-a8eb-a1e23de5335e" />

