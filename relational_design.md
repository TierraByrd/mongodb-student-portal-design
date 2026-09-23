# Relational Design – Student Success Hub

## Tables

### Student
| Column | Type | Notes |
|---|---|---|
| StudentID | INTEGER | Primary Key |
| FirstName | TEXT | |
| LastName | TEXT | |
| Email | TEXT | |
| Program | TEXT | student's major/program |

### AdvisingNote
| Column | Type | Notes |
|---|---|---|
| NoteID | INTEGER | Primary Key |
| StudentID | INTEGER | Foreign Key -> Student.StudentID |
| Date | DATE | date of the note |
| AdvisorName | TEXT | advisor who wrote it |
| Category | TEXT | e.g. "academic plan", "well-being", "career" |
| NoteText | TEXT | free-text note content |

### SupportVisit
| Column | Type | Notes |
|---|---|---|
| VisitID | INTEGER | Primary Key |
| StudentID | INTEGER | Foreign Key -> Student.StudentID |
| Date | DATE | date of the visit |
| ServiceType | TEXT | e.g. "tutoring", "writing center", "workshop" |
| Description | TEXT | short description of the visit |

## Relationships

- AdvisingNote.StudentID references Student.StudentID (one student can have many advising notes)
- SupportVisit.StudentID references Student.StudentID (one student can have many support visits)

Both AdvisingNote and SupportVisit are child tables of Student. A student can have zero or many notes and zero or many visits, but every note and every visit has to belong to exactly one student. That's the kind of rule a relational database enforces automatically through foreign keys, so a note or visit can never be orphaned or point to a student that doesn't exist.
