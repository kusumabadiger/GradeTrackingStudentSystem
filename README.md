# Student Information and Grade Tracker

A console-based Java application that stores student records, accepts subject marks, computes CGPA and generates report cards. Built as a college project to demonstrate OOP, custom exceptions, a hand-written data structure/algorithm and the Java Collections framework.

## Features

- Add a student with USN, name, date of birth, phone number, primary address and email (all fields validated)
- Add or update marks for any subject (0-100)
- Automatic CGPA, total and percentage calculation
- Formatted report card for any student
- Display all student records in a table
- Rank list sorted by CGPA (highest first)
- Friendly error messages for bad input, invalid marks and unknown USNs

## Project Structure

```
GradeTracker/
├── README.md
├── .gitignore
└── src/
    ├── Main.java                      # Console menu and input validation
    ├── Person.java                    # Abstract base class (personal details)
    ├── Student.java                   # Student: marks, CGPA, report card
    ├── StudentManager.java            # Stores students, search, listing, ranking
    ├── InvalidMarksException.java     # Custom exception: marks outside 0-100
    └── StudentNotFoundException.java  # Custom exception: USN not found
```

## How to Run

**Requirements:** JDK 17 or later (check with `javac -version`).

From the project root folder:

```
javac -d out src/*.java
java -cp out Main
```

On Windows PowerShell, use `src\*.java` if the forward-slash version does not work. If your folder path has spaces, wrap it in quotes when using `cd`.

## Menu

```
========== STUDENT GRADE TRACKER ==========
 1. Add student
 2. Add marks
 3. Report card
 4. Display all records
 5. Rank list
 6. Exit
===========================================
```

### Input rules for "Add student"

| Field | Rule |
|---|---|
| USN | 5-15 letters/digits (stored in uppercase, must be unique) |
| Name | Letters, spaces and dots, 2-40 characters |
| DOB | Format `dd-MM-yyyy` |
| Phone | Exactly 10 digits |
| Primary address | 3-100 characters |
| Email | Valid format, e.g. `name@mail.com` |

## CGPA Calculation

Each subject's marks (out of 100) are converted to a grade point:

| Marks | Grade Point |
|---|---|
| 90 - 100 | 10 |
| 80 - 89 | 9 |
| 70 - 79 | 8 |
| 60 - 69 | 7 |
| 50 - 59 | 6 |
| 40 - 49 | 5 |
| Below 40 | 0 (FAIL) |

**CGPA = sum of grade points / number of subjects.**
Percentage = total marks / number of subjects.

## Where the Project Requirements Are Met

### OOP pillars

| Pillar | Where |
|---|---|
| Abstraction | `Person` is an abstract class with the abstract method `getRole()` |
| Encapsulation | Private fields with getters in `Person` and `Student`; marks can only be set through `addMarks()`, which validates them |
| Inheritance | `Student extends Person` and reuses the personal details fields |
| Polymorphism | `Student` overrides `getRole()` from `Person` |

### Custom exceptions

- `InvalidMarksException` is thrown by `Student.addMarks()` when marks are not between 0 and 100.
- `StudentNotFoundException` is thrown by `StudentManager.find()` when the USN does not exist.
- Both are handled with `try / catch` in `Main`.

### Data structure / algorithm

- **Insertion sort**, written by hand in `StudentManager.getRankList()`, ranks students by CGPA. It does not use `Collections.sort` or streams.

### Collections framework

- `LinkedHashMap<String, Student>` in `StudentManager` for storing students by USN and keeping insertion order
- `LinkedHashMap<String, Integer>` in `Student` for subject-to-marks storage
- `ArrayList<Student>` for listing and for the rank list

## Sample Output

```
==================================================================================================
No.  USN          Name                 DOB         Phone       Email                         CGPA
==================================================================================================
1    1MS22CS001   Asha Rao             12-05-2004  9876543210  asha@mail.com                 6.33
     Address: 12 MG Road, Bengaluru
2    1MS22CS002   Ravi K               03-11-2003  9123456780  ravi@mail.com                 7.50
     Address: 45 Park St, Mysuru
==================================================================================================
Total students: 2
```

```
======================================================
                    REPORT CARD
======================================================
USN       : 1MS22CS001
Name      : Asha Rao
DOB       : 12-05-2004
Phone     : 9876543210
Email     : asha@mail.com
Address   : 12 MG Road, Bengaluru
------------------------------------------------------
Subject                 Marks  Grade Point   Result
------------------------------------------------------
Maths                      95           10     PASS
Physics                    82            9     PASS
Chemistry                  38            0     FAIL
------------------------------------------------------
Total                     215 / 300
Percentage             71.67%
CGPA                     6.33
======================================================
```

## Team

| Name | USN | Contribution |
|---|---|---|
| _Member 1_ | _USN_ | _e.g. Person, Student_ |
| _Member 2_ | _USN_ | _e.g. StudentManager, sorting_ |
| _Member 3_ | _USN_ | _e.g. Main, validation_ |

## Possible Future Improvements

- Save and load records from a file so data is not lost on exit
- Search students by name
- Add a Teacher class that also extends `Person`
- Semester-wise marks and credits-based CGPA


