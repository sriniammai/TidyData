# data_analysis.R logic walk through
* set the working directory to the unarchived package home 'UCI HAR Dataset'
* Load the activity_labels.txt and features.txt files to activity_lb and variable_lb dataTables respectively
* Load the files under train subdirectory as dataTables (like x_train, y_train and subject_train ) 
* Load the files under test subdirectory as dataTables (like x_test, y_test and subject_test )
* Row bind the train and test files respectivly(like total_x, total_y and total_subject) and assign appropriate variable names.
* select only mean() and std() variables on total_x 
* finally group by subject & activity and find the mean of each identified group for all variables.

