# 1. SQL Server Database for Latin School  

The **Latin School of San Diego** is a fictional language school that offers courses in Latin language and culture. It is a small school that has been in operation for about 8 years. They offer new classes on a quarterly basis. Up to now they have been using Excel workbooks to track course offerings, student enrollments and instructor payments. The process has become increasingly unwieldly and the school wants to upgrade to a database backend.  

We create a new **SQL Server** [database](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/sql_server_database_script.sql) and migrate the existing **Excel** [data](https://github.com/nabilshadman/sql-server-database-latin-school/tree/main/latin_school_data) into the database. There are two Excel workbooks, each covering about a 4 year period. We import both workbooks into the database.  

**Tech Stack:** T-SQL, SQL Server Management Studio, Excel  

Please refer to [this](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/requirements/project_requirements.pdf) document for the **requirements** of the project.  

# 2. Database Structure  
In this section, we discuss the **business logic** behind the database, where we identify the primary entities and the relationships between them.  

There are **4 primary entities** to be managed: Courses, Sections, Persons and Faculty. Course records describe the content of what will be taught in a course as well as the expected cost and duration. Section records are a subset of a Course record. They contain information on when and where a course will be taught as well as who will be teaching it. Person records contain information students enrolled in courses but also people who are members of the Latin School. Not all members enroll in courses. Faculty records contain information about the instructors who teach classes for the school.  

The **relationships** between these entities are as follows:  
- A Course can have multiple Sections, but a Section is related to only one Course
- A Section can have multiple Faculty and Faculty can teach multiple Sections
- A Section can have multiple Students and Students can be enrolled in multiple Sections
- Because there are 2 many-to-many relationships, a linking table will need to be created for each relationship  

There are 2 **lookup/crosswalk** tables that are related to the Section table:  
- A Term table that contains additional information about the term (aka quarter) in which a section is offered.
- A Room table that contains information about the location of a section.  

The school only captured a single address for a person before. The management wanted the ability to capture multiple addresses per person going forward. Each Address record now has an address type of “home” or “work”. All existing addresses are assigned a “home” value (per requirements).   

We provide an **Entity Relationship Diagram (ERD)** of the database in Figure 1.  


![entity_relationship_diagram](https://github.com/nabilshadman/sql-server-database-latin-school/assets/13073461/b6f69009-ec3e-4bdc-a716-d0a4694ef2b5)  
**Figure 1:** ERD of the database for latin school.  


# 3. Additional Database Objects  
After we create the tables and import the data from the Excel workbooks, we create a few **views** and **stored procedures** for reporting purposes:  

(1) We create a view that shows the number of times each Course has been offered in the history of the school.    
(2) We create a view that displays the gross revenue from tuition as well as faculty payments for each Academic Year.   
(3) We create a procedure that displays the course history for a selected person.   
(4) We create a procedure that adds a new person to the database.  


# 4. Project Deliverables  
At the completion of the project, we provide the following **documents** to the management:  

- Screenshots of the output from [view 1](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/view_1.jpg), [view 2](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/view_2.jpg), [procedure 3](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/procedure_1.jpg), and [procedure 4](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/procedure_2.jpg).  
- A [.sql](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/sql_server_database_script.sql) file script of the entire database and all database objects.  
- A screenshot of an entity relationship [diagram](https://github.com/nabilshadman/sql-server-database-latin-school/blob/main/sql_server_database/entity_relationship_diagram.jpg) showing all table and column names, and the relationships between tables.  

The management can use the following **scripts** to get the output for the views and stored procedures.  

``SELECT * FROM CourseRevenue_v ORDER BY CourseCode``   
``SELECT * FROM AnnualRevenue_v ORDER BY AcademicYear``  
``EXEC StudentHistory_p 1400``   
``EXEC InsertPerson_p 'LeBron','Jordan','work','3300 Chestnut St.','North Pole'``  
``SELECT TOP 1 * FROM Person ORDER BY PersonID DESC``   
``SELECT TOP 1 * FROM Address ORDER BY AddressID DESC``  





