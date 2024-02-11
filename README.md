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

* Below are the important log parsing and processing commands which are highly helpful (all credits goes to ByteByteGo) <br/>

<img width="357" alt="image" src="https://github.com/somrajroy/KubernetesApplicationsDebuggingandLogging/assets/92582005/bd34b89a-4653-46ea-92a4-2563c4e8c633"> <br/>


* Kubernetes is the de-facto standard for container orchestration. It provides the required abstraction for efficiently managing large-scale containerized      applications with declarative configurations, an easy deployment mechanism, and both scaling and self-healing capabilities. However it also introduces new challenges due to its ephemeral and dynamic nature. One of the main challenges is how to centralize Kubernetes logs. <br/>
* Machines in the Cloud are ephemeral. As more companies run their services on containers and orchestrate deployments with Kubernetes, logs can no longer be stored on machines and implementing a log management strategy is of the utmost importance. Logs are an effective way of debugging and monitoring applications, and they need to be stored on a separate backend where they can be queried and analyzed in case of pod or node failures. These separate backends include systems like Elasticsearch, GCP’s Stackdriver, and AWS’ Cloudwatch. This blog does not talk about storing logs but focuses more on Kubernetes application which can be leveraged to fix problems. That would also demostrate how logs are important.<br/>
* In Kubernetes, when pods are evicted, crashed, deleted, or scheduled on a different node, the logs from the containers are gone. The system cleans up after itself. Therefore DevOps engineers lose any information about why the issue/anomaly occurred. This transient nature makes it crucial to implement a centralized log management solution. <br/>
# Importance of Logging and Debugging
At the core of every successful Kubernetes deployment lies a robust logging strategy. Application logs are not mere strings of text; they hold the key to unlocking valuable insights into system behavior, performance bottlenecks, and potential security vulnerabilities. By capturing structured logs, developers and architects can effectively monitor, troubleshoot, and optimize applications running on Kubernetes clusters.<br/><br/>
But logging is just one part of the equation. In the dynamic, transient and distributed world of Kubernetes, where applications span multiple containers and nodes, the need for rapid debugging is inevitable. Debugging is the process of identifying and fixing issues in a Kubernetes application. It is a critical part of maintaining the health and performance of Kubernetes applications. The highly distributed nature of Kubernetes makes traditional approaches to debugging less applicable. This comprehensive blog with equip architects and developers to effectively deal with Kubernetes applications. <br/>
# Introduction to Kubernetes Logging

Application logging is a critical aspect of monitoring and troubleshooting applications running in Kubernetes clusters. Proper logging practices enable developers and system administrators to:

# Kubernetes Application Logs <br/>
Application logs in Kubernetes are crucial for diagnosing issues, monitoring application behavior, and gaining insights into the health and performance of the applications. Containers typically write logs to standard output (stdout) and standard error (stderr) streams. Kubernetes captures logs from each container within a running Pod. <br/>

## Log Sources <br/>

# Kubernetes Events
* In Kubernetes, kubectl events is a command-line tool that allows to view events generated by various resources in the kubernetes cluster. These events provide insights into the state and changes within the Kubernetes cluster. Events serve as real-time notifications about various changes, updates, and issues affecting the Kubernetes objects (pods, deployments, nodes, etc.). They offer valuable insights into the health and performance of the cluster, aiding in monitoring, troubleshooting, and understanding overall cluster activity. <br/>
* There are several types of events that can be reported by Kubernetes, and they are categorized by their type field. The common event types include:<br/>
1. Normal: Indicate successful operations like pod creation, container starting, or node becoming ready. These events represent normal and expected behavior. <br/>
2. Warning: Notify of potential issues, such as low disk space, pod evictions,a pod failing to start, a resource running out of resources, or container restarts exceeding a threshold. These indicate potential issues or warnings. <br/>
3. Error: Error: These events signify errors or problems that need attention. Examples include a pod crashing or a resource entering a failed state. These signify errors or problems that need attention. <br/>
4. Volume events, node events etc - please check the references <br/>

