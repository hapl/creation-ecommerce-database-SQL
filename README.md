# Final Project Transforming and Analyzing Data with SQL

## Project/Goals
The main goal is to with techniques and strategies for Transforming and Analyzing Data with SQL

## Process
### Creation of the ecommerce database
- Creation of tables using scripts that can be seen on the file [create_tables.md](Create_tables.md)
- Upload data using postgres **\COPY** option to import csv files. I also used the wizard from the Postgres GUI for some files.

### Review data integrity
- Check the table content to compare rows and check if the files imported correctly and have the right values.
- Review tables duplication and data cleaning details on [cleaning_data.md](cleaning_data.md).

### Reply questions related to data
-Reply questions related to data structure and values for more details go to [starting_with_questions.md](starting_with_questions.md).

### Follow all the questions in starting with data
- Reply all information need of this section. More information can be found under [starting_with_data.md](starting_with_data.md).

## QA
- Describe the QA process and queries used, more details on this file [QA.md](QA.md).

## Results
- The data allowed me to reply the questions related to the project.
- The details of those questions can be found under this two sections:
    - [starting_with_questions.md](starting_with_questions.md).
    - [starting_with_data.md](starting_with_data.md).

## Challenges 
- The challenges that were faced are:
    - The data type for each column on the CSV was not defined and had to change some tables after creation during import.
    - The values of unit price, revenue and related were multiplied by a million and had to change them in order to get better results.
    - The table analytics had millions of duplicated values that needed to be cleaned from the table to get better results.
 
## Future Goals
- Create primary keys on the tables.
- Create a relationship map and do normalization of the tables.
- Define restrictions to not allow NULL values in some important fields.
