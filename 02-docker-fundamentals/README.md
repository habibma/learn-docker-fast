# Docker Fundamentals

The central idea is:

```
Image
  ↓
docker run
  ↓
Container
  ↓
Process
```

If this becomes intuitive, Inception will be much easier. For this goal, we go back the old question and we review the answer.

## What Problem Is Docker Solving?

Before Docker, suppose you have a program that needs:

```
my-program
├── specific libraries
├── specific configuration
├── specific filesystem
└── specific environment
```

You want to run that program somewhere without it interfering too much with the rest of your system.

Docker gives you a way to **package** the environment and run the program in an **isolated environment**.

A simplified picture:

```
Your Linux machine
│
├── normal programs
│
├── Docker
│    │
│    ├── Container A
│    │    └── process
│    │
│    └── Container B
│         └── process
│
└── other things
```

Remember that _DON'T_ think of Docker as a virtual machine. A container is not a small virtual computer.

It is much closer to:

A process running with isolation around its filesystem, networking, processes, etc.

We'll eventually get deeper into how this works.

## Lessons
[Images](./images.md)  
[Containers](./containers.md)  
[Images vs Containers](./images-vs-containers.md)
[Docker CLI](./docker-cli.md)