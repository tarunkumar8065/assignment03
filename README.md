# Shell Programming — 3rd Assignment

> A collection of 10 shell programs covering process management, file operations, system monitoring, string manipulation, and more.

---

## How to Run

```bash
# Make any script executable
chmod +x <script_name>.sh

# Run the script
./<script_name>.sh
```

---

## Program 1: Check if a Process is Running

**Aim:** Write a script to check if a specific process is running and display its PID.

**Code:** [`q1_process_check.sh`](q1_process_check.sh)
```bash
#!/bin/bash
# Program 1: To check if a specific process is running and display its PID

echo "Enter the process name to check: "
read process_name

pid=$(pgrep -x "$process_name")

if [ -n "$pid" ]; then
    echo "Process '$process_name' is running."
    echo "PID(s): $pid"
    echo ""
    echo "Process details:"
    ps -p $pid -o pid,user,comm,%cpu,%mem,start
else
    echo "Process '$process_name' is NOT running."
fi
```

**Sample Output:**
```
Enter the process name to check:
bash
Process 'bash' is running.
PID(s): 1234
5678

Process details:
  PID USER     COMM            %CPU %MEM  STARTED
 1234 kushagra bash             0.0  0.1 14:00:00
 5678 kushagra bash             0.0  0.1 14:05:00
```
```
Enter the process name to check:
nginx
Process 'nginx' is NOT running.
```

---

## Program 2: Display Disk Usage of Home Directory

**Aim:** Create a script to display the disk usage of the home directory in a human-readable format.

**Code:** [`q2_disk_usage.sh`](q2_disk_usage.sh)
```bash
#!/bin/bash
# Program 2: To display the disk usage of the home directory in human-readable format

echo "Disk Usage of Home Directory ($HOME):"
echo "======================================="
du -sh $HOME 2>/dev/null
echo ""
echo "Breakdown of subdirectories:"
echo "======================================="
du -h --max-depth=1 $HOME 2>/dev/null | sort -rh
echo ""
echo "Overall Disk Usage:"
echo "======================================="
df -h $HOME
```

**Sample Output:**
```
Disk Usage of Home Directory (/home/kushagra):
=======================================
4.2G    /home/kushagra

Breakdown of subdirectories:
=======================================
4.2G    /home/kushagra
1.8G    /home/kushagra/Documents
856M    /home/kushagra/Downloads
512M    /home/kushagra/.local
128M    /home/kushagra/Desktop
64M     /home/kushagra/Pictures

Overall Disk Usage:
=======================================
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   22G   26G  46% /
```

---

## Program 3: Generate Fibonacci Sequence

**Aim:** Write a script to generate the Fibonacci sequence up to n terms, where n is provided by the user.

**Code:** [`q3_fibonacci.sh`](q3_fibonacci.sh)
```bash
#!/bin/bash
# Program 3: To generate the Fibonacci sequence up to n terms

echo "Enter the number of terms (n): "
read n

a=0
b=1

echo "Fibonacci sequence up to $n terms:"
for (( i=1; i<=n; i++ ))
do
    echo -n "$a "
    temp=$((a + b))
    a=$b
    b=$temp
done
echo ""
```

**Sample Output:**
```
Enter the number of terms (n):
10
Fibonacci sequence up to 10 terms:
0 1 1 2 3 5 8 13 21 34
```

> **Verification:** 0, 1, 1, 2, 3, 5, 8, 13, 21, 34 — each term is the sum of the two preceding terms ✓

---

## Program 4: Search and Replace String in a File

**Aim:** Write a script to search for a string in a file and replace it with another string.

**Code:** [`q4_search_replace.sh`](q4_search_replace.sh)
```bash
#!/bin/bash
# Program 4: To search for a string in a file and replace it with another string

FILENAME="sample_data.txt"

# Create a sample file for demonstration
cat > $FILENAME << EOF
Hello World
Welcome to Shell Programming
Shell scripting is powerful
Learn Shell scripting today
Programming is fun
EOF

echo "Original file contents:"
echo "========================"
cat $FILENAME
echo "========================"
echo ""

echo "Enter the string to search: "
read search_str

echo "Enter the replacement string: "
read replace_str

# Perform the replacement using sed
sed -i "s/$search_str/$replace_str/g" $FILENAME

echo ""
echo "Modified file contents:"
echo "========================"
cat $FILENAME
echo "========================"
echo "All occurrences of '$search_str' replaced with '$replace_str'."

# Clean up
rm -f $FILENAME
```

