# Task 1

attempt_download("dongjoo-26spring", "data/ml-1m.zip", "ml-1m.zip", "https://files.grouplens.org/datasets/movielens/ml-1m.zip")

**Expected output**: 
If the dataset has not yet been downloaded, the function will fetch it and upload it to the specified S3 bucket, printing "Downloaded dataset to S3 bucket." and the file will appear under data/ml-1m.zip in the bucket. If the dataset already exists in S3, the function skips the download entirely and prints "Already downloaded. Nothing to do.".

# Task 2

extract_dataset_from_s3("dongjoo-26spring", "data/ml-1m.zip", "ml-1m.zip", "data/")

**Expected output**:
When extract_dataset_from_s3 is called, it downloads ml-1m.zip from S3 and extracts its contents into the data/ folder on your instance. If successful, you will see "Retrieving file from S3 bucket" followed by "Done! Files are now in the 'data/' folder on your instance."

output_data, movies_df = run_offline_step("data/ml-1m/movies.dat", "dongjoo-26spring", "data/movie_embeddings_1980.pt", "movie_embeddings.pt")

**Expected output**:

When run_offline_step is called, it first checks S3 for existing embeddings. On the first run, no embeddings exist yet, so the function filters movies to 1980 and earlier, builds BERT text for each movie, generates embeddings, saves the result locally, and uploads it to S3, printing "Embeddings have not been made. Generating now." followed by "Embeddings created and stored in S3". On subsequent runs, the function will see that the embeddings are already in S3, and will skip regenerating them entirely, and will load the cached file instead, printing "Embeddings have already been made."

# Task 3

ratings = load_ratings("data/ml-1m/ratings.dat") 
movie_data = output_data 
movies_filtered = movies_df 
final_results = get_all_recommendations(movie_data, ratings, movies_filtered, "dongjoo-26spring")

**Expected output**:
When get_all_recommendations is called, it loads all user ratings, selects a random user from the top 5% of users based on their interaction count, and creates a new user with no history as the cold user. Then it generates 5 movie recommendations for each. The results are stored in the final_results variable . If the function runs successfully, you will see "Generating recommendations for cold user." followed by "Generating recommendations for top user."

save_recs_to_s3(final_results, "dongjoo-26spring", "results/task3_recs.json")

**Expected output**: 
When save_recs_to_s3 is called, it writes the recommendations to a local JSON file and uploads it to S3 under results/task3_recs.json. The cold user always receives the 5 most recently released movies from the filtered dataset since there is no history to personalize their recommendations from. The top user receives recommendations based on BERT cosine similarity between their watch history embedding and all the movie embeddings in the filtered dataset. If the file is uploaded successfully, you will see "Uploaded recommendations to S3 bucket." You can also verify the file appeared by checking your S3 bucket directly.

# Task 4
get_all_recommendations_full("dongjoo-26spring", "data/ml-1m/movies.dat", "data/ml-1m/ratings.dat")

**Expected output**:
When get_all_recommendations_full is called, it reuses the existing method structures from Tasks 2 and 3 but on the full movie set with no filtering. It generates embeddings for all movies, gets recommendations for both user types, and saves the results to S3 under results/task4_recs.json. On the first run, no embeddings exist yet for the full dataset so they are generated and uploaded to S3, and you will see "Embeddings have not been made. Generating now." followed by "Embeddings created and stored in S3", "Generating recommendations for cold user.", "Generating recommendations for top user.", and finally "Recommendations for task 4 have been uploaded to S3." On subsequent runs, the embeddings are already in S3 and are reused, so "Embeddings have not been made. Generating now." and "Embeddings created and stored in S3" are replaced with "Embeddings have already been made.", with the remaining output staying the same.

You will also see a JSON output of each user and their recommendations, formatted. 

# Task 5

get_personal_recommendations("dongjoo-26spring", output_data, movies_df)

**Expected output**:
When get_personal_recommendations is called, it creates a personal user profile based on 10 hardcoded movie ratings. For the movies I picked for myself, I chose movies that reflected a preference for romantic and animated classics and a dislike of sci-fi and horror. It is saves the profile to S3 under results/my_profile.json, then generates 5 personalized recommendations by computing the cosine similarity again between the user's taste embedding and all the movie embeddings in the filtered set, and saves those to S3 under results/my_recs.json. Since the ratings are hardcoded, the recommendations will always be the same every run. If everything runs successfully, you will see "Profile has been saved to S3." followed by "Recommendations have been saved to S3."

You will also see a JSON output of the recommendations, formatted. 

# AI Usage
**Tools Used:** Claude

**Key Prompts:** 
* Debugging package and import errors I encountered when setting up the environment (ex. resolving compatibility conflicts with torch, transformers, and boto3)
* How to extract files from a zip in Python
* How to write a dictionary to a local JSON file in Python

**Results:**
* For package and import issues, Claude helped identify the correct install commands and import order because using the direct imports we used in class were causing errors in my notebook. I verified the fixes by re-running the affected cells and ensuring that no errors were appearing anymore.
* For the zip extraction code (zipfile.ZipFile with extractall), I adapted it into my functions and verified it by confirming that for instance the data/ml-1m/ folder and its contents appeared on my instance after running the cell.
* For the JSON write snippet (json.dump with open), I adapted it into my save_recs_to_s3 and get_personal_recommendations functions and verified it by checking that the correct files appeared in my S3 bucket after running those cells.
