# SQL Server Database for Latin School

[![SQL Server](https://img.shields.io/badge/SQL%20Server-2019-CC2927?logo=microsoft-sql-server&logoColor=white)](https://www.microsoft.com/sql-server)
[![T-SQL](https://img.shields.io/badge/T--SQL-Latest-blue)](https://docs.microsoft.com/sql)
[![Excel](https://img.shields.io/badge/Excel-Compatible-217346?logo=microsoft-excel&logoColor=white)](https://products.office.com/excel)

## Overview

The **Latin School of San Diego** is a fictional language school that offers courses in Latin language and culture. It is a small school that has been in operation for about 8 years. They offer new classes on a quarterly basis. Up to now they have been using Excel workbooks to track course offerings, student enrollments and instructor payments. The process has become increasingly unwieldly and the school wants to upgrade to a database backend.

We create a new **SQL Server** [database](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/sql_server_database_script.sql) and migrate the existing **Excel** [data](https://github.com/nabilshadman/sql-server-database-latin-school/tree/main/latin_school_data) into the database. There are two Excel workbooks, each covering about a 4 year period. We import both workbooks into the database.

## Tech Stack

- **Database Engine:** Microsoft SQL Server
- **Development Language:** T-SQL
- **Development Environment:** SQL Server Management Studio (SSMS)
- **Data Migration Source:** Microsoft Excel

## Requirements

Please refer to [this](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/requirements/project_requirements.pdf) document for the detailed **requirements** of the project.

## Database Structure

### Business Logic and Core Entities

There are **4 primary entities** to be managed:

1. **Courses**
   - Describes content to be taught
   - Defines expected cost and duration
   
2. **Sections**
   - Subset of Course records
   - Contains scheduling and location information
   - Tracks teaching assignments

3. **Persons**
   - Student enrollment information
   - School membership records
   - Not all members enroll in courses

4. **Faculty**
   - Instructor information
   - Teaching assignments and records

### Entity Relationships

The **relationships** between these entities are as follows:
- A Course can have multiple Sections, but a Section is related to only one Course
- A Section can have multiple Faculty and Faculty can teach multiple Sections
- A Section can have multiple Students and Students can be enrolled in multiple Sections
- Because there are 2 many-to-many relationships, a linking table will need to be created for each relationship

### Supporting Tables

There are 2 **lookup/crosswalk** tables related to the Section table:
- A Term table that contains additional information about the term (aka quarter) in which a section is offered
- A Room table that contains information about the location of a section

### Address Management

The school only captured a single address for a person before. The management wanted the ability to capture multiple addresses per person going forward. Each Address record now has an address type of "home" or "work". All existing addresses are assigned a "home" value (per requirements).

### Entity Relationship Diagram

![entity_relationship_diagram](https://github.com/nabilshadman/sql-server-database-latin-school/assets/13073461/b6f69009-ec3e-4bdc-a716-d0a4694ef2b5)  
**Figure 1:** ERD of the database for latin school.

## Additional Database Objects

After creating the tables and importing the data from the Excel workbooks, we implement the following reporting tools:

### Views
- A **view** that shows the number of times each Course has been offered in the history of the school
- A **view** that displays the gross revenue from tuition as well as faculty payments for each Academic Year

### Stored Procedures
- A **procedure** that displays the course history for a selected person
- A **procedure** that adds a new person to the database

## Project Deliverables

We provide the following **documents** to the management:

### Screenshots and Documentation
- Screenshots of the output from [view 1](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/view_1.jpg), [view 2](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/view_2.jpg), [procedure 1](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/procedure_1.jpg), and [procedure 2](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/procedure_2.jpg)
- A complete [.sql](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/sql_server_database_script.sql) file script of the entire database and all database objects
- A screenshot of an entity relationship [diagram](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/entity_relationship_diagram.jpg) showing all table and column names, and the relationships between tables

### Usage Scripts

The management can use the following **scripts** to get the output for the views and stored procedures:

```sql
-- View course offerings history
SELECT * FROM CourseRevenue_v ORDER BY CourseCode

-- View annual revenue analysis
SELECT * FROM AnnualRevenue_v ORDER BY AcademicYear

-- Get student course history
EXEC StudentHistory_p 1400

-- Add new person with address
EXEC InsertPerson_p 'LeBron','Jordan','work','3300 Chestnut St.','North Pole'

-- Verify new person and address records
SELECT TOP 1 * FROM Person ORDER BY PersonID DESC
SELECT TOP 1 * FROM Address ORDER BY AddressID DESC
```

## Getting Started

1. Execute the complete database script to create the schema
2. Run validation queries to verify table structure
3. Import historical data using provided migration scripts
4. Verify data integrity using built-in views
5. Test stored procedures with sample data

## Contributing

We welcome contributions to improve the database schema or add new features. Please submit pull requests or open issues for any enhancements. 

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE.txt) file for details.  
