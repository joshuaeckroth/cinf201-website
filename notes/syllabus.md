---
title: Syllabus
layout: note
---

# Syllabus

CINF 201 - 01, Fall 2019 --- Database Systems

MWF 10-11a Davis 101 --- Pre-reqs: CSCI 111Q or 142 or 261.

This course introduces relational SQL databases and NoSQL databases, with an emphasis on the former. Students will learn how to create, read, update, and delete data with SQL queries and how to design databases so that they are efficient and secure. Our focus will be on practical techniques that can be immediately applied outside of the classroom.

## About me

- Joshua Eckroth, [jeckroth@stetson.edu](mailto:jeckroth@stetson.edu), [homepage](http://www2.stetson.edu/~jeckroth/)

- Eliz Hall 214, 386-740-2519

- Office hours: M 1:15-2, T 2-4, F 1-2

## Textbook

You will not need a textbook.

## Grading

- Attendance on work/demo days (usually Fridays): 5%
- Demos (2 required, 5 min each): 10%
- Homeworks (12 of them): 50%
- Quizzes (in class, on Wednesdays): 20%
- Group project: 15%

Late homework is penalized 20% for each day that it is
late. Submissions more than 3 days late receive no credit. Quizzes
cannot be made up unless a doctor's note is provided.

### Attendance

The plan is that every Friday will be a demo/work day. We will start
with demonstrations (you are required to give two demos during the
semester) and then proceed to work on the current homework or group
project. Attendance is required for these days to ensure that every
student is engaged with the material and making progress on
assignments.

The grading rubric for attendance is as follows, out of 3 points:

- attended at least 75% of work days: 3 pts
- attended at least 50% of work days: 2 pts
- attended at least 25% of work days: 1 pt
- attended fewer than 25% of work days: 0 pts

### Homework

Homework is assigned weekly until the group project starts. Most
assignments have a corresponding test script that you can use to check
your solution and I use to assist my grading.

The grading rubric for homework is as follows, out of 3 points:

- all tests pass (if tests are available) or solution meets all stated requirements: 3 pts
- at least 75% of tests pass or stated requirements are met: 2 pts
- at least 25% of tests pass or stated requirements are met: 1 pt
- no tests pass, no requirements met, or no submission: 0 pts

### Demos

You are required to give a 5 minute presentation on two separate
occassions about some new, interesting tool or technique relevant the
class as a whole.

The grading rubric for demos is as follows, out of 3 points:

- tool/technique is practical or interesting: +1 pt
- tool/technique is clearly presented: +1 pt
- tool/technique is novel: +1 pt

"Novel" means creative and new. A not-novel demo includes simple
variations of prior demos from this semester. Presenting the same
tool/technique as someone else will receive no credit.

### Quizzes

During some, but not all, weeks during the semester, an online quiz will be
given. The quiz will always take place on a Wednesday. The quiz will occur in
class but must be taken on Blackboard during the class time. The quizzes will
be short, requiring approximately 5-10 minutes to complete. They will always
have 5 questions. The grading rubric is simple: the number of points you receive
equals the number of questions you answer correctly.

### Group project

The group projects are to be determined.

**Group projects are due at the time of the final exam (there is no final exam).** Your attendance on that day is required so we can all view each other's group projects.

## Calendar

- Week 1: SQL queries for creating, reading, updating, deleting data
- Week 2: graphical tools for accessing databases
- Week 3: Database table design, normalization
- Week 4: Joins
- Week 5: Indexes, performance analysis
- Week 6: Aggregation
- Week 7: Constraints, transactions
- Week 8: Views, triggers
- Week 9: Data import and export
- Week 10: Security, password storage
- Week 11: NoSQL databases
- Week 12: NoSQL databases
- Week 13: Group projects
- Week 14: Group projects
- Week 15: Group projects
- Week 16: Group projects (presentations)

{% comment %}
Homework due dates:

<ul>
{% for p in site.pages sort_by:title order:ascending %}
{% if p.categories contains 'assignments' %}
<li>
<a href="{{ p.url }}">{{ p.title }}</a>, due {{ p.due }}
</li>
{% endif %}
{% endfor %}
</ul>

{% endcomment %}

## Honor code

I am strongly in agreement with the [Stetson University Honor Code](http://www.stetson.edu/other/honor-system/). Any form of cheating is not acceptable, will not be tolerated, and could lead to dismissal from the University.

## Academic success center

If a student anticipates barriers related to the format or requirements of a course, she or he should meet with the course instructor to discuss ways to ensure full participation. If disability-related accommodations are necessary, please register with the Academic Success Center (822-7127; [www.stetson.edu/asc](http://www.stetson.edu/asc)) and notify the course instructor of your eligibility for reasonable accommodations. The student, course instructor, and the Academic Success Center will plan how best to coordinate accommodations. The Academic Success Center is located at 209 E Bert Fish Drive, and can be contacted using the email address [asc@stetson.edu](mailto:asc@stetson.edu).


