
AmigOS - Operating System Simulator

AmigOS is a terminal-based operating system simulator written in C that runs on Linux and Ubuntu. It is built to demonstrate how a real operating system works by using actual Linux system calls rather than just simulating them in theory.

When you run AmigOS it boots with an animated loading screen and then presents a main menu where you can launch 20 different programs. Each program you launch becomes a real Linux process created with fork and exec. The OS tracks every process, manages resources, schedules CPU time, and lets you control processes just like a real operating system would.

Process Management

Every task in AmigOS runs as a real child process. When you launch a task the OS forks itself, the child sends its resource requirements to the parent through a pipe, and if there is enough RAM and HDD space available the task is executed. Each process has its own Process Control Block that stores its PID, state, memory usage, queue level, and priority.

Scheduler

AmigOS uses a multilevel queue scheduler with three levels. Foreground tasks run in the first level using Round Robin scheduling with a 500 millisecond time slice. Background tasks run in the second level using First Come First Served. System tasks run in the third level using Priority scheduling where the lowest priority number runs first. The scheduler runs on its own thread and wakes every 200 milliseconds to check the queues.

Memory and Resource Management

RAM and HDD usage is tracked using POSIX shared memory so all processes can read the resource table at the same time. If you try to launch a task and there is not enough RAM or HDD space available the OS will deny the request and tell you to close other tasks first.

Synchronisation

The OS uses mutex locks to protect the scheduler queues and resource counters from race conditions. A semaphore tracks how many CPU cores are available. A condition variable puts the scheduler thread to sleep when all queues are empty and wakes it when a new process arrives.

Kernel Mode and User Mode

By default you are in user mode where you can launch tasks and check status. Typing "kernel" and entering the password switches you to kernel mode where you can see full process details, kill any process, pause and resume processes, and inspect memory. This simulates the privilege separation between user space and kernel space in a real OS.

Context Switching

Pausing a process sends it SIGSTOP and resuming it sends SIGCONT. Closing a task sends SIGTERM. This is how the OS simulates context switching and interrupt handling using real Linux signals.

Tasks

There are 20 tasks split into three categories. Foreground tasks need user input and include a calculator, notepad with auto-save, file manager, text search, random number generator, word counter, file encryptor, countdown timer, minesweeper, and snake. Background tasks run on their own and include a music simulator, print job simulator, log generator, auto backup system, and disk cleaner. System tasks display information and include a real-time clock, calendar, RAM usage viewer, process list viewer, and system info display.

How to Build and Run

Install GCC and make, then run "make" inside the project folder. Launch the OS with "./OS 2048 256000 8" where the three numbers are RAM in MB, HDD in MB, and number of CPU cores. Type a number from 1 to 20 to launch any task. Type "exit" inside a task to go back to the menu. Type "shutdown" to close AmigOS.
