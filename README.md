# STAT 545B Assignment B4 Option A

### Description  
This repository contains the files and code authored by Renessa Gomes as submission for **Assignment B4** in STAT 545B. This submission was completed according to the guidelines and requirements found [**here**](https://stat545.stat.ubc.ca/assignments/assignment-b4/). 

Three options for completion were presented and of these, **Option A** was selected. **Exercises 1** and **2** were selected for submission.   

The purpose of **Assignment B4 Option A** is to apply concepts presented in class to accomplish various exercises involving strings and functional programming in R. 

### Sources of Data  
Exercise 1 of this assignment required analysis of a text. *A Christmas Carol* by Charles Dickens was chosen and retrieved from [**Project Gutenberg**](https://dev.gutenberg.org/ebooks/19337). 

### Files in the Repository  
This repository contains the following files:  
* `README.md` : introduction and guide to this repository 
* `exercise-1` : folder holding files related to Assignment B4: Exercise 1
  * `exercise-1.Rmd` : submitted code for required tasks of Exercise 1
  * `exercise-1.md` : knit output file and most up-to-date version of `exercise-1.Rmd`
  * `exercise-1_files` : png files of produced plots to view on GitHub
* `exercise-2` : folder holding files related to Assignment B4: Exercise 2
  * `exercise-2.Rmd` : submitted code for required tasks of Exercise 2
  * `exercise-2.md` : knit output file and most up-to-date version of `exercise-2.Rmd`

### Running the Code  
The following packages are required to run the code in `exercise-1.Rmd` and may need to be installed: 
* `gutenbergr` : includes functions required to load texts from Project Gutenberg
* `tidytext` : includes functions required to tidy text data
* `dplyr` : includes functions required for data wrangling
* `stringr` : includes functions required for string manipulation and detection
* `forcats` : includes functions to manipulate factors and levels in data
* `ggplot2` : includes functions required for data visualization

The following packages are required to run the code in `exercise-2.Rmd` and may need to be installed:
* `dplyr` : includes `case_when()` function vectorize multiple `if_else()` statements
* `stringr` : includes functions required for string manipulation
* `tibble` : includes `tibble()` function to create tibbles from scratch
* `testthat` : includes functions required to test and evaluate function behaviors



