# Code Journey - Beyond deployment
## Introduction
DevOps practices go beyond deploying applications in an automated and collaborative manner. As applications mature, get deployed often, and bugs and new features arise, it becomes important to maintain them and test them over time.

Two important testing methods are performance testing, and regression testing. Performance testing is important because as a non-functional test, it allows us to validate our infrastructure and code quality, and if they are prepared for a high load of users, and if the code has an acceptable performance overall.

Regression testing is equally important because it allows us to determine if the changes we do over time have any negative effect in our application.

An ideal use case for testing performance and regression is to capture real-life data from real usage, store it, and use it for testing later. That’s ideal because instead of using an approximation of data, or a fake scenario (like sending 1000 requests per second perfectly), we are using real application load in a situation that really happened. It’s desirable to test using production-accurate data.

For this we will use an emerging technology called eBPF. In very simple terms, eBPF is a technology that allows programs to run safely in the kernel of the operating system. Due to the emergent popularity of the technology, there are many modern toolkits and SDKs for eBPF, such as bcc.

The main advantage to use eBPF in this case is that we can look directly at the operating system and its processes, sockets, stats... in a generic manner, instead of having to do it on a per-application basis (for example, writing an Nginx plug-in to save all of the requests made to a file, and then having to write a similar interceptor for a Java application that uses Tomcat server, another one for a Python application running Flask... and so on).

## Problem Description
The goal of this project is to develop an application leveraging eBPF technologies, to integrate in a larger DevOps workflow process.

The application will run on one or several hosts, alongside a normal API, and will capture all HTTP requests sent to it in a specific port, using eBPF. Those applications are typically written in C, and with a Python wrapper. After capturing the requests, it will save them to a file or database with a corresponding timestamp and all the information in the request body.

The requests for testing can be done by hand.

Then, the complete list of requests will be used in a stress testing tool context, to send those requests to another host.

And that process will be integrated in Jenkins to be triggered on-demand. This creates a workflow in which we are able to test an application by feeding it real requests from real-world usage.

For the hosts sending and receiving hosts, you can use a Docker container with any sample API and the eBPF application. Or you can use linux-based Virtual Machines. They will be provided if necessary.

## Extra points
Having the eBPF application completely automated to capture requests at a certain time would save on space and manual operations. That can be done inside the application itself, or in a cron job.

Saving the requests to any database instead of a file would also be good for portability, and to ensure that the list of requests could be used anywhere and at any time.

## Required tools
- [Docker](https://www.docker.com/)
- [bcc](https://github.com/iovisor/bcc)
- [Locust](https://locust.io/)

## Useful links
- [Introduction to eBPF](https://ebpf.io/what-is-ebpf/)
- [bcc README with tutorials and examples](https://github.com/iovisor/bcc/blob/master/README.md)
- [Learn eBPF tracing](https://www.brendangregg.com/blog/2019-01-01/learn-ebpf-tracing.html)
- [A practical guide to capturing production traffic with eBPF](https://www.datadoghq.com/blog/ebpf-guide/)
- [Locust quick start](https://docs.locust.io/en/stable/quickstart.html)
- [Sample API using Python + Flask](https://github.com/docker/awesome-compose/tree/master/flask)