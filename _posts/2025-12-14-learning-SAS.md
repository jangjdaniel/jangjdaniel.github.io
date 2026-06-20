---
layout: post
title: Learning the basics of SAS
date: 2025-12-14 14:24:00
description: Incomplete!
tags: coding
categories: programming
---

I haven't touched SAS since SIBS almost 2.5 years ago. I took on the challenge of learning how to do data wrangling, simple data visualization, and some other things in SAS for my special topics course in the Spring. Below is my attempt at gathering examples and trying to relate the syntax back into R. I'm hoping to have a copy-paste template for myself after this post. These will be my notes / highlights from following along [this resource published by SAS](https://support.sas.com/content/dam/SAS/support/en/books/free-books/sas-programming-for-r-users.pdf)

## What do I remember?

At the moment, all I know is that SAS is not open-source (unlike R) and that I need to put semicolons everywhere. I'm not entirely sure what its benefit is, other than the fact that some healthcare companies still use it.

## Basics: Load in data

This is how we load in data and make sure we're in the proper file path. First we specify the working directory, then we define the data using this syntax. One thing to notice is the incessant use of semicolons. Note that this is all base SAS.

```sas
libname mylib 'C:/path_to_data';

data my_data;
 set mylib.my_data;
run;
```

Now if you want to inspect data (like the head function and that function that extracts all column names). As a SAS cheat sheet, `proc` refers to a procedure in manipulating data or calculating a statistic. Also, note that comments use /* */ and in-line comments must start with a * and end with a semicolon (WHY??)

```sas
proc print data=my_data (obs=10); * print out the first 10 observations from my_data;
run;
```

```sas
proc contents data=my_data; * extracts variables from my_data;
run;
```

---
## Data Wrangling

Now we're going to do some data wrangling and manipulations. The subtitles will refer to the tidyverse equivalent because I love tidyverse. The thing about SAS is that it's less intuitive to data wrangle as there is no one function that does everything for you. Also we are using data steps instead of procedures, since we're manipulating the data itself.

### Add a new variable

Notice that we can do everything within one data call (what is the proper term for this)

```sas
data my_data;
 set my_data; * not entirely sure what this does;
 transformed_variable = length + 1; * length variable increases by 1;
run;
```

Now based on a conditional, we use IF __ THEN ___, ELSE IF ___ as follows

```sas
data my_data;
 set my_data; * not entirely sure what this does;
 transformed_variable = length + 1; * length variable increases by 1;
run;
```

Now for characters, we use some vector stuff

```sas
data my_data;
 set my_data; * not entirely sure what this does;
 transformed_variable = length + 1; * length variable increases by 1;
run;
```


### Rename a variable
### Mutate a variable based on a condition
### Filter a variable based on a condition
### Select a subset of data
### Pivoting data
### Joining two datasets


---
## Basic Statistics

### Descriptive Statistics
### Something else?



---
## Visualizations

Here are some code chunks for the basics of visualizations (a different package?)


---
## Regression Example

Is this also a special package? I also should describe what stuff `my_data` contains for us to run logistic regression successfully.

```sas
proc logistic data=one;
     class yrs2 (ref='0') / param=ref;
     model iqlt84 (event='1') = ses yrs2;
	 title 'Logistic Regression Adjusted for SES';
run; quit;
```

---
## Packages

## Function setups

## Creating a report

