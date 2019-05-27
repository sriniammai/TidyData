# data_analysis.R logic walk through
* Load dplyr library
library(dplyr)
* set the working directory to the unarchived package home 'UCI HAR Dataset'
setwd('C:\\Cleaning\\week4\\UCI HAR Dataset\')
* Load the activity_labels.txt and features.txt files to activity_lb and variable_lb dataTables respectively
activity_lb <- read.table("./activity_labels.txt")
variable_lb <- read.table("./features.txt")
* Load the files under train subdirectory as dataTables (like x_train, y_train and subject_train ) 
x_train <- read.table("./train/X_train.txt")
y_train <- read.table("./train/y_train.txt")
subject_train <- read.table("./train/subject_train.txt")
* Load the files under test subdirectory as dataTables (like x_test, y_test and subject_test )
x_test <- read.table("./test/X_test.txt")
y_test <- read.table("./test/y_test.txt")
subject_test <- read.table("./test/subject_test.txt")
* Row bind the files and create the respective total files
total_x <- rbind(x_train, x_test)
colnames(total_x) <- variable_lb[,2]
* Select only required mean and standard deviation measurment variables
total_x <- total_x[grep(pattern = 'mean\\(\\)|std\\(\\)', names(total_x))]
* Overwrite activity number to descriptive activity name as variable name 'activity'
total_y <- left_join(rbind(y_train, y_test), activity_lb, by = 'V1')[2]
names(total_y)[1] <- 'activity'
* Subjects from 1 to 30 as variable name 'subject'
total_subject <- rbind(subject_train, subject_test)
names(total_subject)[1] <- 'subject'
* final data after gathering and cleaning
final_tbl <- cbind(total_subject,total_y, total_x)
* mean of each variable by subject and activity
final_tidy_tbl <- final_tbl %>% group_by(subject,activity ) %>% summarise_each(funs = mean)
* the tidy data
View(final_tidy_tbl)
