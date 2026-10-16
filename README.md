Linux System Health Monitor & Reporter

An automated, enterprise-ready shell scripting utility written in Bash that profiles core system health metrics on Linux operating systems. The script safely extracts hardware utilization telemetry and software environment properties, compiling them into a cleanly formatted, persistent plain-text diagnostic log.
The script processes commands sequentially, utilizing structured data streams to build a diagnostic profile without relying on heavy third-party monitoring daemons or taxing system resources.

WORKING:

OUTPUT="system-report.txt"
The script begins by creating a variable called $OUTPUT. This acts as a central configuration setting. Whenever the script needs to write data, it points to this variable so we don't have to hardcode the filename multiple times.

echo "Linux System Health Report"> $OUTPUT
The single > bracket is a destructive write operator. It checks if system-report.txt already exists. If it does, it wipes all historical contents to start fresh. If it doesn't exist, it creates the file and prints the header line.

Collecting System Identity
• $(hostname): A subshell command that queries the kernel for the machine's local network identification name.
• $(uname -r): The uname utility prints system information; the -r flag explicitly requests the exact operating system release version.
• >>: The double bracket is an append operator. It safely injects these text strings onto a brand new line at the bottom of the file without deleting what is already there.

Isolating Disk Storage Space
• df -h /: Queries the disk filesystem utilization explicitly for the root storage partition (/) in human-readable notation (Gigabytes/Megabytes instead of raw blocks).
• | awk 'NR==2 {print $5}': The pipe (|) sends that data table to awk. NR==2 targets the second row of the table (skipping the headers), and {print $5} isolates the 5th column, which holds the active utilization percentage (e.g., 45%).
• This string is assigned to $DISK_PERCENT and logged.

Calculating Real-Time RAM Load
• free -h: Polls the kernel's memory management subsystem to display standard physical and swap allocations.
• awk 'NR==2': Freezes focus on row 2, which represents physical memory hardware (RAM).
• {print $3} and {print $2}: Extracts column 3 (RAM capacity actively occupied by applications) and column 2 (total hardware threshold installed on the motherboard).
• The variables are cleanly paired into a standard configuration format inside the report file.

Section Break Labelling
echo "Top Processes : process list ">> $OUTPUT



EXPECTED OUTPUT :

Hostname: workshop-po
Kernel: 6.x.x
Disk Usage:  /-45% used
Memory Usage: 3.2 GB/8GB
Top Processes: process list
