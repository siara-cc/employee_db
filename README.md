# Sqlite3 port of mysql test_db

This project hosts the Sqlite3 db file ported from mysql test_db found at https://github.com/datacharmer/test_db.  To use this project, download employees.db.bz2, unzip and open using sqlite3 command line tool.

Minor changes have been made to the schema per suggestions from people of `sqlite-users` group.

Given below Schema section is the README from the other repository, with the usage part removed.  This project is also hosted under the same license (CC-BY-SA-3.0).

# Schema

![Schema Image](employees-schema.png?raw=true)

```sql
CREATE TABLE employees (
    emp_id      INTEGER  NOT NULL,
    birth_date  DATE     NOT NULL,
    first_name  TEXT     NOT NULL,
    last_name   TEXT     NOT NULL,
    gender      CHAR     NOT NULL check(gender="M" or gender="F"),    
    hire_date   DATE     NOT NULL,
    PRIMARY KEY (emp_id)
);
CREATE TABLE departments (
    dept_id     INTEGER  NOT NULL,
    dept_name   TEXT     NOT NULL,
    PRIMARY KEY (dept_id),
    CONSTRAINT dept_name_unique UNIQUE (dept_name)
);
CREATE TABLE dept_manager (
   dept_id      INTEGER  NOT NULL,
   emp_id       INTEGER  NOT NULL,
   from_date    DATE     NOT NULL,
   to_date      DATE     NOT NULL,
   FOREIGN KEY (emp_id)  REFERENCES employees (emp_id)    ON DELETE CASCADE,
   FOREIGN KEY (dept_id) REFERENCES departments (dept_id) ON DELETE CASCADE,
   PRIMARY KEY (emp_id, dept_id)
);
CREATE TABLE dept_emp (
    emp_id      INTEGER  NOT NULL,
    dept_id     INTEGER  NOT NULL,
    from_date   DATE     NOT NULL,
    to_date     DATE     NOT NULL,
    FOREIGN KEY (emp_id)  REFERENCES employees   (emp_id)  ON DELETE CASCADE,
    FOREIGN KEY (dept_id) REFERENCES departments (dept_id) ON DELETE CASCADE,
    PRIMARY KEY (emp_id, dept_id)
);
CREATE TABLE titles (
    emp_id      INTEGER  NOT NULL,
    title       TEXT     NOT NULL,
    from_date   DATE     NOT NULL,
    to_date     DATE,
    FOREIGN KEY (emp_id) REFERENCES employees (emp_id) ON DELETE CASCADE,
    PRIMARY KEY (emp_id,title, from_date)
);
CREATE TABLE salaries (
    emp_id      INTEGER         NOT NULL,
    salary      NUMBER          NOT NULL,
    from_date   DATE            NOT NULL,
    to_date     DATE            NOT NULL,
    FOREIGN KEY (emp_id) REFERENCES employees (emp_id) ON DELETE CASCADE,
    PRIMARY KEY (emp_id, from_date)
);
```

# test_db

A sample database with an integrated test suite, used to test your applications and database servers

This repository was migrated from [Launchpad](https://launchpad.net/test-db).

## Where it comes from

The original data was created by Fusheng Wang and Carlo Zaniolo at 
Siemens Corporate Research. The data is in XML format.
http://timecenter.cs.aau.dk/software.htm

Giuseppe Maxia made the relational schema and Patrick Crews exported
the data in relational format.

The database contains about 300,000 employee records with 2.8 million 
salary entries. The export data is 167 MB, which is not huge, but
heavy enough to be non-trivial for testing.

The data was generated, and as such there are inconsistencies and subtle
problems. Rather than removing them, we decided to leave the contents
untouched, and use these issues as data cleaning exercises.

## DISCLAIMER

To the best of my knowledge, this data is fabricated and
it does not correspond to real people. 
Any similarity to existing people is purely coincidental.

## LICENSE

This work is licensed under the 
Creative Commons Attribution-Share Alike 3.0 Unported License. 
To view a copy of this license, visit 
http://creativecommons.org/licenses/by-sa/3.0/ or send a letter to 
Creative Commons, 171 Second Street, Suite 300, San Francisco, 
California, 94105, USA.
