Overview

This C++ Patient Scheduling System simulates a healthcare queue prioritization where patients are served based on urgency. It handles two main types of patients: Urgent and Normal. Urgent patients are always prioritized and served before Normal patients. The system tracks patients’ arrival times, manages queueing, and calculates statistics like average waiting times. Time is simulated manually in the system and is advanced each minute by user input.

The system uses Object-Oriented Programming (OOP) principles and the C++ Standard Template Library (STL) for managing data structures such as vectors and queues. It also includes exception handling to ensure the system operates smoothly even in edge cases.

Features

Patient Details:
Patient ID in the format of an Egyptian National ID (e.g., 28501012345678).
Gender: Male or Female (represented as 'M' or 'F').
Arrival time recorded in 24-hour format (e.g., 14:30 for 2:30 PM).
Patient type: Urgent or Normal.
Queueing System:
Urgent patients are always served before Normal patients.
Maximum service time per patient is limited to 10 minutes.
Functional Requirements:
Data Setup: Randomly generates patient details based on different scenarios (100, 300, or 700 patients).
Time Simulation: The time advances every minute (user-triggered), and patients are dispatched and served accordingly.
Serving Patients: A random number of patients between 5 and 10 are served per minute. The system prioritizes urgent patients first.
Exception Handling:
Ensures proper error handling for out-of-bounds or invalid operations using try-catch blocks.
Statistics:
Tracks and calculates the average waiting time for all patients once the simulation ends.
System Requirements

