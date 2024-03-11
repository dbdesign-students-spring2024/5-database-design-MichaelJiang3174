# Data Normalization and Entity-Relationship Diagramming

## Original data set
| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |


## Non-compliance with 4NF

The original dataset contained various forms of redundancy, transitive dependencies, and multi-valued dependencies, which are not permitted in 4NF.

### Multi-Valued Dependencies
The dataset included fields that were dependent on each other but not on the primary key (such as professor, and professor email), violating the 4NF rule that a table must not contain more than one independent multi-valued relationship.

### Transitive Dependencies
Information such as assignment details was indirectly related to the primary key through another non-key attribute (assignment_id), leading to unnecessary duplication and potential for inconsistency.

### Redundancy
The same information about professors and assignment details was repeated across multiple records.

## Achieving 4NF Compliance

To make the dataset 4NF compliant, we undertook the following steps, creating separate tables for different entities to eliminate multi-valued and transitive dependencies:

### 1. Professors Table

Demonstrate the information of professor.

| professor_id | professor_name | professor_email      |
|--------------|----------------|----------------------|
| 1            | Melvin         | l.melvin@foo.edu     |
| 2            | Logston        | e.logston@foo.edu    |
| 3            | Nevarez        | i.nevarez@foo.edu    |
| ...          | ...            | ...                  |

### 2. Assignments Table

Demonstrate the information of assignment.

| assignment_id | assignment_topic                 | relevant_reading    |
|---------------|----------------------------------|---------------------|
| 1             | Data normalization               | Deumlich Chapter 3  |
| 2             | Single table queries             | Dümmlers Chapter 11 |
| 5             | Python and pandas                | Dümmlers Chapter 14 |
| 4             | Spreadsheet aggregate functions  | Zehnder Page 87     |
| ...           | ...                              | ...                 |

### 3. Courses Table

Demonstrate the information of the course.

| course_id | course_name    |
|-----------|----------------|
| 101       | Data Science   |
| 102       | Database Systems |
| ...       | ...              |

### 4. Classrooms Table

Demonstrate the location of the classroom.

| classroom_id | classroom_location |
|--------------|--------------------|
| 1            | WWH 101            |
| 2            | 60FA 314           |
| 3            | WWH 201            |
| ...          | ...                |

### 5. Sections Table

Connects courses, professors, and classrooms, clearly showing the information of each section.

| section_id | course_id | professor_id | classroom_id |
|------------|-----------|--------------|--------------|
| 1          | 101       | 1            | 1            |
| 2          | 102       | 2            | 2            |
| 3          | 102       | 3            | 3            |
| ...        | ...       | ...          | ...          |

### 6. Grades Table

Stores grades, linking them to students, assignments, and the sections where the assignment was given.

| student_id | section_id | assignment_id | grade | due_date  |
|------------|------------|---------------|-------|-----------|
| 1          | 1          | 1             | 80    | 23.02.21  |
| 7          | 2          | 2             | 25    | 18.11.21  |
| 4          | 1          | 1             | 75    | 23.02.21  |
| 2          | 2          | 5             | 92    | 05.05.21  |
| 2          | 3          | 4             | 65    | 04.07.21  |
| ...        | ...        | ...           | ...   | ...       |

## The ER diagram of 4NF-compliant version of the data set
![ER-diagram](./images/ER-diagram.png)

## Changes the data 4NF-compliant

### Create distinctive table
I restructured the dataset into six distinct tables: Professors, Assignments, Courses, Classrooms, Sections, and Grades. By thinking logically, I made sure that each table represented a unique aspect of the university's grade in course structure.

### Adding additional fields and introduce Primmary keys and Foreign keys
I add some additional fields to make sure that each table can include the necessary information. I introduced primary key to ensure that each record in a table can be uniquely identified. I introduced foreign keys to maintain the relationships between different aspects of the dataset.

### Eliminating redundancy and dependencies
I removed transitive dependencies to ensure that non-key attributes were only dependent on primary keys. By creating dedicated tables for each entity and defining relationships through foreign keys, I ensure each table not contain more than one independent multi-valued fact about an entity. The new structure significantly reduced redundancy by ensuring that information about professors, assignments, courses, grades, sections and classrooms was stored once and referenced elsewhere through foreign keys. 

