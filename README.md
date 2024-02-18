# [work in progress] Kubernetes Application Logging, Debugging & Troubleshooting.
## Introduction
Welcome to the one-stop resource — the definitive guide to Kubernetes application logging and debugging. Whether you are a seasoned Kubernetes expert or just starting your journey into containerization, this comprehensive blog will equip you with the knowledge and tools to master the art of logging and debugging in Kubernetes. <br/><br/>
In the world of modern application development, Kubernetes has emerged as the de facto container orchestration platform, offering unparalleled scalability and flexibility. This blog is intended to be a one stop shop for Kubernetes application debugging and logging purposes. This can also serve as guideline to be followed in Kubernetes implementations as it contains industry standard approaches and best practices.<br/> 
This blog will help to effectively debug and analyze kubernetes applications to make things easy for architects/developers to orchestrate containers by Kubernetes. Given that Kubernetes is complex, knowing where to look for that data and how to interpret it can be tricky but very important.<br/><br/>
* Kubernetes clusters consists of multiple layers that need monitoring, each producing different types of logs. This blog will help to troubleshoot and debug Kubernetes applications. There are also various logging tools that integrate natively with Kubernetes to make the task easier. However to be effective in container orchestration using Kubernetes - a good knowledge of Linux is required because it all starts from there. Without Linux its difficult to go far. The good news is that there are lot of free courses to scale up in Linux. Below are some video courses & articles to master it.
    * [Linux crash Course](https://www.youtube.com/playlist?list=PLT98CRl2KxKHKd_tH3ssq0HPrThx2hESW) <br/>
    * [Bash scripting on Linux](https://www.youtube.com/playlist?list=PLT98CRl2KxKGj-VKtApD8-zCqSaN2mD4w) <br/>
    * [Bash Scripting for Beginners](https://www.youtube.com/playlist?list=PLKMOdY6Bhga5fmUcQQwhfL9thR_Yp1hZ7) <br/>
    * [Learn Linux Bash for Free](https://www.youtube.com/playlist?list=PLlrxD0HtieHh9ZhrnEbZKhzk0cetzuX7l)<br/>
    * [Linux Crash Course - Data Streams (stdin, stdout & stderr)](https://www.youtube.com/watch?v=zMKacHGuIHI) : Handling Input/output /error redirections. This is  crucial concept to know. <br/>
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


* Kubernetes is the de-facto standard for container orchestration. However it also introduces new challenges due to its ephemeral and dynamic nature. One of the main challenges is how to centralize Kubernetes logs. Machines in the Cloud are ephemeral. As more companies run their services on containers and orchestrate deployments with Kubernetes, logs can no longer be stored on machines and implementing a log management strategy is important. Logs are an effective way of debugging and monitoring applications, and they need to be stored on a separate backend where they can be queried and analyzed. These separate backends include systems like Elasticsearch, GCP’s Stackdriver, and AWS’ Cloudwatch. This blog does not talk about storing logs but focuses more on reading/viewing application logs which can be leveraged to fix problems. <br/>
* In Kubernetes, when pods are evicted, crashed, deleted, or scheduled on a different node, the logs from the containers are gone. The system cleans up after itself. Therefore DevOps engineers lose any information about why the issue/anomaly occurred. This transient nature makes it crucial to implement a centralized log management solution. <br/>

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
# Read/view logs with kubectl
`kubectl logs` is a command-line tool in Kubernetes that allows developers to retrieve the logs of a specific container within a pod. It provides insights into the runtime behavior of applications, allowing developers and operators to troubleshoot issues, monitor application performance, and gather information about the application's execution.<br/>
### Use cases & purpose
The primary purpose of `kubectl logs` is to facilitate the examination of real-time and historical log data generated by containers. This is crucial for diagnosing problems, understanding application behavior, and gaining visibility into the activities of running applications. `kubectl logs` is primarily a debugging and monitoring tool, but its capabilities extend to both auditing and research purposes. By effectively utilizing logs, customers can gain valuable insights into their Kubernetes cluster and its applications, enhancing security, compliance, and overall performance. <br/>
* `Debigging & examining application behavior`: View the inner workings of the applications, including stdout and stderr messages from containers. Kubectl logs provide real-time insights into the state of cluster, helping to monitor and troubleshoot its health and performance. Use logs to investigate issues and debug problems in cluster. By looking at the output of containers and other components, customers can identify error messages, trace the flow of requests, and understand how applications behaves under different conditions.<br/>
* `Troubleshooting issues`: Uncover the root cause of errors or unexpected behavior by analyzing log messages. Kubectl logs allow to identify potential problems before they escalate. They give context about occurrences within Kubernetes environment.<br/>
* `Monitoring`: Gain insights into overall performance and identify potential problems early. By analyzing logs, one can track pod creations, deletions, errors, node issues, and more. These events act as an invaluable source of information for developers to understand what’s happening under the hood. Customers can set up alerts and notifications based on log data to detect anomalies and take action before issues become critical.<br/>
* `Auditing` : Use logs to review past events and understand how the cluster has changed over time. This can be useful for compliance purposes, as well as for identifying areas where processes or configurations need improvement. Logs provide a historical record of events within the cluster, including changes to resources, deployments, and system activities. Auditing logs can help ensure compliance with security and regulatory requirements, as well as track any modifications or incidents for analysis and improvement.<br/>
* `Compliance`: Many compliance regulations require auditing and record-keeping. By using `kubectl logs` to gather logs from applications and system components, customers can demonstrate compliance with these regulations. <br/>
* `Review past events`: `kubectl logs` allows to access past deployments, container restarts, errors, and other events through historical logs. This can help customers reconstruct timelines, investigate incidents, and understand changes in cluster configuration. <br/>
* `Improvement identification`: Analyzing historical logs can reveal recurring issues, performance bottlenecks, or inefficient configurations. This information helps identify areas for improvement in processes and configurations.<br/>
* `Application insights`: Examining logs provides valuable insights into application behavior, including successful operations, error messages, resource usage, and performance metrics.<br/>
* `Bottleneck identification`: Logs can expose performance bottlenecks or resource constraints affecting application's operation. By analyzing log data, customers can pinpoint bottlenecks and take steps to optimize application's performance.<br/>
* `Informed decision-making`: Through log analysis, customers gain a deeper understanding of application's behavior and its interaction with the underlying infrastructure. This knowledge can empower customers to make informed decisions about future optimizations, resource allocation, and configuration changes.<br/>
### Log Sources
Kubernetes operates at multiple levels, with each generating logs that offer distinct perspectives. `kubectl logs` it retrieves and displays logs from different sources.

* `Container Logs`: These logs are generated by the applications running inside containers. The core source, holding application-specific messages written to standard output (stdout) and standard error (stderr). 
   Developers can capture output from individual containers, including stdout and stderr streams. They can access container logs using the kubectl logs command with the -c flag followed by the name of the 
   container. <br/>
* `Node Logs`: Insights into node health and activity, such as kubelet actions and systemd logs. The `kubectl logs` command is primarily designed for working with pod and container logs, not node logs. <br/>
* `Cluster Logs`: Logs from Kubernetes system components like API server, etcd, and kubelet, shedding light on cluster-wide operations. <br/>

### kubectl logs key commands <br/>

* `kubectl logs <pod_name>`: Retrieve logs from all Containers in a Pod. Without specifying a container, this command fetches logs from all containers within the specified pod. Useful when investigating interactions between multiple containers in a pod. <br/>
* `kubectl logs --follow` & `kubectl logs -f <pod-name>` : Continuously tail the logs for real-time updates. The `-f` or `--follow` flag streams the logs in real-time. Useful for monitoring ongoing activities 
   and changes. <br/>
* `kubectl logs <pod-name> --since=[duration]`: Filter logs based on a specific time period (i.e. fetch logs generated after a specified timestamp.e.g., 5s, 2m, or 3h) . For example `kubectl logs --since=1h <pod-name>` or `kubectl logs --since=15m <podName>` (last 15 mins). <br/>
* `kubectl logs <pod-name> --tail=<number>`: View the last 100 lines of logs for quick insights (adjust the number as needed). `<number>` specifies the number of lines to show. <br/>
* `kubectl logs <pod-name> --timestamps` : Include timestamps in the log output and provides a chronological view of events. <br/>
* `kubectl logs [pod-name] -c [container-name]` : Get logs from a specific container in a pod. Is useful when a pod has multiple containers and developers need logs from a specific one <br/>
* `kubectl logs [pod-name] --all-containers=true` : Get logs for all containers in a pod.<br/>
* `kubectl logs [pod-name] --previous` or `kubectl logs -p [pod-name]`: Get logs for a pod that has completed or failed. The `--previous` flag allows to view the logs of a pod's previous instance, which is 
   useful for understanding the behavior of completed or failed jobs or why a restart occurred.<br/>
* `kubectl logs [pod-name] | grep "search-expression"` : Filter logs using grep. This command pipes the output of kubectl logs into grep to filter for specific log entries. For e.g. `kubectl logs my-app-pod | grep "ERROR"`<br/>
* `kubectl logs -l app=my-app` : Aggregate Logs for a Set of Pods. Using label selectors, one can aggregate logs for a specific set of pods, useful for getting an overview of a service or application. <br/>
* `kubectl logs my-app-pod > my-app-logs.txt` : Exporting Logs for Analysis. For deeper analysis, one might want to export logs to a file or an external analysis tool. <br/>
* Please refer to the [Kubernetes log documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs) to learn more about different flags that can be used.<br/>

# Kubectl "exec"
* [The kubectl exec command](https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/) allows to start a shell session inside containers running in Kubernetes cluster. This command allows to inspect the container’s file system, check the state of the environment, and perform advanced debugging tools when logs alone don’t provide enough information.<br/>
* `kubectl exec` lets developers specify the container to connect to without worrying about the Kubernetes node it’s on. This is the biggest strength as there is no need to know the node IP (details). (With SSH node details are needed)<br/>
* By running the shell commands, one can see the container’s entire file system and check if the environment is as expected. It can also help identify whether a critical file is missing or locked, or find instances of misconfigured environment variables.<br/>
* Please refer to the [Kubernetes exec command documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec) to learn more about different flags that can be used.<br/>
*   Below is the format of exec command. One can use `-i` and `-t` flags with it. These two flags combined (`-it`) allows to execute commands inside the container but from  own local terminal. This means if someone running something like `ls` s/he will see the files in the container and not on his/her system where the terminal is actually running. In below command `sh` can also be used instead of `bash`. <br/>
        * `kubectl exec -it <POD_NAME> -n <NAME_SPACE>  -- bash` <br/>
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
4. [What are Container Runtimes & 3 Types of Container Runtimes](https://humalect.com/blog/container-runtimes)<br/>
