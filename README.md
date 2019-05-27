# data_analysis.R logic walk through
* Load dplyr library <br>
library(dplyr)
* set the working directory to the unarchived package home 'UCI HAR Dataset'<br>
setwd('C:\\Cleaning\\week4\\UCI HAR Dataset\')<br>
* Load the activity_labels.txt and features.txt files to activity_lb and variable_lb dataTables respectively<br>
activity_lb <- read.table("./activity_labels.txt")<br>
variable_lb <- read.table("./features.txt")<br>
* Load the files under train subdirectory as dataTables (like x_train, y_train and subject_train ) <br>
x_train <- read.table("./train/X_train.txt")<br>
y_train <- read.table("./train/y_train.txt")<br>
subject_train <- read.table("./train/subject_train.txt")<br>
* Load the files under test subdirectory as dataTables (like x_test, y_test and subject_test )<br>
x_test <- read.table("./test/X_test.txt")<br>
y_test <- read.table("./test/y_test.txt")<br>
subject_test <- read.table("./test/subject_test.txt")<br>
* Row bind the files and create the respective total files<br>
total_x <- rbind(x_train, x_test)<br>
colnames(total_x) <- variable_lb[,2]<br>
* Select only required mean and standard deviation measurment variables<br>
total_x <- total_x[grep(pattern = 'mean\\(\\)|std\\(\\)', names(total_x))]<br>
* Overwrite activity number to descriptive activity name as variable name 'activity'<br>
total_y <- left_join(rbind(y_train, y_test), activity_lb, by = 'V1')[2]<br>
names(total_y)[1] <- 'activity'<br>
* Subjects from 1 to 30 as variable name 'subject'<br>
total_subject <- rbind(subject_train, subject_test)<br>
names(total_subject)[1] <- 'subject'<br>
* final data after gathering and cleaning<br>
final_tbl <- cbind(total_subject,total_y, total_x)<br>
* mean of each variable by subject and activity<br>
final_tidy_tbl <- final_tbl %>% group_by(subject,activity ) %>% summarise_each(funs = mean)<br>
* the tidy data<br>
View(final_tidy_tbl)<br>
