# student-grade-tracker
import json
import os

# File to store grades
DATA_FILE = 'grades.json'

def load_grades():
    """Load grades from JSON file if it exists."""
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, 'r') as file:
            return json.load(file)
    return {}

def save_grades(grades):
    """Save grades to JSON file."""
    with open(DATA_FILE, 'w') as file:
        json.dump(grades, file, indent=4)

def add_student(grades):
    """Add a new student."""
    name = input("Enter student name: ").strip().title()
    if name in grades:
        print(f"Student '{name}' already exists!")
        return
    grades[name] = []
    print(f"Student '{name}' added successfully!")
    save_grades(grades)

def add_grade(grades):
    """Add a grade for a student."""
    name = input("Enter student name: ").strip().title()
    if name not in grades:
        print(f"Student '{name}' not found! Add the student first.")
        return
    try:
        grade = float(input("Enter grade (0-100): "))
        if 0 <= grade <= 100:
            grades[name].append(grade)
            print(f"Grade {grade} added for '{name}'!")
            save_grades(grades)
        else:
            print("Grade must be between 0 and 100!")
    except ValueError:
        print("Invalid grade! Enter a number.")

def view_student_grades(grades):
    """View grades and average for a student."""
    name = input("Enter student name: ").strip().title()
    if name not in grades or not grades[name]:
        print(f"No grades found for '{name}'!")
        return
    grades_list = grades[name]
    avg = sum(grades_list) / len(grades_list)
    print(f"\nGrades for '{name}': {grades_list}")
    print(f"Average: {avg:.2f}")
    print()

def list_all_students(grades):
    """List all students and their averages."""
    if not grades:
        print("No students added yet!")
        return
    print("\n=== All Students ===")
    for name, grade_list in grades.items():
        if grade_list:
            avg = sum(grade_list) / len(grade_list)
            print(f"{name}: Average = {avg:.2f} (Grades: {grade_list})")
        else:
            print(f"{name}: No grades yet")
    print()

def remove_student(grades):
    """Remove a student."""
    name = input("Enter student name to remove: ").strip().title()
    if name in grades:
        del grades[name]
        print(f"Student '{name}' removed!")
        save_grades(grades)
    else:
        print(f"Student '{name}' not found!")

def main():
    grades = load_grades()
    print("=== Student Grade Tracker ===")
    
    while True:
        print("\nOptions:")
        print("1. Add Student")
        print("2. Add Grade")
        print("3. View Student Grades")
        print("4. List All Students")
        print("5. Remove Student")
        print("6. Exit")
        
        choice = input("Enter your choice (1-6): ").strip()
        
        if choice == '1':
            add_student(grades)
        elif choice == '2':
            add_grade(grades)
        elif choice == '3':
            view_student_grades(grades)
        elif choice == '4':
            list_all_students(grades)
        elif choice == '5':
            remove_student(grades)
        elif choice == '6':
            print("Goodbye!")
            break
        else:
            print("Invalid choice! Please enter 1-6.")

if _name_ == "_main_":
    main()
