Libraries used
==============

    library(tidyr)
    library(dplyr)

Overview
========

A client is expecting a girl child soon and is not sure of what to name he baby. He wants to analyze the popular girl names given to children born in 2015 and 2016 and then make a decisoion on what to name his soon to be born child.

Raw Data
========

The raw data is contained in 2 files and is provided by the client. Means of data collection are not known.

-   **yob2015.txt** : txt file containing 1 observation in each line. Each observation consists of 3 variables ( **comma** separated). The variables are the first name given to a child born in **2015**, their gender and the number of children that were given that name respectively
-   **yob2016.txt** : txt file containing 1 observation in each line. Each observation consists of 3 variables ( **semicolon** separated). The variables are the first name given to a child born in **2016**, their gender and the number of children that were given that name respectively

Tidy Data (and how to get to it)
================================

### y2016

The raw data contains a wrong entry in yob2016.txt. This needs to be deleted. y2016 is a Dataframe for holding the cleaned up values for yob2016 file with one column for each variable and 1 row per observation

    text <- readLines("yob2016.txt") # Read the text file
    # Convert to dataframe, split and rename columns
    df <- data.frame(text, stringsAsFactors = FALSE) %>% separate(text,c("Name","Gender","Count"))
    df$Count <- as.numeric(df$Count) # correct the class of count
    y2016 <- df[df$Name != grep("yyy$",df$Name,value = TRUE), ]

### y2015

Dataframe for holding the vaues for yob2015 file with one column for each variable and 1 row per observation

    text <- readLines("yob2015.txt") # Read the text file
    # Convert to dataframe, split and rename columns
    y2015 <- data.frame(text, stringsAsFactors = FALSE) %>% separate(text,c("Name","Gender","Count"),sep = ',')
    y2015$Count <- as.numeric(y2015$Count) # correct the class of count

### final

This is the merged dataset containing information from both y2015 and y2016. Dataframe is merged by "Name" and "Gender"

    final <- merge(y2015,y2016,by=c('Name','Gender'))
    names(final)[3:4] <- c('Count 2015','Count 2016') # update merged names
    str(final)

Output
======

File named Top10Girls.csv which contains the top 10 female child names given in 2015 and 2016 combined
