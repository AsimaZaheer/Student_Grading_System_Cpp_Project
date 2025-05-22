# Student_Grading_System_Cpp_Project
PROJECT DESCRIPTION
📌 Project Title:
Student Grading System

🎯 Purpose:
To design a console-based application that:
Allows the user to input student details and marks
Calculates total marks and percentage
Assigns dynamic grades using a curve based on the highest percentage in the class
Identifies and displays the highest scorer
Saves and retrieves student data using file handling

🛠️ Key Features:
Student Data Entry:
Name
Marks for each subject (with validation)
Automatic Calculations:
Total Marks: Sum of all subject marks
Percentage: Based on max marks for each subject
Grade: Assigned dynamically using a grading curve:
A: ≥ 90% of highest
B: ≥ 80% of highest
C: ≥ 70% of highest
D: ≥ 60% of highest
F: < 60% of highest
Highest Scorer Detection:
Finds and displays the student with the highest total marks
File Handling:
Saves results to a CSV file (Student_Grading_System.csv)
Appends new entries without deleting old ones
Allows user to view past records
Input Validation:
Prevents entering empty names
Ensures marks are within valid range (not negative or exceeding max)
User Interface (Console-Based Menu):
Option 1: Enter new student data
Option 2: View saved data from file
Option 3: Exit the program

📂 File Output Format (CSV):
Each record includes:
Student Name
Marks for each subject with max marks in header
Total Marks
Percentage
Grade
Name and marks of highest scorer

🧮 Important Functions:
Function Name	Purpose
calculateTotal()	Sums marks of all subjects
calculatePercentage()	Calculates percentage from total marks and max marks
calculateGrade()	Assigns grade based on curve
HighestScoreFinder()	Finds student with highest total marks

🔄 Flow of Program:
Menu displayed to user.
Based on choice:
1 → Input data, calculate results, save to file, and display.
2 → Read from CSV file and display data.
3 → Exit program.