**Sample Output:**
```
Original file contents:
========================
Hello World
Welcome to Shell Programming
Shell scripting is powerful
Learn Shell scripting today
Programming is fun
========================

Enter the string to search:
Shell
Enter the replacement string:
Bash

Modified file contents:
========================
Hello World
Welcome to Bash Programming
Bash scripting is powerful
Learn Bash scripting today
Programming is fun
========================
All occurrences of 'Shell' replaced with 'Bash'.
```

---

## Program 5: Check Palindrome (String or Number)

**Aim:** Create a script that checks whether a given string or number is a palindrome.

**Code:** [`q5_palindrome_check.sh`](q5_palindrome_check.sh)
```bash
#!/bin/bash
# Program 5: To check whether a given string or number is a palindrome

echo "Enter a string or number: "
read input

# Reverse the input
reversed=$(echo "$input" | rev)

if [ "$input" == "$reversed" ]; then
    echo "'$input' is a Palindrome."
else
    echo "'$input' is NOT a Palindrome."
fi
```

**Sample Output:**
```
Enter a string or number:
madam
'madam' is a Palindrome.
```
```
Enter a string or number:
12321
'12321' is a Palindrome.
```
```
Enter a string or number:
hello
'hello' is NOT a Palindrome.
```

---

## Program 6: Monitor a File for Changes

**Aim:** Write a script that monitors a file for changes and displays a message when it is modified.

**Code:** [`q6_file_monitor.sh`](q6_file_monitor.sh)
```bash
#!/bin/bash
# Program 6: To monitor a file for changes and display a message when modified

echo "Enter the filename to monitor: "
read filename

if [ ! -f "$filename" ]; then
    echo "File '$filename' does not exist. Creating it..."
    touch "$filename"
fi

echo "Monitoring '$filename' for changes... (Press Ctrl+C to stop)"

# Store the initial modification time
last_mod=$(stat -c %Y "$filename" 2>/dev/null || stat -f %m "$filename" 2>/dev/null)

while true
do
    current_mod=$(stat -c %Y "$filename" 2>/dev/null || stat -f %m "$filename" 2>/dev/null)
    
    if [ "$current_mod" != "$last_mod" ]; then
        echo "[$(date '+%Y-%m-%d %H:%M:%S')] File '$filename' has been MODIFIED!"
        last_mod=$current_mod
    fi
    
    sleep 2
done
```

**Sample Output:**
```
Enter the filename to monitor:
test.txt
Monitoring 'test.txt' for changes... (Press Ctrl+C to stop)
[2026-02-26 14:20:15] File 'test.txt' has been MODIFIED!
[2026-02-26 14:20:45] File 'test.txt' has been MODIFIED!
^C
```

> **Note:** The script polls the file every 2 seconds. Modify the file from another terminal to see the change detection message.

---

## Program 7: Read, Sort, and Save Numbers

**Aim:** Create a script to read numbers from a file, sort them in ascending order, and save the output to another file.

**Code:** [`q7_sort_numbers.sh`](q7_sort_numbers.sh)
```bash
#!/bin/bash
# Program 7: To read numbers from a file, sort them, and save to another file

INPUT_FILE="numbers_input.txt"
OUTPUT_FILE="numbers_sorted.txt"

# Create a sample input file with unsorted numbers
cat > $INPUT_FILE << EOF
45
12
89
3
67
23
56
1
78
34
EOF

echo "Contents of input file ($INPUT_FILE):"
echo "========================"
cat $INPUT_FILE
echo "========================"
echo ""

# Sort the numbers in ascending order and save to output file
sort -n $INPUT_FILE > $OUTPUT_FILE

echo "Sorted numbers saved to '$OUTPUT_FILE':"
echo "========================"
cat $OUTPUT_FILE
echo "========================"

# Clean up
rm -f $INPUT_FILE $OUTPUT_FILE
```

**Sample Output:**
```
Contents of input file (numbers_input.txt):
========================
45
12
89
3
67
23
56
1
78
34
========================

Sorted numbers saved to 'numbers_sorted.txt':
========================
1
3
12
23
34
45
56
67
78
89
========================
```

---

## Program 8: Count Files by Extension

**Aim:** Write a script to count the number of files with specific extensions (e.g., `.txt`, `.sh`) in a directory.

