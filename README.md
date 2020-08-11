Getting & Cleaning Data Project
About this repository

This repository was created for the peer-graded assignment in Getting & Cleaning Data course project from John Hopkins University. The purpose of this assignment is to demonstrate the ability to collect, work with, and clean a dataset. The goal is to prepare tidy data that can be used for later analysis.
Details on the files that exist in this repository
1. README.md

A text file which describe this repository and walkthrough of the main script.
2. run_analysis.R

The main script to produce the results (/Results) in this repository.
3. CodeBook.md

Codebook describing variables, the data and transformations
4. Results

Folder containing .txt files which was generated form run_analysis.R script. The generated .txt files in this folder are named as follows:

Class A (includes meanFreq variables) :

    dataset_A.txt
    summary_dataset_A.txt

Class B (excludes meanFreq variables) :

    dataset_B.txt
    summary_dataset_B.txt

5. UCI HAR Dataset

The unzipped copy of the dowloaded dataset from http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones
6. Course Project.Rproj

R Studio project file
run_analysis.R walkthrough

Note: There are arguments in the forum whether to include meanFreq() variables or not in the tidy dataset. As I couldn't find a definite answer to the arguments, I've decided to do both. The script will produce two classes of datasets (A & B), where class A include the meanFreq() variables while class B exclude them.

The script require dplyr package to run (install.packages('dplyr') > library('dplyr'))

run_analysis.R script generate the results according to the following steps:

    Step 1. Load dplyr package using library('dplyr')
    Step 2. List all files in the folder /UCI HAR Dataset into data_path using list.files() function
    Step 3. Read the .txt of interest using read.table() function. The files of interest are as follows:
        features.txt - a list of all features in the input dataset (X_train, X_test)
        activity_labels.txt - a list of labels for the factored activities in the output dataset (Y_train, Y_test)
        X_test.txt - input data for test set
        y_test.txt - output data for test set
        X_train.txt - input data for training set
        y_train.txt - output data for training set
        subject_train.txt - list of subjects for training set (range: 1:30)
        subject_test.txt - list of subjects for testing set (range: 1:30)
    Step 4. Rename the column labels for each dataset accordingly using the feature list plus 'subject' and 'activity'
    Step 5. Extract only mean and std features for the tidy dataset
        First, the features are extracted using regular expression method through grep function. Here the script use two different regex pattern due to produce two different classes of dataset (refer to the note above) which are "([a-zA-Z]+-)+(mean|std)" and "([a-zA-Z]+-)+(mean|std)\\W".
        Using the results from the grep() function, the select() function is used to extract only the mean and std features
    Step 6. Merge the training and testing dataset into a new dataset using cbind() (column binding) and rbind() (row binding) functions.
    Step 7. Rename the activity data column using descriptive activity names in accordance to the list in the activity_labels.txt
    Step 8. Tidy data operation by relabelling the dataset with descriptive variable names
        Use the cleanFunc() custom function to relabel the dataset. The function apply gsub() function by applying several regex patterns to remove non-alphabets, whitespaces, change abbreviations to their full spellings, and rename duplicates.
        Transform all uppercase elements into lowercase using tolower() function
    Step 9. Create second, independent tidy dataset
        Run the avg_data() custom function to summarize the dataset. The function use pipeline operator to group the dataset by subject & activity using group_by() function, and then the summarise() with accross() function to summarize each element in the dataset.
        Run the newLabel() custom function to rename labels in the dataset by affixing 'average' to the labels (excluding subject and activity) using paste() function
    Step 10. Generate .txt files for the final tidy datasets using write.table() function. The files will be written in the /Results folder.
