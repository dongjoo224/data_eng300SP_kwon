# Prerequisites: 
Environment: must be using an AWS EC2 instance running a Jupyter Notebook within a Python virtual environment (ex. "pyspark-venv")
Python packages: must have pyspark, pandas, matplotlib, and boto3 installed. Note that boto3 can be installed automatically via running the very first cell in the notebook. 
Data files: must have the raw Parquet files located in a data/ directory relative to the notebook. 
    data/yellow_tripdata_2026-01.parquet
    data/green_tripdata_2026-01.parquet

# How to Run the Code: 
The project has been formatted so that all you have to do is call one function main(). Open the Jupyter Notebook and run all the cells. The final cell containing "main()" will run all the functions, executing the initialization, data loading, standardizing, cleaning, analytics, model learning, and S3 uploading phases. 

# Expected Outputs: 
For each phase, you should see a corresponding print statement outputted that confirms each phase is runnign correctly. You should see the following print statements and outputs:
    Initializing spark
    Loading the data
    Standardizing the data
    Cleaning the data
    Answer to Q1, Q2, Q3:
    ** A table of analytics will be outputted here **
    Percentage of total trips under 2 miles: 60.45%
    Training RF Model
    Evaluations Table:
    ** A table of analytics will be outputted here **
    Feature Importances Table:
    ** A table of analytics will be outputted here **
    Generating the scatterplot
    ** A scatterplot will be outputted here **
    Plot saved locally:  fare_predictions.png
    File was uplaoded sucessfuly:  taxi_summary_stats.csv
    File was uplaoded sucessfuly:  model_importances_of_predictors.csv
    File was uplaoded sucessfuly:  fare_predictions.png
    Completed everything successfuly!
    
You should see four total tables/plots displaying analytics and answers to questions: 
1. Taxi Stats Table: displays total trips, average fare, and average distance for both the yellow and green taxi types.
2. Model Evaluations Table: displays the training RMSE and testing RMSE values. They are expected to be ~6.19 and ~6.28, respectively.
3. Feature Importances Table: displays the 5 non-fare predictors by their impact on the model. They are ordered by most influencial to least.
4. Scatterplot of Predictions: displays a scatterplot comparing the true fare amounts and predicted fare amounts relative to a perfect prediction line.

If the code runs successfully, you should have the following files saved: 
- taxi_summary_stats.csv should be uploaded to the specified S3 bucket
- model_importances_of_predictors.csv should be uploaded to the specified S3 bucket
- fare_predictions.png should be uploaded to the specified S3 bucket

Finally, you should see "Completed everything successfuly!" which indicates that everything worked and ran. 