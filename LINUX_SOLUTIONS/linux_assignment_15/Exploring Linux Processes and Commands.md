
**Part 1: Process Basics**

1. **Process Identification**

in this task i entered `ps aux` command. This will display a list of processes along with their details, including the PID. The output is,

![[Screenshot from 2023-11-03 11-06-15.png]]
2. **Process Termination**
	let's say we want to terminate a process with PID 5 : 
	**sudo kill 5**
	This command sends a termination signal (by default, `SIGTERM`) to the process with PID 5, requesting it to gracefully terminate. The process may perform cleanup actions before exiting.

**Difference between the `kill` command with the `-TERM` and `-KILL` signals:

1. `kill -TERM or kill -15` :
	- This is the default behavior when you use the `kill` command without specifying a signal.
	- It sends the `SIGTERM` signal to the target process.
	- `SIGTERM` is a termination signal that asks the process to exit gracefully. The process has the opportunity to clean up resources and perform shutdown procedures before terminating.

2. `kill -KILL or kill -9` :
	- This sends the `SIGKILL` signal to the target process.
	- `SIGKILL` is a forceful termination signal that immediately terminates the process without giving it a chance to perform any cleanup. It forcefully kills the process.
	- Use this signal when a process is unresponsive or not terminating with `SIGTERM`. However, it should be used as a last resort because it doesn't allow the process to clean up, which may result in data loss or other issues.

**Part 2: Process Management**

3. **Background and Foreground Processes**

1. Start a Long-Running Process in the Foreground: Open your terminal and run a long-running process, such as `sleep 60`, which will sleep for 60 seconds:
	`sleep 60`
	The `sleep` command is running in the foreground, and it will occupy the terminal until it completes.
2. Suspend the Foreground Process and Move it to the Background: While the `sleep` process is running, press `Ctrl+Z`. This will suspend the foreground process and bring you back to the shell prompt.
3. use the `bg` command to move the suspended process to the background.
4. use the `fg` comamnd to bring the background process back to the foreground.
![[Screenshot from 2023-11-03 11-27-13.png]] 

**Difference between Foreground and Background Processes**:

- **Foreground Process** : 
	- A foreground process is a process that is currently running in the terminal and has control over the terminal.
	- It typically blocks the terminal until it completes or is paused (e.g., by pressing `Ctrl+Z`).
	- You interact with foreground processes directly, and they can receive input from the keyboard.
- **Background Process** : 
	-  A background process is a process that is running independently of the terminal and doesn't block it.
	- It allows you to continue using the terminal for other tasks while the background process is running.
	- Background processes are typically used for tasks that don't require your immediate attention or input, such as long-running computations, file transfers, or system maintenance tasks.

**When to use Each** :

- **Foreground Processes** : 
	- Use foreground processes when you need to interact with the process or provide input to it.
	- For tasks where you want to monitor the progress and receive immediate feedback.
- **Background Processes** : 
	- Use background processes for tasks that can run independently and don't require your constant attention.
	- For long-running or resource-intensive tasks that you want to run while you continue working in the terminal.
	- When running tasks remotely, as disconnecting from a session would terminate foreground processes but not background processes.

4. **Process Prioritization**

**1. Start a Process with Lower Priority (Higher Niceness Value):**

The `nice` command is used to modify the scheduling priority of a process. A higher niceness value makes the process less important and lowers its priority.

**sudo nice -n 10 find / -name "*.txt" 2>/dev/null**

In this example, we use `nice -n 10` to set a high niceness value. The process will search for files with the `.txt` extension, but it will do so with lower priority.

![[Screenshot from 2023-11-03 11-50-25.png]]

**2. Monitor the Process with `top` Command:**

Use the `top` command to monitor the system's processes and their resource usage:

**top**

In the `top` display, you can find your `find` process with a higher niceness value. The process name, PID, CPU usage, and priority (NI) will be visible. It should have a positive niceness value, indicating lower priority.

![[Screenshot from 2023-11-03 11-50-37.png]]

**3.How Process Priority affects System Performance** : 

- Process priority, often represented by the niceness value, affects the CPU scheduling of processes in the system.
- A lower niceness value indicates a higher priority, meaning that the process is more important and will be given more CPU time.
- A higher niceness value (positive) means lower priority, and the process is less important, which results in less CPU time.
- Process priority can have a significant impact on system performance. 
- Processes with lower priority consume fewer CPU resources, allowing higher-priority processes to run smoothly.
- When you lower the priority of a process, it's less likely to compete for CPU time with critical or real-time processes. 

