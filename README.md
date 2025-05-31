# -Hostel-Room-Allocation-System
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

struct Student {
    int student_id;
    string name;
    string gender;
    int age;
    string contact;
    string preferences;
    int assigned_room = -1;
    int priority;
};

struct Room {
    int room_number;
    string room_type;
    int floor;
    bool is_occupied = false;
};

struct Feedback {
    int student_id;
    int rating;
    string comment;
};

// Vectors for Data Storage
vector<Student> students;
vector<Room> rooms = {
    {101, "single", 1, false},
    {102, "double", 1, false},
    {201, "single", 2, false},
    {202, "double", 2, false},
    {301, "double", 3, false},
    {302, "single", 3, false}
};
vector<Feedback> feedbacks;

// Register Student
void registerStudent() {
    Student s;
    cout << "Enter Student ID: "; cin >> s.student_id;
    cout << "Enter Name: "; cin.ignore(); getline(cin, s.name);
    cout << "Enter Gender: "; getline(cin, s.gender);
    cout << "Enter Age: "; cin >> s.age;
    cout << "Enter Contact: "; cin >> s.contact;
    cout << "Enter Preferences (Single/Double): "; cin >> s.preferences;
    cout << "Enter Priority: "; cin >> s.priority;
    students.push_back(s);
    cout << "Student Registered Successfully.\n";
}

// Display All Allocations
void displayAllAllocations() {
    cout << "\nStudent Room Allocations:\n";
    for (const auto &s : students) {
        cout << "Student: " << s.name << " | ID: " << s.student_id << " | Assigned Room: ";
        if (s.assigned_room == -1)
            cout << "Not Assigned\n";
        else
            cout << s.assigned_room << "\n";
    }
}

// Allocate Rooms
void allocateRooms() {
    sort(students.begin(), students.end(), [](Student &a, Student &b) { return a.priority < b.priority; });
    for (auto &s : students) {
        for (auto &r : rooms) {
            if (!r.is_occupied && s.preferences == r.room_type) {
                s.assigned_room = r.room_number;
                r.is_occupied = true;
                cout << "Room " << r.room_number << " assigned to " << s.name << "\n";
                break;
            }
        }
    }
    displayAllAllocations();
}

// Submit Feedback
void submitFeedback() {
    Feedback f;
    cout << "Enter Student ID: "; cin >> f.student_id;
    cout << "Enter Rating (1-5): "; cin >> f.rating;
    cout << "Enter Comment: "; cin.ignore(); getline(cin, f.comment);
    feedbacks.push_back(f);
    cout << "Feedback Submitted.\n";
}

// Swap Rooms
void swapRooms() {
    int id1, id2;
    cout << "Enter Student ID 1: "; cin >> id1;
    cout << "Enter Student ID 2: "; cin >> id2;
    Student *s1 = nullptr, *s2 = nullptr;

    for (auto &s : students) {
        if (s.student_id == id1) s1 = &s;
        if (s.student_id == id2) s2 = &s;
    }

    if (s1 && s2) {
        swap(s1->assigned_room, s2->assigned_room);
        cout << "Rooms Swapped Successfully.\n";
    } else {
        cout << "Student IDs not found.\n";
    }
}

// Main Menu
int main() {
    int choice;
    do {
        cout << "\nHostel Room Allocation System\n";
        cout << "1. Register Student\n2. Allocate Rooms\n3. Display All Allocations\n4. Submit Feedback\n5. Swap Rooms\n6. Exit\nChoose an option: ";
        cin >> choice;

        switch (choice) {
            case 1: registerStudent(); break;
            case 2: allocateRooms(); break;
            case 3: displayAllAllocations(); break;
            case 4: submitFeedback(); break;
            case 5: swapRooms(); break;
            case 6: cout << "Exiting..."; break;
            default: cout << "Invalid choice. Try again.\n";
        }

    } while (choice != 6);

    return 0;
}
