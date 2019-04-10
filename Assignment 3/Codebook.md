# *Cleaning and Getting Data* Assignment code book for run_analysis.R

### The run_analysis.R script performs the data preparation and then followed below steps according to project’s definition.
1. Merges the training and the test sets to create one data set.
2. Extracts only the measurements on the mean and standard deviation for each measurement. 
3. Uses descriptive activity names to name the activities in the data set
4. Appropriately labels the data set with descriptive variable names. 
5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject

### Data Preparation 

The script, `run_analysis.R` does the following things for Data Preparation:

 - Load the `library(dplyr)` for performing cleaning of data.
 - Download the Zip file using the URL "http://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip" and saving it in Repository `UCI HAR Dataset`.
 - Unzip the repository.
 - read the `features.txt` file and `activity_labels.txt' to get the features and Acitivity.
 - Read the `subject_test.txt and subject_train.txt` files.
 - read the `X_test.txt, X_train.txt, Y_test.txt, Y_train.txt` files.
 - Given names to `X_test.txt, X_train.txt` based on the `features.txt` file.
 
 ### 1. Merges the training and the test sets to create one data set 
 - `rbind` row binding -> binding the x_train and x_test in sequence on rows to form final x_data dataset.
 - `rbind` row binding -> binding the y_train and y_test in sequence on rows to form final y_data dataset.
 - `rbind` row binding -> binding the subject_train and subject_test in sequence.
 - `cbind` column binding -> Binding the x_data, y_data and Subject on column to produce single data set.
 - Single Data frame created.
 
 ### 2. Extracts only the measurements on the mean and standard deviation for each measurement.
 - `result_col <- grep("mean()|std()", colnames(final_data))` Create a vector `result_col` of only mean and std from the column names of single Data frame.
 - `result_col1 <- grep("subject|code",colnames(final_data))` Create a vector `result_col1` of only subject and code from the column names of single Data frame.
 - Column list will be passed to the data frame for selecting those columns only `result_data <- final_data[,c(result_col1, result_col)]`.
 - Columns that hold mean or standard deviation measurements are selected from the dataset, while the other measurement columns are excluded from the rest of the analysis.
 
 ### 3. Uses descriptive activity names to name the activities in the data set.
 - `result_data$code <- activities[result_data$code, 2]` code will take out the descriptive activity name from `activities` data frame column and assign it to the `result_data` data frame.
 
### 4. Appropriately labels the data set with descriptive variable names.
 - `clean_names <- gsub("[()]","", colnames(result_data))` Taking out the column names of `result_data` and assign to vector for operations. replaced "[()]" by *SPACES*.
 - `clean_names[2] <- "Activity"` renaming the `code` column of Data frame `result_data` to `Activity`
 - `clean_names <- gsub("Acc", "Accelerometer", clean_names)` replacing the *'Acc'* column names of Data frame `result_data` to *'Accelerometer'* 
 - `clean_names <- gsub("Gyro", "Gyroscope", clean_names)` replacing the *'Gyro'* column names of Data frame `result_data` to *'Gyroscope'* 
 - `clean_names <- gsub("BodyBody", "Body", clean_names)` replacing the *'BodyBody'* column names of Data frame `result_data` to *'Body'* 
 - `clean_names <- gsub("Mag", "Magnitude", clean_names)` replacing the *'Mag'* column names of Data frame `result_data` to *'Magnitude'* 
 - `clean_names <- gsub("^t", "Time", clean_names)` replacing the starting with t *'t'* column names of Data frame `result_data` to *'Time'* 
 - `clean_names <- gsub("^f", "Frequency", clean_names)` replacing the starting with f *'f'* column names of Data frame `result_data` to *'Frequency'* 
 - `clean_names <- gsub("tBody", "TimeBody", clean_names)` replacing the *'tBody'* column names of Data frame `result_data` to *'TimeBody'* 
 - `clean_names <- gsub("-mean", "Mean", clean_names, ignore.case = TRUE)` replacing the *'-mean'* column names of Data frame `result_data` to *'Mean'* 
 - `clean_names <- gsub("-std", "STD", clean_names, ignore.case = TRUE)` replacing the *'-std'* column names of Data frame `result_data` to *'STD'* 
 - `clean_names <- gsub("-freq", "Frequency", clean_names, ignore.case = TRUE)` replacing the *'-freq'* column names of Data frame `result_data` to *'Frequency'* 
 - `clean_names <- gsub("angle", "Angle", clean_names)` replacing the *'angle'* column names of Data frame `result_data` to *'Angle'* 
 - `clean_names <- gsub("gravity", "Gravity", clean_names)` replacing the *'gravity'* column names of Data frame `result_data` to *'Gravity'* 
 - `colnames(result_data) <- clean_names` Stores the names into Column names of `result_data` data frame.
 
 ### 5. From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject. Grouping done on activity and subject then summarise the mean for that grouping.
 - `avg_activity_subject <- result_data %>%
            group_by(subject, Activity) %>%
            summarise_all(funs(mean))`
 - Grouping was done on subject and Activity column and then summarised on mean for all columns storing the result in `avg_activity_subject` data frame.
 
 ### Writing the file.
  - `write.table(avg_activity_subject,file = "Mean_on_subject_n_activity.txt",row.names = F, sep = ",")`.
  - Above line write the data frame `avg_activity_subject' in to the "Mean_on_subject_n_activity.txt" file with no  row names.

Each line in `run_analysis.R` is commented. Reference the file for more information on this process.
