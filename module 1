
#include "Enrollment.h"
#include <iostream>
#include <iomanip>
using namespace std;

Enrollment::Enrollment()
    : student(nullptr), course(nullptr),
      enrollmentDate("N/A"), grade("N/A") {}

Enrollment::Enrollment(Student* student, Course* course,
                       const string& enrollmentDate)
    : student(student), course(course),
      enrollmentDate(enrollmentDate), grade("N/A") {}

void Enrollment::displayEnrollment() const {
    cout << "┌─────────────────────────────────┐\n";
    cout << "│        ENROLLMENT RECORD        │\n";
    cout << "├─────────────────────────────────┤\n";
    if (student)
        cout << "│ Student : " << left << setw(23) << student->getName() << "│\n";
    if (course)
        cout << "│ Course  : " << left << setw(23) << course->getCourseName() << "│\n";
    cout << "│ Date    : " << left << setw(23) << enrollmentDate << "│\n";
    cout << "│ Grade   : " << left << setw(23) << grade          << "│\n";
    cout << "└─────────────────────────────────┘\n";
}
