# TinyShell-Posix-Process-Controller

## Overview
This project is a custom implementation of a minimalist Unix shell, designed to explore the intricacies of the POSIX operating system API. The goal was to build a robust "Stage Manager" for system processes, handling the creation, execution, and termination of programs at a low level.

Unlike standard shells that rely on high-level abstractions, this implementation interacts directly with the kernel to manage process groups, handle asynchronous signals, and prevent concurrency race conditions.

## Key Features
* **Process Creation:** Implements a custom `eval` loop using `fork()` and `execve()` to spawn child processes.
* **Job Control:** Full support for **Foreground (FG)** and **Background (BG)** job execution using the `&` operator.
* **Signal Handling:** A custom signal traffic controller that intercepts `SIGINT` (Ctrl+C) and `SIGTSTP` (Ctrl+Z) and forwards them correctly to foreground process groups.
* **Zombie Reaping:** An asynchronous `SIGCHLD` handler that cleans up terminated processes using non-blocking waits (`WNOHANG`) to prevent resource leaks.
* **Built-in Commands:** Custom implementations of `quit`, `jobs`, `bg` (background), and `fg` (foreground).

## Technical Highlights
This project demonstrates practical application of the following concepts:
* **Concurrency & Synchronization:** Blocking specific signals (like `SIGCHLD`) during critical sections to prevent race conditions between the main shell loop and signal handlers.
* **Inter-Process Communication (IPC):** Managing process groups (`setpgid`) to ensure signals are routed to the correct hierarchy of processes.
* **System Calls:** Direct usage of `waitpid`, `kill`, `sigprocmask`, and `sigaction`.

## Compilation & Usage
To build the shell, run:
```bash
make
```

To start the shell:
```Bash
./tsh
```

Example Session
```Plaintext
tsh> ./myspin 1 &
[1] (26252) ./myspin 1 &
tsh> jobs
[1] (26252) Running ./myspin 1 &
tsh> quit
```

Future Improvements
- Implementation of I/O redirection (`<`, `>`).
- Support for piping (`|`) to chain processes.
