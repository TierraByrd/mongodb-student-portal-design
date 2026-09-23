Relational vs. Document Modeling in MongoDB

## Scenario

This project is for a Student Success Hub, a tool that would let advisors pull up a quick picture of a student's support history. It needs to track students, the advising notes advisors write after meetings, and support visits like tutoring, writing center appointments, or workshops. The hub should be able to answer three things: what a student's profile and recent notes look like, what support visits a student has had recently, and which students have had three or more support visits.

## Relational Design

The relational design uses three tables: Student, AdvisingNote, and SupportVisit. Student holds the core profile info (StudentID, name, email, program). AdvisingNote and SupportVisit both link back to Student through a StudentID foreign key, so every note and every visit has to belong to a real student. Full details and the column list are in `relational_design.md`.

## MongoDB Design

The MongoDB design uses one collection, `students`, with each student's notes and support visits embedded directly inside their document instead of living in separate collections. That keeps everything about one student in a single read, which matches how the required queries actually work (they're all centered on one student or a count per student). Full details and a sample document are in `mongo_design.md`.

## Comparison

The relational design feels strongest when correctness matters most. Since AdvisingNote and SupportVisit both reference Student through a foreign key, the database itself won't let a note or visit get attached to a student that doesn't exist, and it keeps every record in a consistent shape. That's valuable for data other people are going to rely on being accurate.

The MongoDB design feels strongest for the kind of reads advisors actually need. Getting a student's full profile, notes, and visit history takes one query instead of joining three tables together. It's also more forgiving if the shape of a note or visit needs to change later, since there's no rigid schema to redesign. The trade-off is that nothing stops a document from being incomplete or inconsistent unless the team builds that protection in themselves.

## How to Run

1. Make sure MongoDB Community Edition is installed and `mongod` is running locally.
2. Open `mongosh` (the Mongo shell).
3. Copy and run the commands in `week3_mongo_commands.txt` to create the `student_success` database and insert the sample student documents.
4. Copy and run the queries in `week3_mongo_queries.txt` to see the student summary, recent support visits, and high-engagement students results.

## AI Assistance

I used Claude to help me think through the trade-offs between embedding and referencing data in MongoDB, to work through the syntax and structure for the collections and aggregation queries, and to check my understanding of the CAP theorem and consistency concepts against the Week 3 lessons.
