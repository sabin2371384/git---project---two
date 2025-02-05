#include <iostream>
#include <string>
#include <vector>
#include <algorithm>
#include <iomanip>
#include <numeric>

در ابتدای برنامه کتابخانه های مختلفی را قرار دادم که کاربرد های زیر را دارد.
#include <iostream>: 
برای ورودی و خروجی استاندارد 
#include <string>:
 برای استفاده از نوع داده string
#include <vector>: 
برای استفاده از نوع داده vector که یک آرایه دینامیک است.
#include <algorithm>: 
برای استفاده از توابع الگوریتمی 
#include <iomanip>:
 برای فرمت‌دهی خروجی
 (مانند تنظیم دقت اعشار)
#include <numeric>:
 برای استفاده از توابع جمع و محاسبات عددی
 (مانند accumulate)

struct Student {
    string name;
    string studentID;
    string major;
    vector<pair<string, double>> courses;
    double GPA;
};
از   struct Student که
یک ساختار است برای ذخیره اطلاعات دانشجویان شامل نام، شناسه دانشجویی، رشته تحصیلی، دروس و نمرات آن‌ها و معدل (GPA) استفاده کردم .
void registerStudent(vector<Student> &students);
void registerCourse(vector<Student> &students);
void displayStudentList(const vector<Student> &students);
void displayStudentList(const vector<Student> &students, const string &major);
void generateGradeSheet(const vector<Student> &students, const string &studentID);
در این بخش توابعی را برای ثبت نام دانشجو، ثبت نام دروس، نمایش لیست دانشجویان و تولید کارنامه تعریف شده‌ قرار دادم.
void registerStudent(vector<Student> &students) {
    Student newStudent;
    cout << "Enter student name: ";
    cin >> newStudent.name;
    cout << "Enter student ID: ";
    cin >> newStudent.studentID;
    cout << "Enter student major: ";
    cin >> newStudent.major;
    newStudent.GPA = 0.0; 
    students.push_back(newStudent);
    cout << "Student registered successfully!\n";
}
در این بخش توابعی را قرار دادم که اطلاعات یک دانشجو را از کاربر می‌گیرد و آن را به لیست دانشجویان اضافه می‌کند.
void registerCourse(vector<Student> &students) {
    string studentID;
    cout << "Enter student ID: ";
    cin >> studentID;
    for (auto &student : students) {
        if (student.studentID == studentID) {
            string courseName;
            double courseGrade;
            cout << "Enter course name: ";
            cin >> courseName;
            cout << "Enter course grade: ";
            cin >> courseGrade;
            student.courses.push_back(make_pair(courseName, courseGrade));
            student.GPA = accumulate(student.courses.begin(), student.courses.end(), 0.0,
                                [](double sum, const pair<string, double> &course) {
                    return sum + course.second;
                }) / student.courses.size();
            cout << "Course registered successfully!\n";
            return;
        }
    }
    cout << "Student ID not found.\n";
}

در این بخش  تابع به کاربر اجازه می‌دهد تا دروس و نمرات را برای یک دانشجو را ثبت کند و معدل را به‌روز کند.
void displayStudentList(const vector<Student> &students) {
    cout << "Student List:\n";
    for (const auto &student : students) {
        cout << "Name: " << student.name << ", ID: " << student.studentID 
             << ", Major: " << student.major << ", GPA: " << fixed << setprecision(2) 
             << student.GPA << "\n";
    }
}
این تابع لیست تمام دانشجویان را به همراه اطلاعات آن‌ها نمایش می‌دهد.
void displayStudentList(const vector<Student> &students, const string &major) {
    cout << "Students in Major: " << major << "\n";
    for (const auto &student : students) {
        if (student.major == major) {
            cout << "Name: " << student.name << ", ID: " << student.studentID 
                 << ", GPA: " << fixed << setprecision(2) << student.GPA << "\n";
        }
    }

}
این تابع لیست دانشجویانی که در یک رشته خاص تحصیل می‌کنند را نمایش می دهد.
void generateGradeSheet(const vector<Student> &students, const string &studentID) {
    for (const auto &student : students) {if (student.studentID == studentID) {
            cout << "Grade Sheet for " << student.name << " (ID: " << student.studentID << ")\n";
            for (const auto &course : student.courses) {
                cout << "Course: " << course.first << ", Grade: " << course.second << "\n";
            }
            cout << "GPA: " << fixed << setprecision(2) << student.GPA << "\n";
            return;
        }
    }
    cout << "Student ID not found.\n";
}
این تابع کارنامه یک دانشجو را بر اساس شناسه دانشجویی نمایش می دهد.
int main() {
    vector<Student> students;
    int choice;
    do {
        cout << "1. Register Student\n2. Register Course\n3. Display All Students\n4. Display Students by Major\n5. Generate Grade Sheet\n6. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
            case 1:
                registerStudent(students);
                break;
            case 2:
                registerCourse(students);
                break;
            case 3:
                displayStudentList(students);
                break;
            case 4: {
                string major;
                cout << "Enter major: ";
                cin >> major;
                displayStudentList(students, major);
                break;
            }
            case 5: {
                string studentID;
                cout << "Enter student ID: ";
                cin >> studentID;
                generateGradeSheet(students, studentID);
                break;
            }
            case 6:
                cout << "Exiting program.\n";
                break;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 6);
    return 0;
}

در این بخش، در  برنامه یک حلقه do-while را قرار دادم که به کاربر اجازه می‌دهد تا از منوی اصلی گزینه‌های مختلف را انتخاب کند. بر اساس انتخاب کاربر، توابع مربوطه برای ثبت نام دانشجو، ثبت نام دروس، نمایش لیست دانشجویان، نمایش دانشجویان بر اساس رشته، و تولید کارنامه فراخوانی می‌شوند. در نهایت، اگر کاربر گزینه خروج را انتخاب کند، برنامه خاتمه می‌یابد.
