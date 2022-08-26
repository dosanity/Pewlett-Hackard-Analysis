# Pewlett Hackard Analysis

## Project Overview

Pewlett-Packard (PH) is a large firm that currently employs 300,024 individuals. Many employees are approaching retirement age, which will result in a significant number of job openings in the organization. We will be conducting a database analysis for Pewlett Hackard with detailed information on the number of future retirees from all departments currently working at the company to be able to prepare a plan to hire new staff and also to prepare a mentorship initiative. 

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


