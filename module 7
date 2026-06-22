
#include <iostream>
#include <iomanip>
#include <algorithm>
using namespace std;

#include "person/Person.h"
#include "person/Student.h"
#include "person/GradStudent.h"
#include "person/Faculty.h"
#include "person/Staff.h"
#include "course/Course.h"
#include "course/Enrollment.h"
#include "library/Library.h"
#include "finance/FeeRecord.h"
#include "finance/Invoice.h"
#include "hostel/HostelManager.h"
#include "utils/Exceptions.h"
#include "utils/Utils.h"
#include "utils/Reports.h"

const int MAX_STUDENTS   = 50;
const int MAX_FACULTY    = 20;
const int MAX_STAFF      = 20;
const int MAX_COURSES    = 30;
const int MAX_ENROLLMENTS= 100;
const int MAX_FEE_RECORDS= 50;

Student*    students[MAX_STUDENTS];
Faculty*    faculty[MAX_FACULTY];
Staff*      staffMembers[MAX_STAFF];
Course*     courses[MAX_COURSES];
Enrollment* enrollments[MAX_ENROLLMENTS];
FeeRecord   feeRecords[MAX_FEE_RECORDS];

int studentCount    = 0;
int facultyCount    = 0;
int staffCount      = 0;
int courseCount     = 0;
int enrollmentCount = 0;
int feeRecordCount  = 0;

Library      library("data/library_catalog.txt");
HostelManager hostelMgr("HITEC University Hostel");

void seedDemoData();
void cleanupMemory();

void menuMain();
void menuPersonModule();
void menuCourseModule();
void menuLibraryModule();
void menuFinanceModule();
void menuHostelModule();
void menuReportModule();

int main() {
    Utils::printHeader("SMART CAMPUS MANAGEMENT SYSTEM");
    cout << "  HITEC University Taxila\n";
    cout << "  CS-104L: Object-Oriented Programming\n\n";

    library.loadFromFile();

    seedDemoData();

    menuMain();
    
    library.saveToFile();
    Reports::saveCampusReportToFile(
        "data/campus_report.txt",
        students, studentCount,
        faculty,  facultyCount,
        library
    );

    cleanupMemory();

    cout << "\n[SCMS] Session ended. All data saved. Goodbye!\n";
    return 0;
}

void seedDemoData() {
    cout << "\n[SCMS] Loading demo data...\n";

    faculty[facultyCount++] = new Faculty(
        "Dr. Ahmed Khan", "35202-1234567-1", 45, "0300-1111111",
        "F001", "Computer Science", "Associate Professor");

    faculty[facultyCount++] = new Faculty(
        "Ms. Sara Malik", "35202-7654321-2", 35, "0300-2222222",
        "F002", "Computer Science", "Lecturer");

    students[studentCount++] = new Student(
        "Ali Hassan", "35202-1111111-3", 20, "0311-1111111",
        "2023-CS-001", 3, 3.8);

    students[studentCount++] = new Student(
        "Fatima Raza", "35202-2222222-4", 21, "0311-2222222",
        "2023-CS-002", 3, 3.5);

    students[studentCount++] = new Student(
        "Usman Tariq", "35202-3333333-5", 20, "0311-3333333",
        "2023-CS-003", 2, 2.9);

    students[studentCount++] = new GradStudent(
        "Dr. Bilal Qureshi", "35202-4444444-6", 26, "0311-4444444",
        "2022-MS-001", 7, 3.9,
        "Deep Learning for NLP", "Dr. Ahmed Khan", "Artificial Intelligence");

    staffMembers[staffCount++] = new Staff(
        "Rashid Gardener", "35202-5555555-7", 40, "0300-5555555",
        "ST001", "Grounds Staff", 35000.0);

    courses[courseCount++] = new Course(
        "CS-104L", "Object-Oriented Programming",
        3, faculty[0], 30);

    courses[courseCount++] = new Course(
        "CS-201", "Data Structures and Algorithms",
        3, faculty[1], 25);

    try {
        courses[0]->enrollStudent(students[0]);
        courses[0]->enrollStudent(students[1]);
        courses[1]->enrollStudent(students[2]);
        enrollments[enrollmentCount++] = new Enrollment(
            students[0], courses[0], "2025-01-15");
        enrollments[enrollmentCount++] = new Enrollment(
            students[1], courses[0], "2025-01-15");
    } catch (const CapacityExceededException& e) {
        cout << "[Error] " << e.what() << "\n";
    }
    
    library.addItem(new Book(
        "LIB001", "C++ How to Program", "Deitel & Deitel",
        2017, "978-0134448237", "Computer Science", 3));

    library.addItem(new Book(
        "LIB002", "The C++ Programming Language",
        "Bjarne Stroustrup", 2013, "978-0321563842",
        "Computer Science", 2));

    library.addItem(new Journal(
        "LIB003", "IEEE Transactions on Software Engineering",
        "IEEE", 2024, "0098-5589", 50, 1));

    feeRecords[feeRecordCount++] = FeeRecord(students[0], 45000.0, 15000.0);
    feeRecords[feeRecordCount++] = FeeRecord(students[1], 45000.0, 0.0);
    feeRecords[feeRecordCount++] = FeeRecord(students[2], 45000.0, 15000.0);

    feeRecords[0] -= 30000.0;
    hostelMgr.addBlock("Block-A");
    hostelMgr.addBlock("Block-B");
    hostelMgr.addRoomToBlock(0, 101, "double", 1);
    hostelMgr.addRoomToBlock(0, 102, "single", 1);
    hostelMgr.addRoomToBlock(1, 201, "triple", 2);
    hostelMgr.addRoomToBlock(1, 202, "double", 2);
    hostelMgr.allocateRoom(students[0]);
    hostelMgr.allocateRoom(students[2]);

    cout << "[SCMS] Demo data loaded successfully.\n";
}

