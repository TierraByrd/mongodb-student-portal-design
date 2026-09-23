# MongoDB Document Design – Student Success Hub

## Collection

One collection: **students**

Each student's advising notes and support visits are embedded directly inside their student document, instead of living in separate `notes` and `supportVisits` collections.

## Sample Document

```json
{
  "studentId": 1,
  "firstName": "Maria",
  "lastName": "Chen",
  "email": "maria.chen@example.edu",
  "program": "Computer Science",
  "notes": [
    {
      "noteId": 1,
      "date": "2026-09-05",
      "advisor": "Heather Combs",
      "category": "academic plan",
      "text": "Discussed course load for next term and adjusted registration plan."
    },
    {
      "noteId": 2,
      "date": "2026-09-12",
      "advisor": "Heather Combs",
      "category": "well-being",
      "text": "Checked in about balancing coursework with part-time job. Recommended time management workshop."
    }
  ],
  "supportVisits": [
    { "visitId": 1, "date": "2026-08-05", "type": "workshop", "description": "Attended study skills workshop." },
    { "visitId": 2, "date": "2026-09-10", "type": "tutoring", "description": "Tutoring session for Data Structures." },
    { "visitId": 3, "date": "2026-09-15", "type": "writing center", "description": "Writing center help with lab report." }
  ]
}
```

## Why Embed Instead of Reference

I chose to embed notes and support visits inside the student document rather than keeping them in separate collections. The three questions the hub needs to answer (a student's profile and recent notes, a student's recent visits, and which students have 3+ visits) are all centered on one student at a time, so embedding lets me get everything about a student in a single read instead of running multiple queries and joining the results myself. It also matches how an advisor actually works: they pull up one student and want to see their whole history at once, not separate lists they have to piece together. The tradeoff is that if a piece of shared information changed a lot (like an advisor's name), I'd have to update it inside every note where it appears, but since notes and visits are historical records tied to one point in time, that risk is low here. If this hub grew to the point where notes or visits needed to be queried independently of a student (across the whole school, for example), I'd lean toward separate collections referencing students by studentId instead.