**Code:** [`q8_count_extensions.sh`](q8_count_extensions.sh)
```bash
#!/bin/bash
# Program 8: To count the number of files with specific extensions in a directory

echo "Enter the directory path: "
read dir_path

if [ ! -d "$dir_path" ]; then
    echo "Directory '$dir_path' does not exist!"
    exit 1
fi

echo "Enter the file extension to count (e.g., txt, sh, py): "
read ext

count=$(find "$dir_path" -maxdepth 1 -name "*.$ext" -type f | wc -l)

echo ""
echo "Directory: $dir_path"
echo "Extension: .$ext"
echo "Number of .$ext files: $count"

if [ $count -gt 0 ]; then
    echo ""
    echo "Files found:"
    find "$dir_path" -maxdepth 1 -name "*.$ext" -type f -exec basename {} \;
fi
```

**Sample Output:**
```
Enter the directory path:
/home/kushagra/ERD_Assignment
Enter the file extension to count (e.g., txt, sh, py):
sh

Directory: /home/kushagra/ERD_Assignment
Extension: .sh
Number of .sh files: 10

Files found:
q1_process_check.sh
q2_disk_usage.sh
q3_fibonacci.sh
q4_search_replace.sh
q5_palindrome_check.sh
q6_file_monitor.sh
q7_sort_numbers.sh
q8_count_extensions.sh
q9_system_info.sh
q10_multiplication_table.sh
```

---

## Program 9: Display System Information

**Aim:** Write a script to display system information such as OS version, kernel version, and system uptime.

**Code:** [`q9_system_info.sh`](q9_system_info.sh)
```bash
#!/bin/bash
# Program 9: To display system information

echo "========================================="
echo "         SYSTEM INFORMATION              "
echo "========================================="
echo ""

echo "Hostname      : $(hostname)"
echo ""
echo "OS Version    : $(cat /etc/os-release 2>/dev/null | grep PRETTY_NAME | cut -d= -f2 | tr -d '"' || uname -o)"
echo ""
echo "Kernel Version: $(uname -r)"
echo ""
echo "Architecture  : $(uname -m)"
echo ""
echo "System Uptime : $(uptime -p 2>/dev/null || uptime)"
echo ""
echo "Current User  : $(whoami)"
echo ""
echo "Date & Time   : $(date)"
echo ""

echo "========================================="
echo "         MEMORY INFORMATION              "
echo "========================================="
free -h 2>/dev/null || echo "Memory info not available"
echo ""

echo "========================================="
echo "         CPU INFORMATION                 "
echo "========================================="
echo "CPU Model     : $(grep 'model name' /proc/cpuinfo 2>/dev/null | head -1 | cut -d: -f2 | xargs || echo 'N/A')"
echo "CPU Cores     : $(nproc 2>/dev/null || echo 'N/A')"
```

**Sample Output:**
```
=========================================
         SYSTEM INFORMATION              
=========================================

Hostname      : kushagra-pc

OS Version    : Ubuntu 22.04.3 LTS

Kernel Version: 5.15.0-91-generic

Architecture  : x86_64

System Uptime : up 3 days, 5 hours, 42 minutes

Current User  : kushagra

Date & Time   : Wed Feb 26 14:30:00 IST 2026

=========================================
         MEMORY INFORMATION              
=========================================
              total        used        free      shared  buff/cache   available
Mem:          15Gi       6.2Gi       4.8Gi       512Mi       4.5Gi       8.5Gi
Swap:         2.0Gi          0B       2.0Gi

=========================================
         CPU INFORMATION                 
=========================================
CPU Model     : Intel(R) Core(TM) i7-10750H CPU @ 2.60GHz
CPU Cores     : 12
```

---

## Program 10: Multiplication Table

**Aim:** Create a script that generates and displays the multiplication table for a given number.

**Code:** [`q10_multiplication_table.sh`](q10_multiplication_table.sh)
```bash
#!/bin/bash
# Program 10: To generate and display the multiplication table for a given number

echo "Enter a number: "
read num

echo ""
echo "Multiplication Table for $num:"
echo "================================"
for (( i=1; i<=10; i++ ))
do
    result=$((num * i))
    echo "$num x $i = $result"
done
echo "================================"
```

**Sample Output:**
```
Enter a number:
7

Multiplication Table for 7:
================================
7 x 1 = 7
7 x 2 = 14
7 x 3 = 21
7 x 4 = 28
7 x 5 = 35
7 x 6 = 42
7 x 7 = 49
7 x 8 = 56
7 x 9 = 63
7 x 10 = 70
================================
```

---

## File Structure

```
ERD_Assignment/
├── README.md
├── q1_process_check.sh
├── q2_disk_usage.sh
├── q3_fibonacci.sh
├── q4_search_replace.sh
├── q5_palindrome_check.sh
├── q6_file_monitor.sh
├── q7_sort_numbers.sh
├── q8_count_extensions.sh
├── q9_system_info.sh
└── q10_multiplication_table.sh
```