void cleanupMemory() {
    for (int i = 0; i < studentCount;    i++) { delete students[i];    students[i]    = nullptr; }
    for (int i = 0; i < facultyCount;    i++) { delete faculty[i];     faculty[i]     = nullptr; }
    for (int i = 0; i < staffCount;      i++) { delete staffMembers[i];staffMembers[i]= nullptr; }
    for (int i = 0; i < courseCount;     i++) { delete courses[i];     courses[i]     = nullptr; }
    for (int i = 0; i < enrollmentCount; i++) { delete enrollments[i]; enrollments[i] = nullptr; }
}

void menuMain() {
    int choice = 0;
    while (choice != 7) {
        Utils::printHeader("MAIN MENU — SCMS");
        cout << "  1. Person Management    (Module 1)\n";
        cout << "  2. Course & Enrollment  (Module 2)\n";
        cout << "  3. Library System       (Module 3)\n";
        cout << "  4. Fee & Finance        (Module 4)\n";
        cout << "  5. Hostel Management    (Module 5)\n";
        cout << "  6. Reports & Utilities  (Module 6)\n";
        cout << "  7. Exit\n";
        choice = Utils::getIntInput("  Enter choice: ");

        switch (choice) {
            case 1: menuPersonModule();  break;
            case 2: menuCourseModule();  break;
            case 3: menuLibraryModule(); break;
            case 4: menuFinanceModule(); break;
            case 5: menuHostelModule();  break;
            case 6: menuReportModule();  break;
            case 7: break;
            default: cout << "  Invalid option.\n";
        }
    }
}

