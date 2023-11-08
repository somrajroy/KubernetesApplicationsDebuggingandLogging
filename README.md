# [work in progress] Kubernetes Applications - Logging, Debugging & Troubleshooting.
## Introduction
Welcome to the one-stop resource — the definitive guide to Kubernetes application logging and debugging. Whether you are a seasoned Kubernetes expert or just starting your journey into containerization, this comprehensive blog will equip you with the knowledge and tools to master the art of logging and debugging in Kubernetes. <br/><br/>
In the world of modern application development, Kubernetes has emerged as the de facto container orchestration platform, offering unparalleled scalability and flexibility. As developers and architects delve deeper into the Kubernetes ecosystem, the need for robust application logging and efficient debugging becomes increasingly evident. This open source blog is intended to be a one stop shop for Kubernetes application debugging and logging purposes. This can also serve as guideline to be followed in Kubernetes implementations as it contains industry standard approaches and best practices.<br/> 
This blog will help to effectively debug and analyze kubernetes applications to make things easy for architects/developers to orchestrate containers by Kubernetes. Given that Kubernetes is complex, knowing where to look for that data and how to interpret it can be tricky but very important.<br/><br/>
Kubernetes is a very complex ecosystem. It is highly distribuuted and dynamic. The nodes/pods/containers can be terminated, restarted, or rescheduled at any point in time. This transient and dynamic nature of the system is a challenge in itself. Imagine an intricate web of microservices working in harmony, serving countless requests from users across the globe. In such complex distributed systems, understanding application behavior, detecting errors, and diagnosing issues are paramount for maintaining seamless operations. This is where Kubernetes application logging and debugging come to the rescue.<br/><br/>

