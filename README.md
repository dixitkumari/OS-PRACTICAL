# OS-PRACTICAL

QUESTION 1 : Demonstration of various Operating System functions using OS Simulator.
<img width="622" height="858" alt="image" src="https://github.com/user-attachments/assets/7dfd45db-9f18-4c7e-b7cf-17c5472dd0e5" />
<img width="808" height="471" alt="image" src="https://github.com/user-attachments/assets/17c4d953-9924-4218-95fd-69363a66bfa5" />
<img width="464" height="439" alt="image" src="https://github.com/user-attachments/assets/3d3664f3-14d6-4d15-b648-cabb87f505df" />
<img width="578" height="459" alt="image" src="https://github.com/user-attachments/assets/11493557-7054-4ee5-964e-3b732f986e09" />
<img width="368" height="428" alt="image" src="https://github.com/user-attachments/assets/d5622c79-95e3-4973-8887-957fc517c8fa" />
<img width="717" height="441" alt="image" src="https://github.com/user-attachments/assets/cb732d5f-aaab-4d22-a5af-16790af40251" />


**QUESTION 2** QUESTION 2 :: Execute various LINUX commands for: i. ii. iii. Information Maintenance: wc, clear, cal, who, date, pwd File Management: cat, cp, rm, mv, cmp, comm, diff, find, grep, awk Directory Management: cd, mkdir, rmdir, ls . 
<img width="502" height="519" alt="image" src="https://github.com/user-attachments/assets/7ebf4b71-78bb-4291-baae-e339f377d1a0" />
<img width="532" height="583" alt="image" src="https://github.com/user-attachments/assets/24dcaa5b-05ef-4a75-9f61-d75b1e184c4c" />
<img width="504" height="461" alt="image" src="https://github.com/user-attachments/assets/74f973cd-3cf7-4639-b2f1-1e264ce3cb35" />
<img width="458" height="492" alt="image" src="https://github.com/user-attachments/assets/1c8b1583-e598-434c-8a88-5258864222de" />

**QUESTION 3**Execute various LINUX commands for: i. ii. iii. Process Control: fork, getpid, ps, kill, sleep Communication: Input-output redirection, Pipe Protection Management: chmod, chown, chgrp.
<img width="944" height="377" alt="image" src="https://github.com/user-attachments/assets/f9147d9a-029a-4363-912d-5c1be1e3a5ec" />

**QUESTION 4**:  Write a program(using fork () and/or exec () commands) where parent and child execute: i. same program, same code. ii. iii. same program, different code. before terminating, the parent waits for the child to finish its task.

#(i)Same Program , different code#

import os
from multiprocessing import Process

def child():
    print("Child Process")
    print("Child PID:", os.getpid())
    print("Child is executing its task")

if __name__ == "__main__":
    p = Process(target=child)

    print("Parent Process")
    print("Parent PID:", os.getpid())

    p.start()
    p.join()

    print("Parent: Child process completed")

    <img width="940" height="141" alt="image" src="https://github.com/user-attachments/assets/c415e8a8-9d91-460c-a272-47bd77eab102" />

*(ii) Same program , same code*

import os
from multiprocessing import Process

def display():
    print("Executing same code")
    print("Process ID:", os.getpid())

if __name__ == "__main__":

    p = Process(target=display)

    print("Parent Process")
    print("Parent PID:", os.getpid())

    p.start()

    display()

    p.join()

    print("Parent: Child process completed")

<img width="940" height="172" alt="image" src="https://github.com/user-attachments/assets/8783165a-4e0f-4e9e-83ab-7efde180770e" />

*(iii) before terminating, the parent waits for the child to finish its task.*
import os
from multiprocessing import Process
import time

def display():
    print("Child Process")
    print("Child PID:", os.getpid())

    print("Child is performing its task...")
    time.sleep(3)

    print("Child task completed.")

if __name__ == "__main__":

    print("Parent Process")
    print("Parent PID:", os.getpid())

    p = Process(target=display)

    p.start()

    print("Parent is waiting for child...")

    p.join()

    print("Parent: Child has finished its task.")
    print("Parent process terminating.")

<img width="940" height="252" alt="image" src="https://github.com/user-attachments/assets/8d315636-7ca2-40b1-9f90-1ed5642fa85f" />

**QUESTION 5**: Write a program to report behaviour of Linux kernel including kernel version, CPU type and CPU information.

import platform
import subprocess

print("===== SYSTEM INFORMATION =====")

print("Operating System:", platform.system())
print("System Version:", platform.version())
print("Machine Architecture:", platform.machine())

print("\n===== CPU INFORMATION =====")

try:
    result = subprocess.run(
        [
            "powershell",
            "-NoProfile",
            "-Command",
            "Get-CimInstance Win32_Processor | "
            "Select-Object Name, Manufacturer, NumberOfCores, "
            "NumberOfLogicalProcessors | Format-List"
        ],
        capture_output=True,
        text=True
    )

    if result.returncode == 0 and result.stdout.strip():
        print(result.stdout)
    else:
        print("CPU information unavailable.")

except Exception as e:
    print("Error:", e)

