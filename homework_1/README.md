# Homework 1: Aircraft Inventory Analysis

## How to Run the Source Code
### Load in dataset: 
inventory = load_data()

### Part 1: 
inventory = fix_carrier_columns(inventory)

inventory = impute_number_of_seats(inventory)

inventory = impute_capacity_pounds(inventory)

### Part 2:
inventory = standardize_columns(inventory)

inventory = clean_manufacturer(inventory)

inventory = clean_model(inventory)

### Part 3: 
inventory = remove_remaining_missing(inventory)

### Part 4: 
plot_before_boxcox(inventory)

inventory = boxcox(inventory)

plot_after_boxcox(inventory) 

### Part 5: 
inventory = create_column_size(inventory)

status_proportions = plot_operating_status_by_size(inventory)

plot_aircraft_status_by_size(inventory)

### Part 6: 
linear_reg_for_seats = linear_regression_number_of_seats(inventory)

random_forest_for_seats = random_forest_number_of_seats(inventory)

linear_reg_forcap = linear_regression_capacity_in_pounds(inventory)

random_forest_for_cap = random_forest_capacity_in_pounds(inventory)

## Expected Outputs 
### Task 1: 
Imputes the missing values in CARRIER, CARRIER_NAME, AIRLINE_ID, and MANUFACTURE_YEAR columns, removing whitespaces, capitalization inconsistencies, and misinterpreted entries. Imputes the missing values in NUMBER_OF_SEATS and CAPACITY_IN_POUNDS using the median and grouping by AIRCRAFT_TYPE. 

### Task 2: 
Standardize/transforms AIRCRAFT_STATUS and OPERATING_STATUS columns by removing whitespaces and inconsistencies. Standardize/transforms MANUFACTURER column by fixing inaccurate naming conventions to their correct versions. Standardize/transforms MODEL column by removing any unecessary names and characters. 

### Task 3: 
Removes the remaining missing data in inventory, and output statistics: 
Number of rows removed: 37172
Number of data remaining: 95141 rows
Proportion of data remaining: 71.9%

### Task 4:
Outputs skewness of data in columns NUMBER_OF_SEATS and CAPACITY_IN_POUNDS, and plots histogram of data before Box-Cox transformation. 
- Skewness of NUMBER_OF_SEATS: -0.3239
- Skewness of CAPACITY_IN_POUNDS: 4.0522
Applies Box-Cox transformation on NUMBER_OF_SEATS and CAPACITY_IN_POUNDS columns and plots histogram of data after Box-Cox transformation.  
- Box-Cox lambda for NUMBER_OF_SEATS: 0.6379
- Box-Cox lambda for CAPACITY_IN_POUNDS: 0.3391

### Task 5: 
Calculates the 25th, 50th, and 75th percentiles of data in NUMBER_OF_SEATS column, and create new column SIZE. For each group size, calculates the proportions for each operating status and aircraft status, and generates plots for each. 

### Task 6: 
Splits the data into training and test sets, and runs a Linear Regression adn Random Forest model for each column CAPACITY_IN_POUNDS and NUMBER_OF_SEATS. Outputs the RMSE values for each. 
- Linear Regression for NUMBER_OF_SEATS - Train RMSE: 60.69824317053078 and Test RMSE: 61.362504230962124
- Random Forest for NUMBER_OF_SEATS - Train RMSE: 3.1195059304404342 and Test RMSE: 3.315475228940214
- Linear Regression for CAPACITY_IN_POUNDS - Train RMSE: 67941.17261005136 and Test RMSE: 69010.25743579488
- Random Forest for CAPACITY_IN_POUNDS - Train RMSE: 35557.82350522923 and Test RMSE: 39537.0905979372