void menuPersonModule() {
    int choice = 0;
    while (choice != 6) {
        Utils::printHeader("MODULE 1 — PERSON MANAGEMENT");
        cout << "  1. Add Student\n";
        cout << "  2. Add Graduate Student (multi-level inheritance)\n";
        cout << "  3. Add Faculty\n";
        cout << "  4. Add Staff\n";
        cout << "  5. Display All (polymorphism demo)\n";
        cout << "  6. Back\n";
        choice = Utils::getIntInput("  Enter choice: ");

        if (choice == 1) {
            if (studentCount >= MAX_STUDENTS) {
                cout << "  Storage full.\n"; continue;
            }
            string name    = Utils::getStringInput("  Name: ");
            string cnic    = Utils::getStringInput("  CNIC: ");
            int    age     = Utils::getIntInput("  Age: ");
            string contact = Utils::getStringInput("  Contact: ");
            string rollNo  = Utils::getStringInput("  Roll No: ");
            int    sem     = Utils::getIntInput("  Semester (1-8): ");
            double gpa     = Utils::getDoubleInput("  GPA (0.0-4.0): ");
            students[studentCount++] = new Student(
                name, cnic, age, contact, rollNo, sem, gpa);
            cout << "  Student added successfully.\n";

        } else if (choice == 2) {
            if (studentCount >= MAX_STUDENTS) {
                cout << "  Storage full.\n"; continue;
            }
            string name    = Utils::getStringInput("  Name: ");
            string cnic    = Utils::getStringInput("  CNIC: ");
            int    age     = Utils::getIntInput("  Age: ");
            string contact = Utils::getStringInput("  Contact: ");
            string rollNo  = Utils::getStringInput("  Roll No: ");
            int    sem     = Utils::getIntInput("  Semester: ");
            double gpa     = Utils::getDoubleInput("  GPA: ");
            string thesis  = Utils::getStringInput("  Thesis Title: ");
            string supv    = Utils::getStringInput("  Supervisor: ");
            string area    = Utils::getStringInput("  Research Area: ");
            students[studentCount++] = new GradStudent(
                name, cnic, age, contact, rollNo, sem, gpa,
                thesis, supv, area);
            cout << "  Graduate student added.\n";

        } else if (choice == 3) {
            if (facultyCount >= MAX_FACULTY) {
                cout << "  Storage full.\n"; continue;
            }
            string name  = Utils::getStringInput("  Name: ");
            string cnic  = Utils::getStringInput("  CNIC: ");
            int    age   = Utils::getIntInput("  Age: ");
            string cont  = Utils::getStringInput("  Contact: ");
            string empID = Utils::getStringInput("  Employee ID: ");
            string dept  = Utils::getStringInput("  Department: ");
            string desig = Utils::getStringInput("  Designation: ");
            faculty[facultyCount++] = new Faculty(
                name, cnic, age, cont, empID, dept, desig);
            cout << "  Faculty added.\n";

        } else if (choice == 4) {
            if (staffCount >= MAX_STAFF) {
                cout << "  Storage full.\n"; continue;
            }
            string name    = Utils::getStringInput("  Name: ");
            string cnic    = Utils::getStringInput("  CNIC: ");
            int    age     = Utils::getIntInput("  Age: ");
            string contact = Utils::getStringInput("  Contact: ");
            string staffID = Utils::getStringInput("  Staff ID: ");
            string role    = Utils::getStringInput("  Role: ");
            double salary  = Utils::getDoubleInput("  Salary: ");
            staffMembers[staffCount++] = new Staff(
                name, cnic, age, contact, staffID, role, salary);
            cout << "  Staff added.\n";

        } else if (choice == 5) {
            Utils::printHeader("ALL PERSONS (Polymorphism Demo)");
            cout << "\n--- STUDENTS ---\n";
            for (int i = 0; i < studentCount; i++) {
                Person* p = students[i];
                p->displayInfo();     
            }
            cout << "\n--- FACULTY ---\n";
            for (int i = 0; i < facultyCount; i++) {
                Person* p = faculty[i];
                p->displayInfo();
            }
            cout << "\n--- STAFF ---\n";
            for (int i = 0; i < staffCount; i++) {
                Person* p = staffMembers[i];
                p->displayInfo();
            }
        }
    }
}

