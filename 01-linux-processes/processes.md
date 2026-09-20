# What Processes Are
In Linux, a **process** is an instance of a running program. When you run a command in the terminal, Linux creates a process to execute that command.

Let's continue with an our food analogy. Think of a program as a recipe. when you cook your dish, you are making an instance of that recipe. Similarly, when you run a program, Linux creates a process as an instance of that program to execute it.  
As you can make multiple instances of a recipe, you can also run multiple processes of the same program simultaneously.

Processes are the fundamental building blocks of Linux. They are responsible for executing programs, managing resources, and providing isolation between different tasks running on the system.

If you want to test this, open a terminal and run the following command:

```bash
sleep 1000
```

What happens?
The `sleep` command is a simple program that does nothing for a specified amount of time. In this case, it will sleep for 1000 seconds. When you run this command, Linux creates a process to execute the `sleep` program.

Remember that:
> A program is code on disk. A process is that program currently running.  

Now, open another terminal and run the following command to see all processes running on your system:

```bash
ps aux
```

What do you see?
You should see a line that represents the `sleep` process, along with its process ID (PID) and other information. This confirms that Linux has created a process to execute the `sleep` program.  
Now it is time to learn about process IDs and how Linux manages processes.