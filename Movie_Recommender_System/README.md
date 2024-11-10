movie-recommender-system-tmdb-dataset
Taking this initiative towards building this ML project, a movie recommender system from scratch and deploying them at the same time using streamlit. The webpage takes in the movies names from abundant data of 5000 movies and recoommends top 5 movies based on the 'Tags' column (taken by merging keywords, cast, crew, etc columns) by calculating the cosine similarity and keeping the index in Decending order, thereby suggesting 5 movies with top 5 similarity values

Step 1: Set Up the Environment Import Libraries Begin by importing necessary libraries. For example:

Load Data Load the datasets with pandas:

Step 2: Data Preprocessing Merge Datasets Since the movies and credits datasets share a common column (title), you can merge them:

python Copy code movies = movies.merge(credits, on="title")

Inspect Data Check the structure and identify useful columns:

Feature Selection Identify relevant features for the recommendation, such as genres, keywords, and cast. Extract and clean these fields, using helper functions if needed (like fetch_director for the director).

Step 3: Feature Engineering Parse Columns If columns like crew or cast are in JSON format, convert them into a usable structure:

python Copy code import ast

def fetch_director(text): directors = [] try: for i in ast.literal_eval(text): if i['job'] == 'Director': directors.append(i['name']) except (ValueError, SyntaxError): pass return directors movies['director'] = movies['crew'].apply(fetch_director)

Create New Features Use text-based fields like genres, cast, or director to create a combined text feature that represents each movie.

Step 4: Build the Recommendation System Content-Based Filtering Use methods like cosine similarity on the combined features to recommend movies:

python Copy code from sklearn.metrics.pairwise import cosine_similarity from sklearn.feature_extraction.text import CountVectorizer

Example of vectorizing the combined text feature
count_vectorizer = CountVectorizer() count_matrix = count_vectorizer.fit_transform(movies['combined_features']) cosine_sim = cosine_similarity(count_matrix)

Collaborative Filtering If using user-based or item-based collaborative filtering, set up a matrix of user interactions with movies.

Define Recommendation Function Write a function to fetch the top recommendations based on similarity scores:

python Copy code def get_recommendations(title, cosine_sim=cosine_sim): idx = movies[movies['title'] == title].index[0] sim_scores = list(enumerate(cosine_sim[idx])) sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True) sim_scores = sim_scores[1:11] # Get top 10 recommendations movie_indices = [i[0] for i in sim_scores] return movies['title'].iloc[movie_indices]

Step 5: Test the Recommendation System Run Test Queries Test the system with a sample query to verify it returns appropriate recommendations.

Step 6: Save and Document Document the Code Add comments explaining each function and logic behind the recommendations. Save as Python File Save the notebook as a .py file if needed, which can be done directly from Jupyter Notebook by selecting "File > Download as > Python (.py)".