void menuCourseModule() {
    int choice = 0;
    while (choice != 6) {
        Utils::printHeader("MODULE 2 — COURSE & ENROLLMENT");
        cout << "  1. Add Course\n";
        cout << "  2. Display All Courses (operator<<)\n";
        cout << "  3. Enroll Student in Course\n";
        cout << "  4. Compare Two Courses (operator==)\n";
        cout << "  5. Merge Waiting Lists (operator+)\n";
        cout << "  6. Back\n";
        choice = Utils::getIntInput("  Enter choice: ");

        if (choice == 1) {
            if (courseCount >= MAX_COURSES) {
                cout << "  Storage full.\n"; continue;
            }
            string code  = Utils::getStringInput("  Course Code: ");
            string name  = Utils::getStringInput("  Course Name: ");
            int    creds = Utils::getIntInput("  Credit Hours: ");
            int    cap   = Utils::getIntInput("  Max Capacity: ");

            cout << "  Available Faculty:\n";
            for (int i = 0; i < facultyCount; i++)
                cout << "  " << i << ". " << faculty[i]->getName() << "\n";
            int fi = Utils::getIntInput("  Select Faculty index: ");
            Faculty* instr = (fi >= 0 && fi < facultyCount) ? faculty[fi] : nullptr;

            courses[courseCount++] = new Course(code, name, creds, instr, cap);
            cout << "  Course added.\n";

        } else if (choice == 2) {
            cout << "\n";
            for (int i = 0; i < courseCount; i++)
                cout << *courses[i]; 

        } else if (choice == 3) {
            if (studentCount == 0 || courseCount == 0) {
                cout << "  No students or courses available.\n"; continue;
            }
            cout << "  Students:\n";
            for (int i = 0; i < studentCount; i++)
                cout << "  " << i << ". " << students[i]->getName() << "\n";
            int si = Utils::getIntInput("  Student index: ");

            cout << "  Courses:\n";
            for (int i = 0; i < courseCount; i++)
                cout << "  " << i << ". " << courses[i]->getCourseCode() << "\n";
            int ci = Utils::getIntInput("  Course index: ");

            if (si < 0 || si >= studentCount || ci < 0 || ci >= courseCount) {
                cout << "  Invalid selection.\n"; continue;
            }
            try {
                courses[ci]->enrollStudent(students[si]);
                if (enrollmentCount < MAX_ENROLLMENTS) {
                    enrollments[enrollmentCount++] = new Enrollment(
                        students[si], courses[ci], Utils::getCurrentDate());
                }
                cout << "  Enrollment successful.\n";
            } catch (const CapacityExceededException& e) {
                cout << "  [Exception] " << e.what() << "\n";
                cout << "  Adding to waitlist instead...\n";
                courses[ci]->addToWaitList(students[si]);
            }

        } else if (choice == 4) {
            if (courseCount < 2) { cout << "  Need at least 2 courses.\n"; continue; }
            int a = Utils::getIntInput("  First course index: ");
            int b = Utils::getIntInput("  Second course index: ");
            if (a < 0 || a >= courseCount || b < 0 || b >= courseCount) {
                cout << "  Invalid.\n"; continue;
            }
            bool equal = (*courses[a] == *courses[b]);
            cout << "  " << courses[a]->getCourseCode() << " == "
                 << courses[b]->getCourseCode() << " ? "
                 << (equal ? "YES (same course)" : "NO (different courses)") << "\n";

        } else if (choice == 5) {
            if (courseCount < 2) { cout << "  Need at least 2 courses.\n"; continue; }
            int a = Utils::getIntInput("  First course index: ");
            int b = Utils::getIntInput("  Second course index: ");
            if (a < 0 || a >= courseCount || b < 0 || b >= courseCount) {
                cout << "  Invalid.\n"; continue;
            }
            
            Student** merged = *courses[a] + *courses[b];
            int total = courses[a]->getWaitListCount() +
                        courses[b]->getWaitListCount();
            cout << "  Merged waitlist (" << total << " students):\n";
            for (int i = 0; i < total; i++) {
                if (merged && merged[i])
                    cout << "  - " << merged[i]->getName() << "\n";
            }
            delete[] merged; 
        }
    }
}