## Purpose and Utilization <br/> 
* Kubectl events are records of the state changes within a Kubernetes cluster. They provide valuable information about the activities and status of resources in the cluster. The purpose of kubectl events is to help administrators and developers monitor the health and performance of their applications running in Kubernetes.<br/>
* To utilize events in Kubernetes leverage `kubectl` which provides a unified way to manage and access the kubernetes cluster. `kubectl events` is a command-line tool in Kubernetes that helps monitor and diagnose cluster health by displaying events associated with various cluster objects (pods, deployments, nodes, etc.). These events provide details about resource creation, updates, errors, and warnings, giving valuable insights into the cluster's state and activity. <br/>
* The purpose of kubectl events is to provide a real-time stream of events associated with different objects in the cluster. These events can include information about resource creation, deletion, errors, warnings, and other changes. By examining events, one can gain visibility into what is happening within the cluster and identify potential issues or unexpected behavior. hey offer valuable insights into the health and performance of the cluster, aiding in monitoring, troubleshooting, and understanding overall cluster activity. These events are a form of metadata related to the pods, jobs, nodes, and other kubernetes resources. They capture various activities and document the changes that occur inside the cluster. Viewing stored events can explain problems and help resolve failures.<br/>
* Kubernetes Events, are in a way, logs in themselves. An event is a Kubernetes resource/object. that occurs when a change happens with another Kubernetes resource/object whether it’s Pods, Services, Nodes, etc. It includes information about errors and changes to resource state. For example, events may include scheduler decisions and reasons for pod deletion.<br/>
* Events are API objects stored on the API server. By default, Kubernetes drops event data 60 minutes after events are fired, so customers need to have a mechanism for storing event data in a persistent location.<br/>

## kubectl commands <br/>
* `kubectl events` is primarily a monitoring and diagnostics tool. It leverages events emitted by Kubernetes objects (not custom components) to provide insights into cluster activity <br/>
* `kubectl get events`: Lists all events in the current namespace.<br/>
* `kubectl get events --all-namespaces`: List events across all namespaces.<br/>
* `kubectl describe event <event_name>`: View detailed information about a specific event.<br/>
* `kubectl -n abc-namespace events --for pod/web-pod-13je7 --watch` : To filter events for a specific pod and watch for new events <br/>
* `--watch` : Stream events as they occur in real-time <br/>
* `--field-selector=<key>=<value>`: Filter by specific fields (e.g., `--field-selector involvedObject.kind=Pod`).<br/>
* `--sort`: Sort events by different criteria (e.g., `--sort=-lastTimestamp`).<br/>
* `--output=<format>`: Specify output format (e.g., `--output=json`)<br/>
* List only events of type ‘Warning’ or ‘Normal’: `kubectl events --types=Warning,Normal`<br/>
* `kubectl get events --field-selector type=Warning` : filter events based on types to focus on specific aspects of the cluster <br/>
* `kubectl events -o yaml` : List recent events in YAML format <br/>

### Further tips to effectively utilize events <br/>
* Use namespaces to organize events for large clusters or if there are multiple applications and different clusters. I had this in my last project and ensured that customer has different clusters. Consider creating separate namespaces for different environments (e.g., development, staging, production). Namespace isolation ensures that events are neatly categorized and don’t overlap. Organizing events with namespaces is a crucial point for clarity and managing large clusters. <br/>
* Combine kubectl events with other tools like `kubectl describe`, `kubectl logs`, and `kubectl get` for deeper troubleshooting. <br/>
* Leverage monitoring tools that aggregate and analyze events for broader cluster inights. <br/>
* By effectively utilizing kubectl events, one can gain valuable insights into Kubernetes cluster's health, troubleshoot issues faster, and ensure the smooth operation of applications. <br/>
* Set up RBAC (Role-Based Access Control) for Event Monitoring : Ensure that access to `kubectl events` and other related commands is restricted based on the principle of least privilege. Set up RBAC rules to control who can access event information, helping to maintain security and prevent unauthorized access to critical cluster data. Security considerations and RBAC setup adds a significant dimension. <br/>
* Use Labels and Annotations for Enhanced Event Context & enriching events with labels/annotations: When creating resources in Kubernetes, consider using labels and annotations to add additional context to events. This will make it easier to filter and understand events related to specific applications, environments, or other custom criteria, providing more detailed insights. <br/>
* Tailing events for real-time updates: Execute `--watch` for continuous monitoring. <br/>
* Filtering by severity: Highlighting filtering by severity (`--field-selector=level=Warning`) could aid troubleshooting.

