Getting the tidy data

* Step  1: download the libraries that I will end up using:
library(plyr)
library(dplyr)
library(stringr)
library(gdata)

* 2. Setting the work directory
setwd("~/Desktop/R/COURSERA/CLASSES/Getting and cleaning data/Final Project/UCI HAR Dataset/test")

* 3. Downloading the raw data to the following variables using the command: read.table

Test:

X_test <- read.table("X_test.txt") 
Type: Data frame of 2947 observations and 561 variables
Contains: Measurements of the device
Details:  Numeric float values 

y_test <- read.table("y_test.txt")
Type: Data frame of 2947 observations and 1 variable
Contains: numeric values that refer to the subject activity
Details: the values of this variable are as follow:
1 WALKING
2 WALKING_UPSTAIRS
3 WALKING_DOWNSTAIRS
4 SITTING
5 STANDING
6 LAYING

subject_test <- read.table("subject_test.txt")
Type: Data frame of 2947 observations and 1 variable
Contains: Numeric values that refer to the subject, in this case there are 9 different subjects
Details: Subjects are: 2, 4, 9, 10, 12, 13, 18, 20 and 24








Train:

X_train <- read.table("X_test.txt") 
Type: Data frame of 7352 observations and 561 variables
Contains: Measurements of the device
Details:  Numeric float values 

y_ train <- read.table("y_test.txt")
Type: Data frame of 7352 observations and 1 variable
Contains: numeric values that refer to the subject activity
Details: the values of this variable are as follow:
1 WALKING
2 WALKING_UPSTAIRS
3 WALKING_DOWNSTAIRS
4 SITTING
5 STANDING
6 LAYING

subject_ train <- read.table("subject_test.txt")
Type: Data frame of 7352 observations and 1 variable
Contains: Numeric values that refer to the subject, in this case there are 21 different subjects
Details: Subjects are: 1, 3, 5, 6, 7, 8, 11, 14, 15, 16, 17, 19, 21, 22, 23, 25, 26, 27, 28, 29 and 30


Features:

features <- read.table("features.txt")
Type: Data frame of 561 observations and 2 variables
Contains: Factors that refer to the description of the measurement


* 4.  Merging the training and test sets:

* 4.1 Use the second observation from the variable:  
First:  Select only the second observation of the variable features and store it to the same variable features
Second: Invert the new variable feature, and store the result in the variable row_feature. With this, we have the measurement description in one long variable (1 obs and 561 variables). 
row_feature will be use to name the columns in a later step.

* 4.2 Making bigger blocks: Use the function cbind to combine the Test and Train dataframes to make the Train_block and Test_block respectively, and finally use the command rbind over Test_block and Train_block to make the main_block (10299 obs, and 563 variables)

*4.3 Getting the main_block dimensions
main_block_dim: main_block dimensions
main_block_rows: main_block number of rows
main_block_cols: main_block number of columns

* 4.4 Prepare a variable lable_names to use it to name the columns of main_block
Add two columns ("Subject" and "Activity") to the row_feature 

* 4.5 Updating the column names of main_block and removing characers
Using a FOR loop we update the main_block names using the lable_names,  and using another FOR loop to remove the strings: "\\()-", "\\-" and replace them with  ""
*4.6 Store the main_block with their new column names to the variable: q1

*5 Creating a data frame containing only the standard deviations and mean measurements

* 5.1 Getting the Subject and Activity columns from main_block
main_block_subject contains only the mains_block column with name: Subject
main_block_activity contains only the mains_block column with name: Activity

* 5.2 Getting the columns from main_block that contain information about the main or standard deviation only 
main_block_std: data frame that contains only standard deviation measurements from the main_block dataframe
main_block_mean: data frame that contains only standard deviation measurements from the main_block dataframe

* 5.3 Combining the variables: main_block_subject, main_block_activity, main_block_mean and main_block_std using the command cbind.data
to generate the new variable: std_mean and copy it to q2_3


* 5.4 Update the column names of std_mean

* 6 Get the std_mean dimensions

std_mean_rows: number of rows in std_mean
std_mean_cols: row of rows in std_mean

* 7 Changing the activities numbers to a better descriptive value
* 7.1 Using a FOR loop we change the Activities number to a more descriptive variable as follows:
1 WALKING
2 WALKING_UPSTAIRS
3 WALKING_DOWNSTAIRS
4 SITTING
5 STANDING
6 LAYING

* 8 Update the variable q2_3

* 9 Calculating the mean
* 9.1 using the command aggregate we calculate the mean of the std_mean   and store the results in the variable q5

