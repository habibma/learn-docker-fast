## Before Docker
In the age before Docker, life was really hard for developers and system administrators. Deploying applications was a complex and error-prone process, often leading to the infamous "works on my machine" problem.


## "Works on my machine"
Developers would write code on their local machines, which often had different configurations, libraries, and dependencies than the production servers. When they tried to run their applications in production, they would encounter errors and unexpected behavior, leading to frustration and wasted time.
In such a scenario, developers would often say:
> "It works on my machine!"

## Virtual machines
For avoidance of the "works on my machine" problem, virtual machines (VMs) were introduced. VMs allowed developers to create isolated environments that could mimic production servers.

VMs solved some problems, But they also introduced new challenges. VMs were resource-intensive, slow to start, and required significant overhead to manage. Imagine each time you want to run an application, you have to boot up a full operating system, which takes time and consumes resources.


## Linux namespaces + cgroups
Computer scientists and engineers sought a more efficient solution. They turned to Linux namespaces and cgroups, which provided lightweight isolation for processes without the overhead of full virtual machines.

These were very helpful, but they were not user-friendly. Learning to use namespaces and cgroups required deep knowledge of Linux internals, and managing them was complex and error-prone.

## LXC
LXC (Linux Containers) was a project that aimed to provide a user-friendly way to use Linux namespaces and cgroups. It allowed developers to create lightweight, isolated environments for their applications.
But LXC was still not widely adopted, as it required a steep learning curve and lacked the tooling and ecosystem that developers needed to easily build, ship, and run applications in containers.
Developers need easy-to-use tools.

## Docker
Here comes Docker, which simplified the process of creating, deploying, and managing containers. Docker provided a user-friendly interface and a rich ecosystem of tools that made it easy for developers to work with containers without needing to understand the underlying complexities of Linux namespaces and cgroups.  
> -> Containers became easy to build, ship and run

If you think the technical terms are too much, don't worry. The next lessons will explain them in a simple way.