#include<iostream>
#include<fstream>
#include<iomanip>
#include<stdlib.h>

using namespace std;

void project();
void addData();
void displayData();
void deleteData();
void getTrash();

class Student {
    int admissionNo;
    char sname[20];
    char sgender;
    int std;
    float smarks;
    double spercentage;

public:
    void getData();
    void showData();
    int getAdmno() {
        return admissionNo;
    }
};

int main() {
    Student s;
    project();
    return 0;
}

void project() {
    int ch;
    do {
        system("cls");
        cout << "**************STUDENT MANAGEMENT SYSTEM***************\n";
        cout << "1. Write Student Record\n";
        cout << "2. Read Student Record\n";
        cout << "3. Delete Student Record\n";
        cout << "4. Get Deleted Records\n";
        cout << "0. Exit\n";
        cout << "Enter your choice: ";
        cin >> ch;
        system("cls");

        switch (ch) {
            case 1: addData(); break;
            case 2: displayData(); break;
            case 3: deleteData(); break;
            case 4: getTrash(); break;
        }
        system("pause");
    } while (ch);
}

void Student::getData() {
    cout << "\n\n*******Enter Student Data*******\n";
    cout << "Admission No.: "; cin >> admissionNo;
    cout << "Full Name: "; cin.ignore(); cin.getline(sname, 20);
    cout << "Gender (M/F): "; cin >> sgender;
    cout << "Class: "; cin >> std;
    cout << "Total Marks (/500): "; cin >> smarks;
    cout << endl;
    spercentage = smarks * 100.0 / 500.00;
}

void Student::showData() {
    cout << "\n\n*******Student Details*******\n";
    cout << "Admission No.: " << admissionNo << endl;
    cout << "Full Name: " << sname << endl;
    cout << "Gender: " << sgender << endl;
    cout << "Class: " << std << endl;
    cout << "Total Marks (/500): " << smarks << endl;
    cout << "Percentage: " << spercentage << endl;
    cout << endl;
}

void Student::addData() {
    ofstream fout;
    fout.open("Stu.txt", ios::out | ios::app | ios::binary);
    s.getData();
    fout.write((char*)&s, sizeof(s));
    fout.close();
    cout << "\n\n*******Data Successfully Saved to File*******\n";
}

void displayData() {
    ifstream fin;
    fin.open("Stu.txt", ios::in | ios::binary);
    while (fin.read((char*)&s, sizeof(s))) {
        s.showData();
    }
    fin.close();
    cout << "\n\n******Data Reading from File Successfully Done*****\n";
}

void deleteData() {
    int n, flag = 0;
    ifstream fin;
    ofstream fout, tout;
    fin.open("Stu.txt", ios::in | ios::binary);
    fout.open("Temp.txt", ios::out | ios::app | ios::binary);
    tout.open("Trash.txt", ios::out | ios::app | ios::binary);
    cout << "Enter Admission Number: ";
    cin >> n;

    while (fin.read((char*)&s, sizeof(s))) {
        if (n == s.getAdmno()) {
            cout << "This Record " << n << " has been sent to Trash:\n";
            s.showData();
            tout.write((char*)&s, sizeof(s));
            flag++;
        } else {
            fout.write((char*)&s, sizeof(s));
        }
    }
    fout.close();
    tout.close();
    fin.close();

    if (flag == 0) 
        cout << n << " No Record found*****\n\n";

    remove("Stu.txt");
    rename("Temp.txt", "Stu.txt");
}

void getTrash() {
    ifstream fin;
    fin.open("Trash.txt", ios::in | ios::binary);
    while (fin.read((char*)&s, sizeof(s))) {
        s.showData();
    }
    fin.close();
    cout << "\n\n*******Data Reading from Trash File Successfully Done*****\n";
}