## Links/Guide to Kubernetes Events <br/> 

Below are some resources and references to learn more about events : <br/>

1. [How to Use Kubectl Get Events - A Complete Guide](https://humalect.com/blog/kubectl-get-events)<br/>
2. [Types of Kubernetes Events](https://www.bluematador.com/blog/kubernetes-events-explained)<br/>
3. [Understanding Kubernetes Events: A Guide](https://www.kosli.com/blog/understanding-kubernetes-events-a-guide/)<br/>
4. [How to Watch Kubernetes Events](https://thechief.io/c/editorial/how-watch-kubernetes-events/)<br/>
5. [Kubernetes events for troubleshooting](https://learn.microsoft.com/en-us/azure/aks/events?tabs=azure-cli)<br/>
6. [Events : Official Documentation](https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/event-v1/)<br/>
7. [A complete guide to Kubernetes events](https://www.airplane.dev/blog/kubernetes-events)<br/>
8. [How to use Kubernetes events for effective alerting and monitoring](https://www.cncf.io/blog/2023/03/13/how-to-use-kubernetes-events-for-effective-alerting-and-monitoring/)<br/>
9. [How to Use Kubectl Get Events](https://www.learnitguide.net/2023/04/how-to-use-kubectl-get-events.html)<br/>

# Kubectl "exec"
* [The kubectl exec command](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec) allows to start a shell session inside containers running in Kubernetes cluster. This command allows to inspect the container’s file system, check the state of the environment, and perform advanced debugging tools when logs alone don’t provide enough information.<br/>
* "kubectl exec" lets developers specify the container to connect to without worrying about the Kubernetes node it’s on. This is the biggest strength as there is no need to know the node IP (details). (With SSH node details are needed)<br/>
* By running the shell commands, one can see the container’s entire file system and check if the environment is as expected. It can also help identify whether a critical file is missing or locked, or find instances of misconfigured environment variables.<br/>
*   Below is the format of exec command. One can use "-i" and "-t" flags with it. These two flags combined (-it) allows to execute commands inside the container but from  own local terminal. This means if someone running something like "ls" s/he will see the files in the container and not on his/her system where the terminal is actually running. In below command "sh" can also be used instead of "bash". <br/>
        * kubectl exec -it <POD_NAME> -n <NAME_SPACE>  -- bash <br/>
# Application Logging - stdout & stdin
In all container-based applications (not just kubernetes) it is best practice to direct application logs to stdout/stderr  standard output streams. Kubernetes natively captures and manages these streams, making log collection and aggregation straightforward. There is no worry about losing these logs, as kubelet, Kubernetes’ node agent, will collect these streams and write them to a local file behind the scenes, so that it can be accessed with Kubernetes.<br/>
Directing application logs to stdout/stderr is considered a best practice in containerized environments like Kubernetes and when using orchestration tools. Below are the reasons why this practice is recommended.<br/>
* _Reduced operational burden_: Managing log files in a custom folder requires additional administrative tasks, such as monitoring disk usage, ensuring proper permissions, and dealing with data corruption or loss due to hardware failures. Using stdout/stderr eliminates these operational burdens, as logs are automatically managed by the platform. In contrast, logs written to a file can be difficult to access and manage, especially in a distributed environment like Kubernetes. <br/>
* _Containerization Benefits_: When running applications in containers, it is recommended to follow the Twelve-Factor App methodology, which suggests treating logs as event streams and writing them to stdout/stderr. This approach aligns with the principles of containerization, where applications are expected to be stateless and produce logs as part of their standard output. <br/>
* _Standardization & Interoperability_ : Writing logs to stdout/stderr follows the Unix/Linux philosophy of using standard streams for communication. Containers are designed to work with these standard streams, making it easier to collect and manage logs from different containers consistently. Logging to custom folders or files might introduce variations in log formats and make log collection and aggregation more challenging. By adhering to this convention, customers can ensure consistency across different applications and components running in Kubernetes.<br/>
* _Efficient Resource Utilization_: Writing logs to a file can consume a significant amount of disk space (container's filesystem), especially in environments with limited storage. In contrast, logs written to stdout/stderr are ephemeral and do not require persistent storage or  extra disk space or I/O operations. Leveraging stdout/stderr eliminates the need for separate logging infrastructure, reducing overall costs and optimizing resource utilization.<br/>
* _Portability & Dynamic Environments_: By directing logs to stdout/stderr, developers adhere to the principle of treating containers as ephemeral and stateless. With this approach, application logs become part of the container's standard output and error streams, making them easy to capture and transport. This portability allows to run containers on different environments without worrying about specific log file locations or configurations. It simplifies deployment across various platforms and orchestrators like Kubernetes, Docker Swarm, or Rancher. Logs written to stdout/stderr are captured by the container runtime, even if the container is ephemeral, allowing for later analysis. Writing logs to files in custom folders can lead to log loss when containers are removed/deleted.<br/>
* _Efficient Log Aggregation & Centralization & Integration with Ecosystem Tools_ : Redirecting logs to stdout/stderr enables better log aggregation and consolidation. Log aggregation tools, such as Elasticsearch, FluentBit/Fluentd, or Logstash (EFK/ELK stack), can easily collect and parse logs from these standard streams. Centralized log aggregation simplifies the management and analysis of logs, improving operational efficiency. <br/>
* _Easier Debugging_ : Directing logs to stdout/stderr streamlines the debugging process. As logs are written to the standard streams, they are automatically displayed in the container logs, making it convenient to investigate issues during development and troubleshooting. Developers and operators can easily access container logs through commands like kubectl logs in Kubernetes, docker logs in Docker, or using log aggregators. It eliminates the need to ssh into containers or navigate file systems to access application logs, saving time and effort. <br/>
* _Streamlined Containerization_: When using container orchestration platforms like Kubernetes, leveraging stdout/stderr for logs aligns with the platform's conventions and best practices. Kubernetes, for example, treats logs written to stdout/stderr as important signals about the container's health. It allows for log rotation, collection, and integration with other monitoring and logging solutions. Many cloud-native tools and platforms are built with stdout/stderr redirection in mind, making it easier to integrate and leverage their features. <br/>
* _Easy Integration with Kubernetes Logging Solutions_: Kubernetes provides built-in mechanisms to collect logs from stdout/stderr of containers running in pods. These logs can be easily accessed and managed using Kubernetes logging solutions like Fluentd, Fluent Bit, or the Kubernetes API. <br/>
* _Improved scalability_: As applications scale up or down, managing logs in a custom folder becomes increasingly complex. Standardizing logs to stdout/stderr enables easy scaling and minimizes the risk of running out of disk space or experiencing performance degradation due to log volume.

In summary - Directing application logs to stdout/stderr is a widely accepted best practice in containerized environments. By adopting the practice of directing application logs to stdout/stderr, customers can ensure better portability, facilitate log aggregation, simplify debugging, and align with industry best practices. It enhances overall log management and analysis and allows for seamless integration with container orchestration platforms and reduces operational complexities. By adhering to this practice, customer's can make their logging process more efficient, robust, and scalable in their Kubernetes clusters or any other containerized environment. <br/>

# Further references

Below are some additional resources and references for further learning:<br/>

1. [Kubernetes logging best practices](https://www.cncf.io/blog/2023/07/03/kubernetes-logging-best-practices/#:~:text=This%20data%20includes%20information%20about,monitor%20and%20maintain%20application%20health.)<br/>
2. [Bash Scripting Examples](https://www.youtube.com/watch?v=q2z-MRoNbgM)<br/>
3. [Metrics, tracing, and logging](https://peter.bourgon.org/blog/2017/02/21/metrics-tracing-and-logging.html)<br/>
