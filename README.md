# Pewlett Hackard Analysis

## Project Overview

Pewlett-Hackard (PH) is a large firm that currently employs 300,024 individuals. Many employees are approaching retirement age, which will result in a significant number of job openings in the organization. We will be conducting a database analysis for Pewlett Hackard with detailed information on the number of future retirees from all departments currently working at the company to be able to prepare a plan to hire new staff and also to prepare a mentorship initiative. 

The goal of this study is to construct a list of employees approaching retirement age using Pewlett-Packard employee data. Employees born between January 1, 1952, and December 31, 1955 are included on the list. The company's management must be aware of the overall number of retirees, as well as the department in which they work and their title.

The database provided by Pewlett-Packard consisted of 6 CSV files:

+ [employees.csv](https://github.com/dosanity/Pewlett-Hackard-Analysis/files/9433529/employees.csv)
+ [departments.csv](https://github.com/dosanity/Pewlett-Hackard-Analysis/files/9433516/departments.csv) 
+ [dept_emp.csv](https://github.com/dosanity/Pewlett-Hackard-Analysis/files/9433522/dept_emp.csv)
+ [dept_manager.csv](https://github.com/dosanity/Pewlett-Hackard-Analysis/files/9433527/dept_manager.csv)
+ [titles.csv](https://github.com/dosanity/Pewlett-Hackard-Analysis/files/9433532/titles.csv)
+ [salaries.csv](https://github.com/dosanity/Pewlett-Hackard-Analysis/files/9433533/salaries.csv)

## Resources

+ Data Tools: PostgreSQL

+ Software: pgAdmin 4.26

## Entity Relationship Diagrams (ERDs)

An entity relationship diagram (ERD) is a type of flowchart that highlights different tables and their relationships to each other. The ERD does not include any actual data, but it does capture the following pertinent information from each CSV file. We used the ERD to capture the primary keys, foreign keys, and data types of each column.

![image](https://user-images.githubusercontent.com/29410712/186916191-dac541c0-bcf1-4195-9369-bc9f5108cc86.png)

## Results

### The Number of Retiring Employees by Title

We created a table that holds all the titles of employees who were born between January 1, 1952 and December 31, 1955 which is stored in the `retirement_titles.csv`. 

```
SELECT 	e.emp_no,
e.first_name,
e.last_name,
ti.title,
ti.from_date,
ti.to_date
INTO retirement_titles
FROM employees as e
INNER JOIN titles as ti
ON (e.emp_no = ti.emp_no)
WHERE (e.birth_date BETWEEN '1952-01-01' AND '1955-12-31')
ORDER BY e.emp_no ASC;
```
Next we created a table to hold all the current titles of employees who were born between Jaunary 1, 1952 and December 31, 1955 which is stored in the `unique_titles.csv`. 

```
SELECT DISTINCT ON (emp_no) emp_no,
first_name,
last_name,
title
INTO unique_titles
FROM retirement_titles
WHERE (to_date = ('9999-01-01'))
ORDER BY emp_no, to_date DESC;
```
Finally we counted the number of retiring individuals by their current titles which is stored in the `retiring_titles.csv`.
```
SELECT COUNT(title), title
INTO retiring_titles
FROM unique_titles
GROUP BY title
ORDER BY COUNT(title) DESC;
```

<img src="https://user-images.githubusercontent.com/29410712/186964316-8e6fa234-f03a-428c-9431-ce176245eb14.png"  width=50% height=50%>

+ Looking at the numbers in the table we see that 24% of the employees of the company are about to retire. This is a high percentage of individuals retiring and would require Pewlett Hackard to hire many new employees. 
+ Additionally, around 50% of all retirees are engineers and 71% are holding senior positions.

## The Number of Employees Eligible for the Mentorship Program

Given the vast number of employees departing and the skillsets they possess, the firm decides to select a group of employees born in 1965 (i.e., they are 10 years younger than the retirees) and turn them into trainers for new hires and coworkers who will be promoted to fill future openings. The management at Pewlett Hackard wants to be prepared for this development, known as the "Silver Tsunami" by designating some of the retirees as mentors to train the future generation of workers who would run Pewlett Hackard's operations and day-to-day activities.

We created a table that holds all of the employees who were eligible for the mentorship program and we stored in the `mentorship_eligibility.csv`. 

```
SELECT DISTINCT ON (e.emp_no) e.emp_no,
e.first_name,
e.last_name,
e.birth_date,
dep.from_date,
dep.to_date,
ti.title
INTO mentorship_eligibilty
FROM employees as e
INNER JOIN dept_emp as dep
ON (e.emp_no = dep.emp_no)
INNER JOIN titles as ti
ON (e.emp_no = ti.emp_no)
WHERE (dep.to_date = ('9999-01-01')) 
AND (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')
ORDER BY e.emp_no;
```

+ There are 1549 employees eligible for the mentorship program.
+ With 72458 retirees needing replacement, there would be about 46 employees per trainer. 

As a result, Pewlett Hackard would need to increase the number of trainers to make the program effective.

## Summary

### How many roles will need to be filled as the "silver tsunami" begins to make an impact?

As stated before, Pewlett Hackard would need to increase the number of trainers to make the program effective. Currently, there are 1549 employees eligible for the mentorship program and 72458 retirees needing replacement. This would result in about 46 employees per trainer. The company would need about 4.5 times more trainers to effectively have "silver tsunami" create an impact.

### Are there enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees?

There are not enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees. Each mentor would need to train about 46 new employees. Depending on how many employees each mentor can train, Pewlett Hackard would need to increase the amount of mentors to make the program effective.