print("===== REPORT COMPLETED =====")
<img width="763" height="482" alt="image" src="https://github.com/user-attachments/assets/cf7210a8-6d26-4e6b-bbb6-711ffb1dc64f" />

**QUESTION 6 : Write a program to report behaviour of Linux kernel including information on configured memory, amount of free and used memory. (Memory information)**

import platform

print("===== LINUX KERNEL MEMORY INFORMATION =====")

if platform.system() == "Linux":

    with open("/proc/meminfo", "r") as file:
        lines = file.readlines()

    memory = {}

    for line in lines:
        parts = line.split()
        memory[parts[0].rstrip(":")] = int(parts[1])

    total = memory["MemTotal"]
    free = memory["MemFree"]
    available = memory["MemAvailable"]
    used = total - available

    print("Total Memory:", total // 1024, "MB")
    print("Free Memory:", free // 1024, "MB")
    print("Used Memory:", used // 1024, "MB")
    print("Available Memory:", available // 1024, "MB")

elif platform.system() == "Windows":

    import ctypes

    class MEMORYSTATUSEX(ctypes.Structure):
        _fields_ = [
            ("dwLength", ctypes.c_ulong),
            ("dwMemoryLoad", ctypes.c_ulong),
            ("ullTotalPhys", ctypes.c_ulonglong),
            ("ullAvailPhys", ctypes.c_ulonglong),
            ("ullTotalPageFile", ctypes.c_ulonglong),
            ("ullAvailPageFile", ctypes.c_ulonglong),
            ("ullTotalVirtual", ctypes.c_ulonglong),
            ("ullAvailVirtual", ctypes.c_ulonglong),
            ("ullAvailExtendedVirtual", ctypes.c_ulonglong)
        ]

    mem = MEMORYSTATUSEX()
    mem.dwLength = ctypes.sizeof(MEMORYSTATUSEX)

    ctypes.windll.kernel32.GlobalMemoryStatusEx(ctypes.byref(mem))

    total = mem.ullTotalPhys // (1024 * 1024)
    free = mem.ullAvailPhys // (1024 * 1024)
    used = total - free

    print("Total Memory:", total, "MB")
    print("Free Memory:", free, "MB")
    print("Used Memory:", used, "MB")
    print("Memory Usage:", mem.dwMemoryLoad, "%")

else:
    print("Operating system not supported.")

print("\n===== MEMORY REPORT COMPLETED =====")

<img width="940" height="318" alt="image" src="https://github.com/user-attachments/assets/18a8c912-1841-43bd-ae07-55489021c5ff" />

**QUESTION 7 : Write a program to copy files using system calls.**

import os

try:
    # Open source file
    source = os.open("source.txt", os.O_RDONLY)

    # Create destination file
    destination = os.open(
        "destination.txt",
        os.O_WRONLY | os.O_CREAT | os.O_TRUNC,
        0o644
    )

    # Copy file contents
    while True:
        data = os.read(source, 1024)

        if not data:
            break

        os.write(destination, data)

    print("File copied successfully!")

    os.close(source)
    os.close(destination)

except OSError as e:
    print("Error:", e)

<img width="940" height="79" alt="image" src="https://github.com/user-attachments/assets/2a1f7740-cadd-4adf-b4ca-c77c51170181" />

**QUESTION 8 : Write a program to implement First Come First Serve (FCFS) scheduling algorithm.**

n = int(input("Enter number of processes: "))

processes = []

for i in range(n):
    at = int(input(f"Enter Arrival Time for P{i+1}: "))
    bt = int(input(f"Enter Burst Time for P{i+1}: "))

    processes.append([i+1, at, bt])

# Sort processes according to arrival time
processes.sort(key=lambda x: x[1])

current_time = 0
total_wt = 0
total_tat = 0

print("\nProcess\tAT\tBT\tCT\tTAT\tWT")

for p in processes:
    pid, at, bt = p

    if current_time < at:
        current_time = at

    current_time += bt

    ct = current_time
    tat = ct - at
    wt = tat - bt

    total_wt += wt
    total_tat += tat

    print(f"P{pid}\t{at}\t{bt}\t{ct}\t{tat}\t{wt}")

print("\nAverage Waiting Time:", total_wt / n)
print("Average Turnaround Time:", total_tat / n)

<img width="753" height="319" alt="image" src="https://github.com/user-attachments/assets/e2c6d8ac-2de9-49ee-8eb5-ced21d45b216" />

**QUESTION 9 : Write a program to implement Shortest Job First (SJF) scheduling algorithm.**
n = int(input("Enter number of processes: "))

processes = []

for i in range(n):
    at = int(input(f"Enter Arrival Time for P{i+1}: "))
    bt = int(input(f"Enter Burst Time for P{i+1}: "))

    processes.append([i+1, at, bt])

current_time = 0
completed = 0
total_wt = 0
total_tat = 0

done = [False] * n

print("\nProcess\tAT\tBT\tCT\tTAT\tWT")

while completed < n:

    available = []

    for i in range(n):
        if processes[i][1] <= current_time and not done[i]:
            available.append(i)

    if not available:
        current_time += 1
        continue

    # Select process with shortest burst time
    idx = min(available, key=lambda i: processes[i][2])

    pid, at, bt = processes[idx]

    current_time += bt

    ct = current_time
    tat = ct - at
    wt = tat - bt

    total_wt += wt
    total_tat += tat

    done[idx] = True
    completed += 1

    print(f"P{pid}\t{at}\t{bt}\t{ct}\t{tat}\t{wt}")

print("\nAverage Waiting Time:", round(total_wt/n, 2))
print("Average Turnaround Time:", round(total_tat/n, 2))

<img width="940" height="390" alt="image" src="https://github.com/user-attachments/assets/1927eb80-06cf-45ee-b41d-9db3a886b7de" />

**QUESTION 10 : : Write a program to implement non-preemptive priority-based scheduling algorithm.**

n = int(input("Enter number of processes: "))

processes = []

for i in range(n):
    at = int(input(f"Enter Arrival Time for P{i+1}: "))
    bt = int(input(f"Enter Burst Time for P{i+1}: "))
    pr = int(input(f"Enter Priority for P{i+1}: "))

    processes.append([i+1, at, bt, pr])

current_time = 0
completed = 0
total_wt = 0
total_tat = 0

done = [False] * n

print("\nProcess\tAT\tBT\tPriority\tCT\tTAT\tWT")

while completed < n:

    available = []

    for i in range(n):
        if processes[i][1] <= current_time and not done[i]:
            available.append(i)

    if not available:
        current_time += 1
        continue

    # Select process with highest priority
    idx = min(available, key=lambda i: (processes[i][3], processes[i][1], processes[i][0]))

    pid, at, bt, pr = processes[idx]

    current_time += bt

    ct = current_time
    tat = ct - at
    wt = tat - bt

    total_wt += wt
    total_tat += tat

    done[idx] = True
    completed += 1

    print(f"P{pid}\t{at}\t{bt}\t{pr}\t\t{ct}\t{tat}\t{wt}")

print("\nAverage Waiting Time:", round(total_wt/n, 2))
print("Average Turnaround Time:", round(total_tat/n, 2))

<img width="617" height="389" alt="image" src="https://github.com/user-attachments/assets/46fe9a95-4783-478a-9868-97e4e2edabc5" />

**QUESTION 11 : Write a program to calculate sum of n numbers using Pthreads.**

import threading
n = int(input("Enter the number of elements: "))
arr = []
print("Enter", n, "numbers:")

for i in range(n):
    num = int(input())
    arr.append(num)
total = 0
# Function executed by thread
def calculate_sum():
    global total
    for num in arr:
        total += num
# Create thread
t = threading.Thread(target=calculate_sum)
# Start thread
t.start()
# Wait for thread to finish
t.join()
print("Sum of", n, "numbers =", total) 

<img width="624" height="198" alt="image" src="https://github.com/user-attachments/assets/4cedd502-a1ed-44c4-b956-f6e579099003" />

**QUESTION 12: Write a program to implement first-fit, best-fit and worst-fit allocation strategies.**
# Memory Allocation Strategies

def first_fit(blocks, processes):
    memory = blocks.copy()
    print("\nFIRST FIT ALLOCATION")
    print("Process\tSize\tBlock")

    for i, p in enumerate(processes):
        allocated = False
        for j in range(len(memory)):
            if memory[j] >= p:
                print(f"P{i+1}\t{p}\tB{j+1}")
                memory[j] -= p
                allocated = True
                break
        if not allocated:
            print(f"P{i+1}\t{p}\tNot Allocated")
def best_fit(blocks, processes):
    memory = blocks.copy()
    print("\nBEST FIT ALLOCATION")
    print("Process\tSize\tBlock")
    for i, p in enumerate(processes):
        best = -1
        for j in range(len(memory)):
            if memory[j] >= p:
              if best == -1 or memory[j] < memory[best]:
                    best = j
        if best != -1:
            print(f"P{i+1}\t{p}\tB{best+1}")
            memory[best] -= p
        else:
            print(f"P{i+1}\t{p}\tNot Allocated")
def worst_fit(blocks, processes):
    memory = blocks.copy()
    print("\nWORST FIT ALLOCATION")
    print("Process\tSize\tBlock")
   for i, p in enumerate(processes):
        worst = -1
       for j in range(len(memory)):
            if memory[j] >= p:
                if worst == -1 or memory[j] > memory[worst]:
                    worst = j
      if worst != -1:
            print(f"P{i+1}\t{p}\tB{worst+1}")
            memory[worst] -= p
        else:
            print(f"P{i+1}\t{p}\tNot Allocated")
# Main Program
n = int(input("Enter number of memory blocks: "))
blocks = []
print("Enter memory block sizes:")
for i in range(n):
    blocks.append(int(input()))
m = int(input("\nEnter number of processes: "))
processes = []
print("Enter process sizes:")
for i in range(m):
    processes.append(int(input()))
first_fit(blocks, processes)
best_fit(blocks, processes)
worst_fit(blocks, processes)


<img width="718" height="606" alt="image" src="https://github.com/user-attachments/assets/dfe79956-e94f-4239-9e2d-0b6ea02517da" />
