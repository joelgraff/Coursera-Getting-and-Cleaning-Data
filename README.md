####The data set for this project can be found at:

[https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip] (https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip)

####Running the script

Save the script data_analysis.R to the folder containing the extracted dataset.  Note, the extracted data must exist in a folder entitled "UCI HAR Dataset".  The run_analysis.R script must be placed in the parent folder.

Execute the script by loading and sourcing it (source 'data_analysis.R').  When complete, the script will display a table view of the resultant dataset.

####Selecting the data

The decision was made at the start to select only those data points which specifically contained the phrase "mean()" or "std()" and not simply "mean".  This dileneation was made as in the first case, it was understood that the variable was a driect mean / standard deviation calculation of a series of values.  Several "mean" variables were excluded, namely 'angle' variables containing the following names:

-gravityMean
-tBodyAccMean
-tBodyAccJerkMean
-tBodyGyroMean
-tBodyGyroJerkMean

These were excluded because they, in themselves, were not mean calculations, but new calculations based upon a mean calculation of a measurement not included in the data set.  Therefore, they are 'mean' calculations in name only and including them in the final result appeared inconsistent with the project scope.

####Data Labels and Structure

As a brief note, the activity labels from the original data set were preserved as were the column variable names.  Data is grouped by each activity each subject performs, with means of the remaining 66 variables for that activity being calculated as the final variable value.

####Script Details

The script globally executes the main function and calls View() on the result.  The script which processes this data is broken down into several functions:

#####LoadAndMergeData()

This is the "main" function of the script.  Form here, all other work is performed and the resulting data frame is returned.

Within this function, the remaining four functions are called in the following order:

1. BuildDataSet()
2. ApplyActivityLabels()
3. ApplySubjectLabels()
4. AggregateMeans()

#####BuildDataSet()

This function loads the testing and training data, selects the features (variables) for the resulting set, and returns a matrix of the result.

Specifically:

- "test/X_test.txt" and "train/X-train.txt" are loaded.
- "features.txt" is loaded
- features which contain ONLY the text "mean()" or "std()" are selected by grep regular expression.
- the two tables are row-bound (rbind) and named with the selected feature names

#####ApplyAcitivtyLabels()

This function applies activity labels from the original set to the data matrix generated by BuildDataSet().

Specifically:

- all activity id numbers are read from "test/Y_test.txt" and "train/Y_train.txt" and row-bound into a single matrix
- activity labels are read from "activity_labels.txt"
- the activity column data is generated as a factor vector using the factor() function with the activity id data and the acitivty labels
- the activity column is prepended column-bound (cbind) to the data matrix provided to the function as input, and the result is returned

#####ApplySubjectLabels()

This function loads the subject data and prepends it as a column to the data matrix generated by BuildDataSet().

Specifically:

- "test/subject_test.txt" and "train/subject_train.txt" are loaded
- the results are row-bound (rbind) into a vector.
- the resulting vector is prepended column-bound (cbind) to the data matrix provided as the function input and the result is returned.

#####AggregateMeans()

This function groups the data by subject and activity and calculates the corresponding mean for each variable.

Specifically:

- generates groups for subject and activity using group_by()
- calculates group means using summarise_each()
- returns the result
