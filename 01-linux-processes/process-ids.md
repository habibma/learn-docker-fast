# Process ID == PID

Every process in Linux is assigned a unique identifier called a **Process ID** or **PID**. The PID is used by the operating system to manage and track processes. It allows the system to differentiate between different processes, even if they are running the same program.

PID is a useful integer to reference a process.  
If you "sleep" pragram is still running, find its PID by running the following command in another terminal:

```bash
ps aux | grep sleep
```

And let's terminate it by running the following command in another terminal:

```bash
kill <PID>
```
When you want to terminate a process, you can use its PID after the `kill` command.


## Hand's on
Open a terminal and run the following command to see what happens:

```bash
sleep 1000
```

by running this command, you are creating a process that will sleep for 1000 seconds.

Now press `Ctrl + Z` to pause the process. You will see a message like this:

```
[1]+  Stopped                 sleep 1000
```

This time try this command:

```bash
sleep 1000 &
```

What happened?

Explore processes by running the following command:

```bash
ps aux | grep sleep
```

Still runnig, right?

You can terminate it by running the following command in the same terminal:

```bash
kill <PID>
```

This parctice helps you to understand what Foreground and Background processes are and how to manage them using their PIDs. Running sleep using `&` at the end of the command makes it run in the background, allowing you to continue using the terminal for other commands.

Remember that `kill` command is used to terminate processes. You ask Linux to terminate a process politely by sending a signal `SIGTERM`. Signal `SIGTERM` means " Please terminate yourself."

If a process does not terminate after receiving the `SIGTERM` signal, you can use the `SIGKILL` signal to forcefully terminate it. You can do this by running the following command:

```bash
kill -9 <PID>
```

## Summary

```
sleep program starts

       ↓

Linux creates process

       ↓

process gets a PID

       ↓

process runs for 5 seconds

       ↓

sleep exits

       ↓

process terminates

       ↓

shell gets control back
```
Now you have a basic understanding of what a process is, how Linux assigns PIDs, and how to manage processes using their PIDs. Without any hesitation, go for learning Docke Fundamentals.