void menuLibraryModule() {
    int choice = 0;
    while (choice != 7) {
        Utils::printHeader("MODULE 3 — LIBRARY SYSTEM");
        cout << "  1. Add Book\n";
        cout << "  2. Add Journal\n";
        cout << "  3. Search by Title\n";
        cout << "  4. Issue Item to Student\n";
        cout << "  5. Return Item (overdue check)\n";
        cout << "  6. Display Catalog\n";
        cout << "  7. Back\n";
        choice = Utils::getIntInput("  Enter choice: ");

        if (choice == 1) {
            string id    = Utils::getStringInput("  Item ID: ");
            string title = Utils::getStringInput("  Title: ");
            string auth  = Utils::getStringInput("  Author: ");
            int    year  = Utils::getIntInput("  Publication Year: ");
            string isbn  = Utils::getStringInput("  ISBN: ");
            string genre = Utils::getStringInput("  Genre: ");
            int    copies= Utils::getIntInput("  Copies Available: ");
            library.addItem(new Book(id, title, auth, year, isbn, genre, copies));
            cout << "  Book added.\n";

        } else if (choice == 2) {
            string id     = Utils::getStringInput("  Item ID: ");
            string title  = Utils::getStringInput("  Title: ");
            string auth   = Utils::getStringInput("  Author: ");
            int    year   = Utils::getIntInput("  Publication Year: ");
            string issn   = Utils::getStringInput("  ISSN: ");
            int    vol    = Utils::getIntInput("  Volume: ");
            int    issue  = Utils::getIntInput("  Issue Number: ");
            library.addItem(new Journal(id, title, auth, year, issn, vol, issue));
            cout << "  Journal added.\n";

        } else if (choice == 3) {
            string title = Utils::getStringInput("  Search title: ");
            LibraryItem* found = library.searchByTitle(title);
            if (found) {
                cout << "  Found!\n";
                found->displayInfo(); 
            } else {
                cout << "  Item not found.\n";
            }

        } else if (choice == 4) {
            string rollNo = Utils::getStringInput("  Student Roll No: ");
            string itemID = Utils::getStringInput("  Item ID: ");
            try {
                library.issueItem(rollNo, itemID, Utils::getCurrentDate());
            } catch (const ItemNotFoundException& e) {
                cout << "  [Exception] " << e.what() << "\n";
            }

        } else if (choice == 5) {
            string rollNo = Utils::getStringInput("  Student Roll No: ");
            string itemID = Utils::getStringInput("  Item ID: ");
            int    days   = Utils::getIntInput("  Days kept: ");
            try {
                library.returnItem(rollNo, itemID, days);
                cout << "  Item returned successfully.\n";
            } catch (const OverdueException& e) {
                cout << "  [Overdue] " << e.what() << "\n";
                cout << "  Fine: Rs. " << e.getFineAmount() << "\n";
            } catch (const ItemNotFoundException& e) {
                cout << "  [Exception] " << e.what() << "\n";
            }

        } else if (choice == 6) {
            library.displayCatalog();
        }
    }
}

void menuFinanceModule() {
    int choice = 0;
    while (choice != 6) {
        Utils::printHeader("MODULE 4 — FEE & FINANCE");
        cout << "  1. Create Fee Record for Student\n";
        cout << "  2. Record Payment (operator-=)\n";
        cout << "  3. Display Fee Records\n";
        cout << "  4. Generate Invoice (static counter demo)\n";
        cout << "  5. Copy Fee Record (copy constructor demo)\n";
        cout << "  6. Back\n";
        choice = Utils::getIntInput("  Enter choice: ");

        if (choice == 1) {
            if (feeRecordCount >= MAX_FEE_RECORDS) {
                cout << "  Storage full.\n"; continue;
            }
            if (studentCount == 0) { cout << "  No students.\n"; continue; }
            for (int i = 0; i < studentCount; i++)
                cout << "  " << i << ". " << students[i]->getName() << "\n";
            int si    = Utils::getIntInput("  Student index: ");
            double sf = Utils::getDoubleInput("  Semester Fee: ");
            double hf = Utils::getDoubleInput("  Hostel Fee (0 if none): ");
            feeRecords[feeRecordCount++] = FeeRecord(students[si], sf, hf);
            cout << "  Fee record created.\n";

        } else if (choice == 2) {
            if (feeRecordCount == 0) { cout << "  No records.\n"; continue; }
            for (int i = 0; i < feeRecordCount; i++) {
                Student* s = feeRecords[i].getStudent();
                cout << "  " << i << ". "
                     << (s ? s->getName() : "N/A")
                     << " | Balance: Rs. "
                     << feeRecords[i].getBalance() << "\n";
            }
            int idx = Utils::getIntInput("  Select record: ");
            if (idx < 0 || idx >= feeRecordCount) { cout << "  Invalid.\n"; continue; }
            double amt = Utils::getDoubleInput("  Payment amount: ");
            feeRecords[idx] -= amt; 

        } else if (choice == 3) {
            Reports::displayAllFeeRecords(feeRecords, feeRecordCount);
            Reports::reportOutstandingBalances(feeRecords, feeRecordCount);

        } else if (choice == 4) {
            Invoice inv1(Utils::getCurrentDate());
            inv1.addItem("Semester Fee", 45000.0);
            inv1.addItem("Library Fee", 500.0);
            cout << inv1; 

            Invoice inv2 = inv1; 
            inv2.addItem("Hostel Fee", 15000.0);
            cout << "\n[Copy of Invoice with extra item]\n";
            cout << inv2;

            cout << "\n  Total invoices generated: "
                 << Invoice::getInvoiceCount() << "\n";

        } else if (choice == 5) {
            if (feeRecordCount == 0) { cout << "  No records.\n"; continue; }
            int idx = Utils::getIntInput("  Record to copy (index): ");
            if (idx < 0 || idx >= feeRecordCount) { cout << "  Invalid.\n"; continue; }
            FeeRecord copy = feeRecords[idx]; 
            cout << "\n  Original:\n";
            feeRecords[idx].displayFeeRecord();
            cout << "\n  Copy:\n";
            copy.displayFeeRecord();
        }
    }
}

