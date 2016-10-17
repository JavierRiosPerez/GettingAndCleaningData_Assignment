# GettingAndCleaningData_Assignment
Solution for the Getting and cleaning data course
======

## libraries and others
library(plyr)
library(dplyr)
library(stringr)
library(gdata)

# setting the work directory
# all the files are located in a folder called UCI HAR Dataset
setwd("~/Desktop/R/COURSERA/CLASSES/Getting and cleaning data/Final Project/UCI HAR Dataset/test")


## reading the test data
X_test <- read.table("X_test.txt")
y_test <- read.table("y_test.txt")
subject_test <- read.table("subject_test.txt")

table(subject_test)
table(y_test)


## reading the train data
setwd("~/Desktop/R/COURSERA/CLASSES/Getting and cleaning data/Final Project/UCI HAR Dataset/train")
X_train <- read.table("X_train.txt")
y_train <- read.table("y_train.txt")
subject_train <- read.table("subject_train.txt")

table(subject_train)
table(y_train)

## Reading the features
setwd("~/Desktop/R/COURSERA/CLASSES/Getting and cleaning data/Final Project/UCI HAR Dataset")
features <- read.table("features.txt")


##################################################################
# question 1
# Merges the training and the test sets to create one data set
##################################################################

## converting the feature from col to row
features <- select(features, V2)
row_feature <- as.data.frame(t(features))


## making blocks
Train_block <- cbind(subject_train, y_train, X_train)
Test_block <- cbind(subject_test, y_test, X_test)
main_block <- rbind(Train_block, Test_block)

## at this point we have the main block with labes...
main_block_dim <- dim(main_block)
main_block_rows <- main_block_dim[1]
main_block_cols <- main_block_dim[2]

##editing the lables

lable_names <- as.data.frame(c("Subject","Activity",row_feature))

## this code changes the col names of the main block
  for(i in 1:main_block_cols){

    names(main_block)[i] <- as.character(lable_names[1,i])
  }


## this code changes the col names of the main block
for(i in 1:main_block_cols){
  
  names(main_block)[i] <- as.character(lable_names[1,i])
  temp <- names(main_block)[i]
  temp <- gsub("\\()-"," ",temp)
  temp <- gsub("\\-", " ", temp)
  names(main_block)[i] <- temp
}

q1 <- main_block


##########################################################################################
# question 2 and 3
#Extracts only the measurements on the mean and standard deviation for each measurement.
#Uses descriptive activity names to name the activities in the data set
##########################################################################################


## getting the mean and strdv only
## -mean()-
## -std()-

main_block_subject <- main_block[ , grepl("Subject", colnames(main_block))]
main_block_activity <- main_block[ , grepl("Activity", colnames(main_block))]

main_block_std <- main_block[ , grepl("std", colnames(main_block))]
main_block_mean <- main_block[ , grepl("mean", colnames(main_block))]

## merging both data frames
std_mean <- cbind.data.frame(main_block_subject,main_block_activity,main_block_mean,main_block_std)
q2_3 <- std_mean


#changing the name of the col names
colnames(std_mean)[1] <- "Subject"
colnames(std_mean)[2] <- "Activity"

## changing the activities names from numbers to a description
## 1 walking
## 2 walking upstairs
## 3 walking downstaris
## 4 sitting
## 5 standing
## 6 lying

std_mean_rows <- dim(std_mean)[1]
std_mean_cols <- dim(std_mean)[2]

for(i in 1:std_mean_rows)
{
  if(std_mean[i,2] == 1)
    { 
      std_mean[i,2] <- "walking"
  }
  if(std_mean[i,2] == 2)
  { 
    std_mean[i,2] <- "walking upstairs"
  }
  if(std_mean[i,2] == 3)
  { 
    std_mean[i,2] <- "walking downstairs"
  }
  if(std_mean[i,2] == 4)
  { 
    std_mean[i,2] <- "sitting"
  }
  if(std_mean[i,2] == 5)
  { 
    std_mean[i,2] <- "standing"
  }
  if(std_mean[i,2] == 6)
  {
    std_mean[i,2] <- "lying"
  }
}
q2_3 <- q2_3 # updateing the q2 and q3 answers


##########################################################################################
# question 4
#Appropriately labels the data set with descriptive variable names.
##########################################################################################
  
std_mean$Activity <- factor(std_mean$Activity)
q4 <- std_mean

##########################################################################################
# question 5
# From the data set in step 4, creates a second, independent tidy data set with the average 
#of each variable for each activity and each subject.
##########################################################################################

q5 <- aggregate(std_mean[,-c(1,2)],list(std_mean$Subject, std_mean$Activity), mean)
#changing the name of the col names
colnames(q5)[1] <- "Subject"
colnames(q5)[2] <- "Activity"
 