**Part 3: Process Monitoring**

5. **Real-time Process Monitoring**

Use the `top` command to monitor the real-time status of processes.

**top**

The `top` command displays a real-time view of the system's processes, resource usage, and system statistics. Here's how to interpret the information displayed:

- **Load Average** : At the top of the `top` display, you'll see the load averages. These values represent the average number of processes that are waiting for CPU time over the last 1, 5, and 15 minutes.
- **Tasks** : This section provides an overview of the number of processes, including running, sleeping, stopped, and zombie processes.
- **CPU** : The CPU section shows the usage of CPU cores. It provides information on the percentage of CPU time used by user processes (`us`), system processes (`sy`), and processes with a lower priority (`ni`). It also displays the percentage of idle time (`id`) and other statistics.
- **Memory** : This section displays memory-related statistics, including the total physical memory, used memory, free memory, buffers, and cache. It also shows the total swap space and its usage.
- **Swap** : The swap section indicates the swap usage, including the total swap space, used swap, and free swap.
- **PID(Procees ID)** : The list of processes is displayed below the system statistics. Each process is identified by its unique PID.
- **USER** : This column shows the username of the owner of the process.
-  **PR (Priority)**: The priority of the process. Lower values indicate higher priority.
- **NI (Nice Value)**: The niceness value of the process, which affects its priority. A higher niceness value means lower priority.
- **VIRT (Virtual Memory)**: The virtual memory used by the process.
- **RES (Resident Memory)**: The resident memory used by the process, which is the portion of virtual memory that is currently in physical RAM.
- **SHR (Shared Memory)**: The shared memory used by the process.
- **S (Status)**: The status of the process, such as `R` for running, `S` for sleeping, `D` for uninterruptible sleep, and others.
- **%CPU**: The percentage of CPU usage by the process.
- **%MEM**: The percentage of physical memory (RAM) used by the process.
- **TIME+**: The total CPU time used by the process since it started.
- **COMMAND**: The name of the command or program associated with the process.

**Interpreting the Information**:

- Look at the CPU and memory sections to identify processes that are using the most CPU and memory. The `%CPU` and `%MEM` columns are particularly useful for this.
- High CPU or memory usage can be an indication of a process consuming a significant amount of resources. You can identify the process in the `COMMAND` column.
- Monitor the load averages to get an idea of the system's overall load.
- You can use the `q` key to exit the `top` command.

6. **Process Information**

To retrieve detailed information about a specific process, you can use the `ps` command with various options. You can use `ps` with the `-p` option followed by the PID of the process you want to examine.

`ps -p 10`

This command will display detailed information about the process with PID 10. You'll see information such as the command that was executed, the status of the process, the user who started the process, and more.

![[Screenshot from 2023-11-03 12-31-53.png]]

If the process you want to inspect is still running and you're not sure about its PID, you can use the `ps aux` command to list all processes and their details, including the PID.

For example:

**ps aux | grep root**

This will display the details of the matching process,including it's PID.


![[Screenshot from 2023-11-03 12-38-10.png]]

**Part 4: Scripting and Automation**

7. **Process Automation**

1. **Write a Bash script that Starts three background processes, each running a different command**.

#!/bin/bash

**Start the first background process**
command1="sleep 10" #sleep for 10 seconds
$command1 &

**Start the second background process**
command2="echo 'this is an example!'"
$command2 &

**Start the third background process**
command3="ls /tmp" #list files in /tmp"
$command3 &

**Redirect top output to a text file**
top -b -n 1  > top_output.txt &

**Analyze the generated text file**
cat top_output.txt

![[Screenshot from 2023-11-03 13-44-44.png]]
In this script:

1. We define three different commands (`command1`, `command2`, and `command3`) that represent the commands you want to run in the background. 
2. We use the `&` symbol to run each command in the background. This allows the script to continue executing without waiting for the commands to complete.
3. After starting each background process, we print a message to indicate the process has started along with its PID.
4. The `top` command is used to monitor the CPU and memory usage of all processes (-b for batch mode, -d for delay, and -n for the number of iterations). The output is redirected to a text file named `top_output.txt`. The `&` at the end runs `top` in the background.

![[Screenshot from 2023-11-03 13-45-19.png]]