* Kubernetes clusters consists of multiple layers that need monitoring, each producing different types of logs. This blog will help to troubleshoot and debug Kubernetes applications by gaining visibility into Kubernetes. There are also various logging tools that integrate natively with Kubernetes to make the task easier.
However to be effective in container orchestration using Kubernetes - a good knowledge of Linux is required because it all starts from there. Without Linux its difficult to go far. The good news is that there are lot of free courses to scale up in Linux. Below are some video courses & articles to master it.
    * [Linux crash Course](https://www.youtube.com/playlist?list=PLT98CRl2KxKHKd_tH3ssq0HPrThx2hESW) <br/>
    * [Bash scripting on Linux](https://www.youtube.com/playlist?list=PLT98CRl2KxKGj-VKtApD8-zCqSaN2mD4w) <br/>
    * [Bash Scripting for Beginners](https://www.youtube.com/playlist?list=PLKMOdY6Bhga5fmUcQQwhfL9thR_Yp1hZ7) <br/>
    * [Learn Linux Bash for Free](https://www.youtube.com/playlist?list=PLlrxD0HtieHh9ZhrnEbZKhzk0cetzuX7l)<br/>
    * [Linux Crash Course - Data Streams (stdin, stdout & stderr)](https://www.youtube.com/watch?v=zMKacHGuIHI) : Handling Input/output /error redirections. This is most important concept to know in Kubernetes logging & debugging<br/>
    * [Learning Path: Kubernetes](https://developer.ibm.com/tutorials/linux-basics-and-commands/)<br/>
    * [Learning Kubernetes](https://developer.ibm.com/series/kubernetes-learning-path/)<br/>
    * [Learn Linux, 101: A roadmap for LPIC-1](https://developer.ibm.com/tutorials/l-lpic1-map/)<br/>
* Additionally one should also get comfortable with containerization concepts and tools like Docker to work in Kubernetes. Containers are the building blocks of Kubernetes, so knowing about Docker is essential. [This YouTube course is very good & can be considered.](https://www.youtube.com/watch?v=RqTEHSBrYFw)<br/>
* Below are the official Kubernetes links which would aid in troubleshooting.
   * [Kubernetes Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)<br/>
   * [Troubleshooting Applications](https://kubernetes.io/docs/tasks/debug/debug-application/)<br/>
   * [Debug Running Pods](https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/)<br/>
   * [Define a Command and Arguments for a Container](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/)<br/>
   * [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container/)<br/>
   * [Some things you didn’t know about kubectl](https://kubernetes.io/blog/2015/10/some-things-you-didnt-know-about-kubectl_28/#attach-to-existing-containers) <br/>
   
* Kubernetes is the de-facto standard for container orchestration. It provides the required abstraction for efficiently managing large-scale containerized      applications with declarative configurations, an easy deployment mechanism, and both scaling and self-healing capabilities. However it also introduces new challenges due to its ephemeral and dynamic nature. One of the main challenges is how to centralize Kubernetes logs. <br/>
* Machines in the Cloud are ephemeral. As more companies run their services on containers and orchestrate deployments with Kubernetes, logs can no longer be stored on machines and implementing a log management strategy is of the utmost importance. Logs are an effective way of debugging and monitoring applications, and they need to be stored on a separate backend where they can be queried and analyzed in case of pod or node failures. These separate backends include systems like Elasticsearch, GCP’s Stackdriver, and AWS’ Cloudwatch. This blog does not talk about storing logs but focuses more on Kubernetes application which can be leveraged to fix problems. That would also demostrate how logs are important.<br/>
* In Kubernetes, when pods are evicted, crashed, deleted, or scheduled on a different node, the logs from the containers are gone. The system cleans up after itself. Therefore DevOps engineers lose any information about why the issue/anomaly occurred. This transient nature makes it crucial to implement a centralized log management solution. <br/>
# Importance of Logging and Debugging
At the core of every successful Kubernetes deployment lies a robust logging strategy. Application logs are not mere strings of text; they hold the key to unlocking valuable insights into system behavior, performance bottlenecks, and potential security vulnerabilities. By capturing structured logs, developers and architects can effectively monitor, troubleshoot, and optimize applications running on Kubernetes clusters.<br/><br/>
But logging is just one part of the equation. In the dynamic, transient and distributed world of Kubernetes, where applications span multiple containers and nodes, the need for rapid debugging is inevitable. Debugging is the process of identifying and fixing issues in a Kubernetes application. It is a critical part of maintaining the health and performance of Kubernetes applications. The highly distributed nature of Kubernetes makes traditional approaches to debugging less applicable. This comprehensive blog with equip architects and developers to effectively deal with Kubernetes applications. <br/>
# Introduction to Kubernetes Logging

Application logging is a critical aspect of monitoring and troubleshooting applications running in Kubernetes clusters. Proper logging practices enable developers and system administrators to:


# Kubernetes Events
* Kubernetes Events, are in a way, logs in themselces. An event is a Kubernetes resource/object. that occurs when a change happens with another Kubernetes resource/object whether it’s Pods, Services, Nodes, etc. It includes information about errors and changes to resource state. For example, events may include scheduler decisions and reasons for pod deletion.<br/>
* Events are API objects stored on the API server. By default, Kubernetes drops event data 60 minutes after events are fired, so customers need to have a mechanism for storing event data in a persistent location.<br/>
# Kubectl "exec"
* [The kubectl exec command](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec) allows to start a shell session inside containers running in Kubernetes cluster. This command allows to inspect the container’s file system, check the state of the environment, and perform advanced debugging tools when logs alone don’t provide enough information.<br/>
* "kubectl exec" lets developers specify the container to connect to without worrying about the Kubernetes node it’s on. This is the biggest strength as there is no need to know the node IP (details). (With SSH node details are needed)<br/>
* By running the shell commands, one can see the container’s entire file system and check if the environment is as expected. It can also help identify whether a critical file is missing or locked, or find instances of misconfigured environment variables.<br/>
*   Below is the format of exec command. One can use "-i" and "-t" flags with it. These two flags combined (-it) allows to execute commands inside the container but from  own local terminal. This means if someone running something like "ls" s/he will see the files in the container and not on his/her system where the terminal is actually running. In below command "sh" can also be used instead of "bash". <br/>
        * kubectl exec -it <POD_NAME> -n <NAME_SPACE>  -- bash <br/>
# Application Logging - stdout & stdin
In all container-based applications (not just kubernetes) it is best practice to direct application logs to stdout/stderr  standard output streams. Kubernetes natively captures and manages these streams, making log collection and aggregation straightforward. There is no worry about losing these logs, as kubelet, Kubernetes’ node agent, will collect these streams and write them to a local file behind the scenes, so that it can be accessed with Kubernetes.<br/>
Directing application logs to stdout/stderr is considered a best practice in containerized environments like Kubernetes and when using orchestration tools. Below are the reasons why this practice is recommended.<br/>
* Standardization & Interoperability : Writing logs to stdout/stderr follows the Unix/Linux philosophy of using standard streams for communication. Containers are designed to work with these standard streams, making it easier to collect and manage logs from different containers consistently. Logging to custom folders or files might introduce variations in log formats and make log collection and aggregation more challenging. By adhering to this convention, customers can ensure consistency across different applications and components running in Kubernetes.<br/>
* Saves disk space & Efficient Resource Utilization: Writing logs to a file can consume a significant amount of disk space (container's filesystem), especially in environments with limited storage. In contrast, logs written to stdout/stderr are ephemeral and do not require persistent storage or  extra disk space or I/O operations. This can help optimize resource usage and prevent storage-related issues. <br/>
* Portability & Dynamic Environments: By directing logs to stdout/stderr, developers adhere to the principle of treating containers as ephemeral and stateless. With this approach, application logs become part of the container's standard output and error streams, making them easy to capture and transport. This portability allows to run containers on different environments without worrying about specific log file locations or configurations. It simplifies deployment across various platforms and orchestrators like Kubernetes, Docker Swarm, or Rancher. Logs written to stdout/stderr are captured by the container runtime, even if the container is ephemeral, allowing for later analysis. Writing logs to files in custom folders can lead to log loss when containers are removed/deleted.<br/>
* Log Aggregation & Centralization & Integration with Ecosystem Tools : Log Aggregation : Redirecting logs to stdout/stderr enables better log aggregation and consolidation. Log aggregation tools, such as Elasticsearch, FluentBit/Fluentd, or Logstash (EFK/ELK stack), can easily collect and parse logs from these standard streams. Centralized log aggregation simplifies the management and analysis of logs, improving operational efficiency. <br/>
* Easier Debugging : Directing logs to stdout/stderr streamlines the debugging process. As logs are written to the standard streams, they are automatically displayed in the container logs, making it convenient to investigate issues during development and troubleshooting. Developers and operators can easily access container logs through commands like kubectl logs in Kubernetes, docker logs in Docker, or using log aggregators. It eliminates the need to ssh into containers or navigate file systems to access application logs, saving time and effort. <br/>
* Streamlined Containerization: When using container orchestration platforms like Kubernetes, leveraging stdout/stderr for logs aligns with the platform's conventions and best practices. Kubernetes, for example, treats logs written to stdout/stderr as important signals about the container's health. It allows for log rotation, collection, and integration with other monitoring and logging solutions. Many cloud-native tools and platforms are built with stdout/stderr redirection in mind, making it easier to integrate and leverage their features. <br/>

In summary - Directing application logs to stdout/stderr is a widely accepted best practice in containerized environments. By adopting the practice of directing application logs to stdout/stderr, customers can ensure better portability, facilitate log aggregation, simplify debugging, and align with industry best practices. It enhances overall log management and analysis and allows for seamless integration with container orchestration platforms and reduces operational complexities. By adhering to this practice, customer's can make their logging process more efficient, robust, and scalable in their Kubernetes clusters or any other containerized environment. <br/>

# Further references

Below are some additional resources and references for further learning:<br/>

1. [Kubernetes logging best practices](https://www.cncf.io/blog/2023/07/03/kubernetes-logging-best-practices/#:~:text=This%20data%20includes%20information%20about,monitor%20and%20maintain%20application%20health.)<br/>
2. [Bash Scripting Examples](https://www.youtube.com/watch?v=q2z-MRoNbgM)<br/>
