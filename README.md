
#include "Course.h"
#include <iostream>
#include <iomanip>
using namespace std;

Course::Course()
    : courseCode("CS000"), courseName("Unknown"), creditHours(3),
      instructor(nullptr), maxCapacity(30),
      enrolledCount(0), waitListCount(0) {
    for (int i = 0; i < MAX_WAITLIST; i++) waitList[i] = nullptr;
}

Course::Course(const string& courseCode, const string& courseName,
               int creditHours, Faculty* instructor, int maxCapacity)
    : courseCode(courseCode), courseName(courseName),
      creditHours(creditHours), instructor(instructor),
      maxCapacity(maxCapacity), enrolledCount(0), waitListCount(0) {
    for (int i = 0; i < MAX_WAITLIST; i++) waitList[i] = nullptr;
}

void Course::enrollStudent(Student* student) {
    if (enrolledCount >= maxCapacity) {
        throw CapacityExceededException(courseCode, maxCapacity);
    }
    student->enrollCourse(courseCode);
    enrolledCount++;
}

void Course::addToWaitList(Student* student) {
    if (waitListCount < MAX_WAITLIST) {
        waitList[waitListCount++] = student;
        cout << "[Info] " << student->getName()
             << " added to waitlist for " << courseCode << "\n";
    } else {
        cout << "[Warning] Waitlist for " << courseCode << " is also full.\n";
    }
}

bool Course::operator==(const Course& other) const {
    return courseCode == other.courseCode;
}

ostream& operator<<(ostream& os, const Course& course) {
    os << "┌─────────────────────────────────┐\n";
    os << "│          COURSE DETAILS         │\n";
    os << "├─────────────────────────────────┤\n";
    os << "│ Code     : " << left << setw(22) << course.courseCode  << "│\n";
    os << "│ Name     : " << left << setw(22) << course.courseName  << "│\n";
    os << "│ Credits  : " << left << setw(22) << course.creditHours << "│\n";
    os << "│ Capacity : " << left << setw(22) << course.maxCapacity << "│\n";
    os << "│ Enrolled : " << left << setw(22) << course.enrolledCount << "│\n";
    if (course.instructor != nullptr) {
        os << "│ Instructor: " << left << setw(21)
           << course.instructor->getName() << "│\n";
    }
    os << "└─────────────────────────────────┘\n";
    return os;
}

Student** Course::operator+(const Course& other) const {
    int totalSize = waitListCount + other.waitListCount;
    if (totalSize == 0) return nullptr;

    Student** merged = new Student*[totalSize];
    for (int i = 0; i < waitListCount; i++)
        merged[i] = waitList[i];
    for (int i = 0; i < other.waitListCount; i++)
        merged[waitListCount + i] = other.waitList[i];

    return merged;
}

Student* Course::getWaitListEntry(int i) const {
    if (i >= 0 && i < waitListCount)
        return waitList[i];
    return nullptr;
}
