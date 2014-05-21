
CodeBook
--------

Original Data
-------------

This dataset is a tiny dataset resulted from a transformation of a bigger set of data, namely

Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012

The originary data source can be found at http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones

Oridinal dataset description taken from its website:

    The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data. 

    The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain. 

    For each record in the dataset it is provided: 
    - Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration. 
    - Triaxial Angular velocity from the gyroscope. 
    - A 561-feature vector with time and frequency domain variables. 
    - Its activity label. 
    - An identifier of the subject who carried out the experiment.




Data Acquisition
----------------

The data is acquired automatically by the script. Please make sure to set your working directory at line 1 of the script, variable working.directory.

When the script is run, the following steps happen for obtaining the data

    1. A directory named data is created (if not already existing) in the current working directory
    2. The working directory is switched to the data folder
    3. If the original data set (named dataset.zip) does not exist already, the data set is downloaded from https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip and named dataset.zip
    4. The dataset is unzipped. The original dataset is present in data/UCI HAR

Data Transformation
-------------------

Starting from the originary dataset, the following high-level steps are performed

    1. The files features.txt, train/X_train.txt, and test/X_test.txt are loaded as data tables. The X_ tables are combined via rbind()
    2. As requested by step 2 of the Project page (https://class.coursera.org/getdata-003/human_grading/view/courses/972136/assessments/3/submissions), only the measurements of the mean and the standard deviation are retained from the originary dataset. This is achieved via df <- df[, grep(".*\\.(mean|std)\\.\\..*", names(df), value=T)]
    3. The files train\\subject_train.txt and test\\subject_test.txt are loaded and combined via the rbind() command
    4. The files train\\y_train.txt and test\\y_test.txt are loaded and combined via the rbind() command
    5. As requested by step 3 of the Project page, the descriptive names for the activities, found in activity_labels.txt, are used in the intermediary tidy dataset.
    6. Point 1 of the Project page is performed. The training and the test sets are merged to create one data set. At this point, the dataset is in the ![form suggested in Coursera forms:](https://coursera-forum-screenshots.s3.amazonaws.com/d3/2e01f0dc7c11e390ad71b4be1de5b8/Slide2.png)
    7. A series of regular expressions clean the name of the variables. The variables of the original data set are in the form fBodyAccJerk-mean()-Z. R automatically converts the unfriendly symbols, e.g., fBodyAccJerk.mean...Z. The regular expressions transform the intermediary names in R coding standard-friendly names, e.g., f.body.acc.jerk.mean.z. The variables are listed below.
    8. The intermediary dataframe is melted to obtain all the activities performed by all the users, and their average values. an O(N^3) for loop is performed, first using the unique subject values, then the unique activity values per subject, then the variable values. The average values are computed and stored in the means temporary variable. Before the end of the outer loop cycle, the means variable is appended to the soon-to-be-born tidy dataset
    9. The columns are re-ordered to have subject and activity as first columns
    10. The dataset tidymeans.txt is produced.


Variables
---------

The following is a table representing the variables of the tidy dataset. They are provided as they appear in the dataset.
Variable subject is the participant anonymous code from the original experiment. It is an integer number ranging from 0 to 30.
Variable activity is a categorical variable of the activity performed by the participants. Its value is one from the set {LAYING, SITTING, STANDING, WALKING, WALKING_DOWNSTAIRS, WALKING_UPSTAIRS}.

All the other variables (ID 3 to 68) are the average of each original variable (see XXX) for each activity and each subject. Their value is numeric (float). It can be negative or positive.


| ID | Name                           |
|----|--------------------------------|
| 1  | subject                        |
| 2  | activity                       |
| 3  | t.body.acc.mean.x              |
| 4  | t.body.acc.mean.y              |
| 5  | t.body.acc.mean.z              |
| 6  | t.body.acc.std.x               |
| 7  | t.body.acc.std.y               |
| 8  | t.body.acc.std.z               |
| 9  | t.gravity.acc.mean.x           |
| 10 | t.gravity.acc.mean.y           |
| 11 | t.gravity.acc.mean.z           |
| 12 | t.gravity.acc.std.x            |
| 13 | t.gravity.acc.std.y            |
| 14 | t.gravity.acc.std.z            |
| 15 | t.body.acc.jerk.mean.x         |
| 16 | t.body.acc.jerk.mean.y         |
| 17 | t.body.acc.jerk.mean.z         |
| 18 | t.body.acc.jerk.std.x          |
| 19 | t.body.acc.jerk.std.y          |
| 20 | t.body.acc.jerk.std.z          |
| 21 | t.body.gyro.mean.x             |
| 22 | t.body.gyro.mean.y             |
| 23 | t.body.gyro.mean.z             |
| 24 | t.body.gyro.std.x              |
| 25 | t.body.gyro.std.y              |
| 26 | t.body.gyro.std.z              |
| 27 | t.body.gyro.jerk.mean.x        |
| 28 | t.body.gyro.jerk.mean.y        |
| 29 | t.body.gyro.jerk.mean.z        |
| 30 | t.body.gyro.jerk.std.x         |
| 31 | t.body.gyro.jerk.std.y         |
| 32 | t.body.gyro.jerk.std.z         |
| 33 | t.body.acc.mag.mean            |
| 34 | t.body.acc.mag.std             |
| 35 | t.gravity.acc.mag.mean         |
| 36 | t.gravity.acc.mag.std          |
| 37 | t.body.acc.jerk.mag.mean       |
| 38 | t.body.acc.jerk.mag.std        |
| 39 | t.body.gyro.mag.mean           |
| 40 | t.body.gyro.mag.std            |
| 41 | t.body.gyro.jerk.mag.mean      |
| 42 | t.body.gyro.jerk.mag.std       |
| 43 | f.body.acc.mean.x              |
| 44 | f.body.acc.mean.y              |
| 45 | f.body.acc.mean.z              |
| 46 | f.body.acc.std.x               |
| 47 | f.body.acc.std.y               |
| 48 | f.body.acc.std.z               |
| 49 | f.body.acc.jerk.mean.x         |
| 50 | f.body.acc.jerk.mean.y         |
| 51 | f.body.acc.jerk.mean.z         |
| 52 | f.body.acc.jerk.std.x          |
| 53 | f.body.acc.jerk.std.y          |
| 54 | f.body.acc.jerk.std.z          |
| 55 | f.body.gyro.mean.x             |
| 56 | f.body.gyro.mean.y             |
| 57 | f.body.gyro.mean.z             |
| 58 | f.body.gyro.std.x              |
| 59 | f.body.gyro.std.y              |
| 60 | f.body.gyro.std.z              |
| 61 | f.body.acc.mag.mean            |
| 62 | f.body.acc.mag.std             |
| 63 | f.body.body.acc.jerk.mag.mean  |
| 64 | f.body.body.acc.jerk.mag.std   |
| 65 | f.body.body.gyro.mag.mean      |
| 66 | f.body.body.gyro.mag.std       |
| 67 | f.body.body.gyro.jerk.mag.mean |
| 68 | f.body.body.gyro.jerk.mag.std  |


The features variables somehow follow the following naming convention

{f|t}.{body|gravity}.{acc|gyro}.{mag|mean|std}

where

t is time, f is the frequency, body and gravity are reference frames, acc is the accelerometer, gyro is the gyroscope, mag is the euclidean magnitude, mean is the average value, and std is the standard deviation. Jerk, where present, is the jerk signal, as opposed to smooth signal (everything else)
