# CS2-lab-assignment
/******************************************************************************

                              Online C++ Compiler.
               Code, Compile, Run and Debug C++ program online.
Write your code in this editor and press "Run" button to compile and execute it.

*******************************************************************************/

#include <iostream>
#include <string>
#include <regex>
#include <queue>
#include <vector>
#include <cstdlib>  // for rand()
#include <ctime>    // for time()

using namespace std;

// Patient class to store patient details
class Patient {
public:
    string id;
    char gender; // 'M' or 'F'
    string arrivalTime;
    string type; // "Urgent" or "Normal"
    int waitingTime = 0; // in minutes

    Patient(string id, char gender, string arrivalTime, string type) {
        this->id = id;
        this->gender = gender;
        this->arrivalTime = arrivalTime;
        this->type = type;
    }

    bool validateID() {
        regex idPattern("^[0-9]{14}$");
        return regex_match(this->id, idPattern);
    }
};

// Comparator for priority queue to prioritize urgent patients first
struct PatientComparator {
    bool operator()(const Patient& p1, const Patient& p2) {
        // Prioritize urgent patients over normal patients
        if (p1.type == "Urgent" && p2.type != "Urgent") {
            return false; // p1 comes first
        } else if (p1.type != "Urgent" && p2.type == "Urgent") {
            return true; // p2 comes first
        }
        // If both are the same type, prioritize by arrival time
        return p1.arrivalTime > p2.arrivalTime;
    }
};

// QueueManager class to manage patient queues
class QueueManager {
private:
    priority_queue<Patient, vector<Patient>, PatientComparator> urgentQueue; // Urgent patients queue
    queue<Patient> normalQueue; // Normal patients queue

public:
    void dispatchPatients(vector<Patient>& patients, string currentTime) {
        for (auto& patient : patients) {
            if (patient.arrivalTime == currentTime) {
                if (patient.type == "Urgent") {
                    urgentQueue.push(patient);
                } else {
                    normalQueue.push(patient);
                }
            }
        }
    }

    void servePatients(int N) {
        int servedCount = 0;
        cout << "Serving " << N << " patients..." << endl;

        // Serve urgent patients first
        while (!urgentQueue.empty() && servedCount < N) {
            Patient p = urgentQueue.top();
            urgentQueue.pop();
            servedCount++;
            cout << "Patient ID: " << p.id << " (Urgent) served." << endl;
        }

        // Serve normal patients if there's still space
        while (!normalQueue.empty() && servedCount < N) {
            Patient p = normalQueue.front();
            normalQueue.pop();
            servedCount++;
            cout << "Patient ID: " << p.id << " (Normal) served." << endl;
        }
    }

    void calculateStatistics(int totalPatients, int urgentCount, int normalCount) {
        cout << "Total patients: " << totalPatients << endl;
        cout << "Urgent patients: " << urgentCount << endl;
        cout << "Normal patients: " << normalCount << endl;
        // Add logic to calculate average waiting time here (just a placeholder)
        cout << "Average waiting time: " << (urgentCount + normalCount) / 2.0 << " minutes" << endl;
    }
};

// SystemManager class to simulate the overall system
class SystemManager {
public:
    vector<Patient> allPatients; // Store all generated patients
    QueueManager queueManager;
    string currentTime = "00:00"; // Simulate time starting from 00:00


    SystemManager() {
        srand(time(0)); // Seed random number generator
    }

    void generatePatients(int scenario) {
        int numPatients = 0;
        if (scenario == 1) numPatients = 100;
        else if (scenario == 2) numPatients = 300;
        else if (scenario == 3) numPatients = 700;

        for (int i = 0; i < numPatients; ++i) {
            string id = to_string(rand() % 10000000000000); // Random 14-digit ID
            char gender = (rand() % 2 == 0) ? 'M' : 'F';
            string arrivalTime = to_string(rand() % 24) + ":" + to_string(rand() % 60); // Random arrival time
            string type = (rand() % 2 == 0) ? "Urgent" : "Normal";

            Patient p(id, gender, arrivalTime, type);
            if (p.validateID()) {
                allPatients.push_back(p);
            }
        }
    }

    void advanceTime() {
        // Increment time by 1 minute (simplified time simulation)
        int hour = stoi(currentTime.substr(0, 2));
        int minute = stoi(currentTime.substr(3, 2));
        minute++;
        if (minute == 60) {
            minute = 0;
            hour++;
            if (hour == 24) {
                hour = 0;
            }
        }
        currentTime = (hour < 10 ? "0" : "") + to_string(hour) + ":" + (minute < 10 ? "0" : "") + to_string(minute);
    }

    string getCurrentTime() {
        return currentTime;
    }

    void displayStatus() {
        cout << "Current Time: " << currentTime << endl;
        cout << "Waiting Urgent Patients: ";
        // Display all waiting urgent patients
        cout << endl;
        cout << "Waiting Normal Patients: ";
        // Display all waiting normal patients
        cout << endl;
    }
};

int main() {
    SystemManager system;

    system.generatePatients(2); // Scenario 2: Generate 300 patients

    string userInput;
    while (true) {
        system.advanceTime();
        system.queueManager.dispatchPatients(system.allPatients, system.getCurrentTime());
        system.queueManager.servePatients(rand() % 6 + 5); // Random N between 5 and 10
        system.queueManager.calculateStatistics(system.allPatients.size(), 120, 180); // Example statistics

        system.displayStatus();

        cout << "Press Enter to advance time by 1 minute..." << endl;
        getline(cin, userInput);
    }

    return 0;
}