void menuHostelModule() {
    int choice = 0;
    while (choice != 5) {
        Utils::printHeader("MODULE 5 — HOSTEL MANAGEMENT");
        cout << "  1. Add Hostel Block\n";
        cout << "  2. Add Room to Block\n";
        cout << "  3. Allocate Room to Student\n";
        cout << "  4. Vacate Room\n";
        cout << "  5. Back\n";
        choice = Utils::getIntInput("  Enter choice: ");

        if (choice == 1) {
            string bname = Utils::getStringInput("  Block Name: ");
            hostelMgr.addBlock(bname);

        } else if (choice == 2) {
            int blockIdx = Utils::getIntInput("  Block index (0-based): ");
            int roomNo   = Utils::getIntInput("  Room Number: ");
            string type  = Utils::getStringInput("  Type (single/double/triple): ");
            int floor    = Utils::getIntInput("  Floor: ");
            hostelMgr.addRoomToBlock(blockIdx, roomNo, type, floor);

        } else if (choice == 3) {
            if (studentCount == 0) { cout << "  No students.\n"; continue; }
            for (int i = 0; i < studentCount; i++)
                cout << "  " << i << ". " << students[i]->getName() << "\n";
            int si = Utils::getIntInput("  Student index: ");
            if (si < 0 || si >= studentCount) { cout << "  Invalid.\n"; continue; }
            hostelMgr.allocateRoom(students[si]);

        } else if (choice == 4) {
            string rollNo = Utils::getStringInput("  Student Roll No: ");
            int    roomNo = Utils::getIntInput("  Room Number: ");
            hostelMgr.vacateRoom(rollNo, roomNo);
        }
    }
    hostelMgr.generateReport();
}

void menuReportModule() {
    int choice = 0;
    while (choice != 7) {
        Utils::printHeader("MODULE 6 — REPORTS & UTILITIES");
        cout << "  1. Sort Students by GPA (std::sort)\n";
        cout << "  2. Find Student by Roll No (std::find_if)\n";
        cout << "  3. Library Summary\n";
        cout << "  4. Hostel Occupancy Report\n";
        cout << "  5. Outstanding Fee Balances\n";
        cout << "  6. Full Campus Report\n";
        cout << "  7. Back\n";
        choice = Utils::getIntInput("  Enter choice: ");

        if (choice == 1) {
            Reports::sortStudentsByGPA(students, studentCount);

        } else if (choice == 2) {
            string rollNo = Utils::getStringInput("  Enter Roll No to find: ");
            Student* found = Reports::findStudentByRollNo(
                students, studentCount, rollNo);
            if (found) {
                cout << "  Found!\n";
                found->displayInfo();
            } else {
                cout << "  Student not found.\n";
            }

        } else if (choice == 3) {
            Reports::displayLibrarySummary(library);

        } else if (choice == 4) {
            Reports::displayHostelReport(hostelMgr);

        } else if (choice == 5) {
            Reports::reportOutstandingBalances(feeRecords, feeRecordCount);

        } else if (choice == 6) {
            Reports::generateCampusReport(
                students, studentCount,
                faculty,  facultyCount,
                library,
                feeRecords, feeRecordCount,
                hostelMgr
            );
        }
    }
}
