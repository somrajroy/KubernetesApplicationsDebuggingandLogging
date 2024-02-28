# A practical guide to debugging/troubleshooting kubernetes applications - Mastering Logs, Events & industry practices
## Introduction
Welcome to an in-depth exploration of Kubernetes application logging. This guide will equip anyone with the battle-tested techniques and industry best practices to demystify the inner workings of kubernetes applications.<br/>

This blog will help to effectively debug and analyze kubernetes applications to make things easy for architects/administrators/developers to orchestrate containers by Kubernetes. Given that Kubernetes is complex, knowing where to look for that data and how to interpret it can be tricky but very important. I have applied these techniques/strategies and industry best practices to troubleshoot critical issues resulting in faster resolution times. You will get all the Kubernetes logging & debugging essentials right here and can fix critical Kubernetes issues much faster with this actionable guide. <br/>

# Why master Kubernetes logging?
Below are the crucial benefits if anyone masters the techniques, strategies and industry practices mentioned in this blog (most of which I have applied).

* `Boost debugging efficiency`: Leverage targeted techniques for effective troubleshooting mentioned in this actionable guide. For developers this blog can be a debugging goldmine. <br/>
* `Pinpoint issues rapidly & Bottleneck identification`: Logs expose performance bottlenecks or resource constraints affecting application's operation. By analyzing log data, customers can pinpoint bottlenecks and take steps to optimize application's performance. They help to diagnose problems quicker and with more precision. <br/>
* `Informed decision-making`: Through log analysis, architects/customers gain a deeper understanding of application's behavior and its interaction with the underlying infrastructure. This knowledge can empower customers to make informed decisions about future optimizations, resource allocation, and configuration changes.<br/>
* `Gain deeper application insights`: Understand how applications behave, optimize performance, and ensure smooth operation. <br/>
* Logs also helps in `Application improvement` and `Performance optimization`. <br/>
# Who is this guide for?
This guide is intended to help people at all levels and roles. I have applied most of these techniques/strategies in real-time projects & have witnessed first-hand the positive impact they can have on project outcomes.  <br/>
1. Kubernetes developers/admistrators and enthusiasts. <br/>
2. Architects & customers looking for `Application improvement` and `Performance optimization`.
3. Architects/Delivery Managers/Customers : The industry practices and tips/tricks can help in streamlining deployment strategies resulting in reduced downtime, improved application performance, and enhanced overall user experience.<br/>
4. `People working on Proposals/RFP's` that involve Kubernetes: Strengthen proposals with best industry pratices and actionable insights. This guide may belp in creating differentiated offerings with industry best practices that ensure peace of mind for customers which will help their responses stand out from the competition. <br/>
5. `Good Industry Practices` : This blog incorporates/mentions industry best practices, offering a benchmark for teams to align with and implement in their projects. <br/>
6.  `Shorter Learning Curve` : Practical insights and hands-on strategies provide conceptual clarity which will help accelerating the learning curve for anyone. <br/>

# Linux and Docker
To be effective in container orchestration using Kubernetes - a good knowledge of Linux & Docker is required because it all starts from there. Without Linux its difficult to go far. Don't worry - learning curves won't hold you back! I've curated a set of free, high-quality resources to jumpstart your Linux and Docker skills. Below are the video courses & articles to master them.<br/>
    * [Linux crash Course](https://www.youtube.com/playlist?list=PLT98CRl2KxKHKd_tH3ssq0HPrThx2hESW) <br/>
    * [Bash scripting on Linux](https://www.youtube.com/playlist?list=PLT98CRl2KxKGj-VKtApD8-zCqSaN2mD4w) <br/>
    * [Bash Scripting for Beginners](https://www.youtube.com/playlist?list=PLKMOdY6Bhga5fmUcQQwhfL9thR_Yp1hZ7) <br/>
    * [Learn Linux Bash for Free](https://www.youtube.com/playlist?list=PLlrxD0HtieHh9ZhrnEbZKhzk0cetzuX7l)<br/>
    * [Linux Crash Course - Data Streams (stdin, stdout & stderr)](https://www.youtube.com/watch?v=zMKacHGuIHI) : Handling Input/output /error redirections. This is  crucial concept to know. <br/>
    * [Learning Path: Kubernetes](https://developer.ibm.com/tutorials/linux-basics-and-commands/)<br/>
    * [Learning Kubernetes](https://developer.ibm.com/series/kubernetes-learning-path/)<br/>
    * [Learn Linux, 101: A roadmap for LPIC-1](https://developer.ibm.com/tutorials/l-lpic1-map/)<br/>
* Additionally one should also get comfortable with containerization concepts and tools like Docker to work in Kubernetes. Containers are the building blocks of Kubernetes, so knowing about Docker is essential.  [This YouTube course is very good & can be considered.](https://www.youtube.com/watch?v=RqTEHSBrYFw)<br/>
* Below are the official Kubernetes links which would aid in troubleshooting. Master debugging and troubleshooting with these authoritative guides. You would need the concepts in rest of the blog. <br/>
   * [Kubernetes Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)<br/>
   * [Troubleshooting Applications](https://kubernetes.io/docs/tasks/debug/debug-application/)<br/>
   * [Debug Running Pods](https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/)<br/>
   * [Define a Command and Arguments for a Container](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/)<br/>
   * [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container/)<br/>
   * [Some things you didn’t know about kubectl](https://kubernetes.io/blog/2015/10/some-things-you-didnt-know-about-kubectl_28/#attach-to-existing-containers) <br/>

* Below are the important Linux log parsing and processing commands which are highly helpful (all credits goes to ByteByteGo). Kubernetes operates within Linux environment, and understanding its core concepts, commands, and navigation is crucial for effective troubleshooting and configuration. Docker, as the containerization technology, forms the building blocks of the applications within Kubernetes. Hence it is important to learn both. <br/>

<img width="357" alt="image" src="https://github.com/somrajroy/KubernetesApplicationsDebuggingandLogging/assets/92582005/bd34b89a-4653-46ea-92a4-2563c4e8c633"> <br/>

# What is a log and why logging is important in Kubernetes
A log is a text record of an event or incident that took place at a specific time. Logs, in the context of  computing and software, is essentially a chronological record of events, activities and messages generated by an application, system, or network device. It's like a digital diary that captures what happened, when it happened, and by whom (if applicable).  It's like a digital diary that captures what happened, when it happened, and by whom (if applicable). Imagine it as a backstage pass to Kubernetes clusters, offering insights into its inner workings.  <br/><br/>
Logging in Kubernetes is not just a best practice; it's a fundamental necessity. Logs are essential for understanding the behavior, performance, and state of a system. Details are mentioned in this blog.<br/>
# Types of Logs in Kubernetes
In Kubernetes, various types of logs provide insights into different aspects of the cluster and application lifecycle. Understanding logs holds the key to unlocking the health and behavior of applications & clusters. While seemingly simple, Kubernetes offers different types of logs, each with a unique purpose and utility.<br/>

* `Application Logs` : These logs are generated by the applications running inside containers & represent the specific messages generated by application's code, offering insights into its behavior, performance, and errors.. They are useful for understanding the behavior of the applications and are typically written to standard output and standard error streams. Application logs can be accessed using `kubectl logs` command and are essential for debugging and monitoring application behavior. These logs capture information related to the behavior, events, errors, and other runtime details of the specific application or service deployed as part of a containerized workload.  <br/>
* `Container logs` : These are the logs that are produced by the processes running inside a container. They include both application logs and any other output from the container's processes, such as system messages, start-up scripts or errors from the container runtime itself. Includes the output of both the application (stdout) and potential errors (stderr) produced by the containerized process. Stored in a location specified by the container runtime, commonly in the `/var/log/containers/` directory.<br/>
* `Pod Logs` : Combine the logs from all containers running within a pod. Encompass the entire pod, representing a logical application unit. Aggregated logs from all containers within a pod. When we run `kubectl logs` for a pod, it aggregates the logs from all containers within that pod. Provides an overall picture of application activity across its constituent containers. <br/>
* `Events` : Record changes or state transitions in the cluster, such as pod creations, deletions, or changes in resource configurations. Kubernetes keeps track of `events`, which can be normal changes to the 
  state of an object in a cluster (such as a container being created or starting) or errors (such as the exhaustion of resources). Valuable for auditing, tracking changes over time, and understanding the 
  historical state of the cluster. Events are critical for troubleshooting and identifying issues. <br/>
* `Node Logs` : Capture logs from the host machine (node - where the containers run), including kernel logs, systemd, kubelet, Docker engine, system daemons, and Kubernetes components (and node-level services). 
   Useful for diagnosing issues at the node level, understanding system-level events, and monitoring the overall health of the node. <br/>
* `Audit Logs` : Record API requests made to the Kubernetes API server, capturing activities related to resource creation, modification, and deletion. Records of API requests, user actions, and security-related 
   events. Essential for security and compliance purposes. Audit logs provide a detailed trail of actions taken within the Kubernetes cluster, aiding in forensic analysis and compliance audits. <br/>
* Other than these there are some more like Controller Manager Logs, API Server Logs, scheduler logs etc which are rarely used. <br/>

## Difference between application logs, container logs and pod logs
Application logs are a subset of container logs, which, in turn, are part of the aggregated pod logs. Application logs specifically pertain to the output and information produced by the application or service running inside a container. Container logs encompass both application logs and other runtime details, while pod logs offer a holistic view of the logs from all containers within a pod. <br/>
* `Application logs` capture messages directly from application code. Consider application logs as the specific messages application produces, offering focused insights into its behavior. They contain 
   information that the applications output as they run, typically written to stdout or stderr. Consider application logs as the specific messages the application produces, offering focused insights into its 
   behavior. Application logs are usually written to stdout or stderr streams by the application itself. <br/>
* `Container Logs` : Logs generated by an individual container within a pod & encompasses application logs and other container-related output. These logs are produced by the processes running inside a container. 
  They include both application logs and any other output from the container's processes, such as system messages or errors from the container runtime itself. These logs are captured by Kubernetes and include 
  the output of a container's stdout and stderr streams. Kubernetes captures logs from each container in a running Pod and makes them available to clients via a special feature of the Kubernetes API. Think of 
  container logs as a broader view of what happens within a container, including application logs.<br/>
* `Pod Logs` : Pod logs aggregate the logs from all containers within a pod, but might not always reflect pure application messages. Can be retrieved using the `kubectl logs` command without specifying a 
  container, as it fetches logs from all containers within the pod. Use pod logs to get a holistic perspective on an application unit, but be aware it might include non-application messages from other 
  containers. Pod logs are useful for monitoring the overall behavior of a pod, which may include multiple application containers. <br/>

In summary, application logs are a subset of container logs, which, in turn, are part of the aggregated pod logs. Application logs specifically pertain to the output and information produced by the application or service running inside a container. Container logs encompass both application logs and other runtime details, while pod logs offer a holistic view of the logs from all containers within a pod.<br/>

# Kubernetes Events
* In Kubernetes, kubectl events is a command-line tool that allows to view events generated by various resources in the kubernetes cluster. These events provide insights into the state and changes within the Kubernetes cluster. Events serve as real-time notifications about various changes, updates, and issues affecting the Kubernetes objects (pods, deployments, nodes, etc.). They offer valuable insights into the health and performance of the cluster, aiding in monitoring, troubleshooting, and understanding overall cluster activity. <br/>
* Events are API objects stored on the API server. By default, Kubernetes drops event data 60 minutes after events are fired, so customers need to have a mechanism for storing event data in a persistent location (if required). <br/>
* There are several types of events that can be reported by Kubernetes, and they are categorized by their type field. The common event types include:<br/>
1. `Normal`: Indicate successful operations like pod creation, container starting, or node becoming ready. These events represent normal and expected behavior. <br/>
2. `Warning`: Notify of potential issues, such as low disk space, pod evictions,a pod failing to start, a resource running out of resources, or container restarts exceeding a threshold. These indicate potential issues or warnings. <br/>
3. `Error`: Error: These events signify errors or problems that need attention. Examples include a pod crashing or a resource entering a failed state. These signify errors or problems that need attention. <br/>
4. `Volume events, node events etc` - please check the references <br/>

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

### Tips/tricks to effectively utilize events <br/>
* `Use namespaces to organize events for large clusters or if there are multiple applications and different clusters`. I had this in my last project and ensured that customer has different clusters. Consider creating separate namespaces for different environments (e.g., development, staging, production). Namespace isolation ensures that events are neatly categorized and don’t overlap. Organizing events with namespaces is a crucial point for clarity and managing large clusters. <br/>
* Combine kubectl events with other tools like `kubectl describe`, `kubectl logs`, and `kubectl get` for deeper troubleshooting. <br/>
* Leverage monitoring tools that aggregate and analyze events for broader cluster inights. By effectively utilizing kubectl events, one can gain valuable insights into Kubernetes cluster's health, troubleshoot issues faster, and ensure the smooth operation of applications. <br/>
* `Set up RBAC (Role-Based Access Control) for Event Monitoring` : Ensure that access to `kubectl events` and other related commands is restricted based on the principle of least privilege. Set up RBAC rules to control who can access event information, helping to maintain security and prevent unauthorized access to critical cluster data. Security considerations and RBAC setup adds a significant dimension. <br/>
* `Use Labels and Annotations for Enhanced Event Context & enriching events with labels/annotations`: When creating resources in Kubernetes, consider using labels and annotations to add additional context to events. This will make it easier to filter and understand events related to specific applications, environments, or other custom criteria, providing more detailed insights. <br/>
* `Tailing events for real-time updates`: Execute `--watch` for continuous monitoring. <br/>
* `Filtering by severity`: Highlighting filtering by severity (`--field-selector=level=Warning`) could aid troubleshooting.

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

## Tips/tricks to effectively utilize kubectl logs <br/>
* `Organize Logs with Namespaces`: Use namespaces to separate logs for different environments or applications. <br/>
* `Structured Logging & Custom Formatting for Logs` : Using structured logging formats like JSON can make it easier to search, filter, and analyze log data. Consider configuring the applications to log messages in a structured and standardized format (e.g., JSON). This makes it easier to parse and analyze logs using dedicated log analysis tools, improving overall log readability and searchability. <br/>
* `Use labels and selectors to filter logs`: Kubernetes allows to label pods and use selectors to filter logs from specific pods or containers. This can be helpful when there are a large number of pods and 
  customers/developers want to focus on specific ones. Using label selectors with kubectl logs can help aggregate logs from pods with specific labels, which is useful for monitoring applications across multiple 
  pods. This can also help manage the logging directory structure and prevent logs from taking up too much disk space. It involves archiving or deleting old logs. <br/>
* `Logging Levels`: Including different log levels (e.g., DEBUG, INFO, WARNING, ERROR, CRITICAL) can help categorize log messages based on their severity, which is useful during troubleshooting and debugging. <br/>
* `Log Rotation and Retention Policies`: Consider configuring the applications to log messages in a structured and standardized format (e.g., JSON). This makes it easier to parse and analyze logs using dedicated 
  log analysis tools, improving overall log readability and searchability. <br/>
* `Time-Based Log Retrieval`: Utilizing flags like `--since`, `--since-time`, and `--previous` can help retrieve logs based on time or the previous instance of a container, which is useful for troubleshooting 
  recent issues <br/>
* `Sensitive Information`: It's important to avoid logging sensitive information. Use environment variables or secrets to store such data securely. <br/>
* `Contextual Information`: Including timestamps, hostnames, and request IDs in logs can help in correlating log events and troubleshooting issues. <br/>
* `Monitor Resource Usage` : Track resource usage when retrieving logs, especially in production environments. Fetching extensive logs from multiple containers may impact the performance of the cluster. 
  Customers should be mindful of the resources consumed by log retrieval operations. <br/>
* `Log Streaming (Aggregate and Analyze)`: Streaming logs to a centralized log server or log management system is recommended for efficient searching, analysis, and long-term retention of logs. Leverage tools 
   like FluentBit or ELK to collect and analyze logs from various sources. <BR/>
* `Integrate with Monitoring and Alerting Systems` : Integrate logs with monitoring and alerting systems. This allows to set up alerts based on specific log events or patterns, enabling proactive issue detection 
  and rapid response to critical incidents. <br/>
* `Filter and Search`: Use tools like grep or dedicated log viewers for filtering and searching through large log volumes. <br/>
* `Set Up Logging Drivers`: Configure logging drivers for structured log formats and easier parsing. <br/>
* `Control Plane Logs`: Tailing the logs of Kubernetes control plane components like the API server, controller manager, scheduler, and etcd can provide insights into the cluster's behavior. This maynot be 
  required for serverless services (e.g. AWS EKS Fargate).  <br/>

# Kubectl "exec"
* [The kubectl exec command](https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/) allows to start a shell session inside containers running in Kubernetes cluster (i.e. interact with running conatiners & execute commands in container(s) within a pod). This command allows to inspect the container’s file system, check the state of the environment, and perform advanced debugging tools when logs alone don’t provide enough information. It is essential for interacting with the containers, performing tasks such as debugging, modifying files, or checking the environment of the container.<br/>
* `kubectl exec` lets developers specify the container to connect to without worrying about the Kubernetes node it’s on. This is the biggest strength as there is no need to know the node IP (details). (With SSH node details are needed). This command works by establishing a connection to the container's standard input, output, and error streams, enabling the user to interact with the container's processes.<br/>
* By running the shell commands, one can see the container’s entire file system and check if the environment is as expected. It can also help identify whether a critical file is missing or locked, or find instances of misconfigured environment variables. Connecting to a container is useful to view logs, inspect processes, mount points, environment variables, and package versions, amongst other things.<br/>
* An important point to keep in mind is that - `kubectl exec` will gives full shell access to the container, so modifying it and installing packages that are not part of the container image is possible but is not recommended unless for temporary troubleshooting purposes. If extra modifications or packages are required permanently for the container, the image should be modified, and the new version should be deployed to maintain immutability and reproducibility.<br/>
* Please refer to the [Kubernetes exec command documentation](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec) to learn more about different flags that can be used.<br/>
* The `kubectl exec` command works by initiating a process on the client side, which then [communicates with the Kubernetes API server](https://erkanerol.github.io/post/how-kubectl-exec-works/), the kubelet on the node where the pod is running, and finally the container 
  runtime to execute the command in the container. <br/>
*   Below is the format of exec command. One can use `-i` and `-t` flags with it. These two flags combined (`-it`) allows to execute commands inside the container but from  own local terminal. This means if someone running something like `ls` s/he will see the files in the container and not on his/her system where the terminal is actually running. In below command `sh` can also be used instead of `bash`. <br/>
        * `kubectl exec -it <POD_NAME> -n <NAME_SPACE>  -- bash` <br/>
### Key commands <br/>
* `kubectl -n [namespace] exec -it [pod-name] -- /bin/sh` or (`kubectl exec -it <pod-name> -- /bin/bash`): Executing a command in a container with a namespace. This command specifies the namespace of the pod and 
   opens a shell session in the container for live troubleshooting. `-it`: Provides an interactive terminal. <br/>
* `kubectl exec -it [pod-name] -- bash -c "command1; command2; command3` : Executing a multi-command sequence in a container. This command allows to execute multiple commands in sequence within the 
   container's shell. <br/>
* `kubectl exec` requires the container name, even if the pod only has one container. If the container does not have a shell, then we may need to use a different command to access it, such as `kubectl exec [pod- 
   name] -- cat /proc/1/mounts` <br/>
* `kubectl exec [pod-name] -- ls /` : Running a command in a container. This command lists the contents of the root directory in the specified pod. <br/>
* `kubectl exec [pod-name] -c [container-name] -- ls /` : Executing a command in a container with a specific name. The `-c` or `--container` flag specifies the container within the pod where the command should 
   be executed. <br/>
* `kubectl exec [pod-name] -- sleep  5000` : Running a command in a container and then exiting. This example runs a sleep command in the container and then exits, which can be useful for testing or observing the 
   container's behavior. <br/>
* `kubectl exec -it <pod-name> -c <container-name> -- <command>` : Run a Command in a Specific Container.<br/>
* `kubectl exec <pod-name> -- <command>` : Execute a Single Command in a Container. <br/>
* `kubectl exec -it <pod-name> -- /bin/bash -c "while true; do echo hello; sleep 10; done"` : Run a Command and Keep it Running. `/bin/bash -c "while true; do echo hello; sleep 10; done"`: Command that runs 
   continuously.<br/>
* `kubectl exec <pod-name> -c <container-name> --previous -- <command>` : Execute a Command in a Previous Container Instance.
* `kubectl exec <pod-name> -- env VAR_NAME=value command-to-execute` : Set Environment Variables in the Command. `env VAR_NAME=value`: Set environment variables for the command. <br/>
* `kubectl exec <pod_name> -- cat /var/log/myapp.log` :  Views application logs directly within the container. <br/>
* `kubectl exec -i -t <pod_name> -- sh -c "tail -f /var/log/nginx/access.log` : Advanced debugging. Opens an interactive shell and tails the Nginx access log in real-time. <br/>
* `kubectl exec <pod_name> -- ls -la /usr/local/bin` : Lists files and directories within a specific path for configuration inspection. <br/>
* `kubectl exec <pod_name> -- sed -i "s/old_value/new_value/" /etc/myapp.conf` : Modifies a configuration file directly within the container. To be used cautiously. <br/>
* `kubectl exec <pod_name> -- echo "NEW_ENV_VAR=value" >> /etc/environment` : Adds a new environment variable to the container's environment. <br/>
* `kubectl exec <pod_name> -- df -h` : Examine disk usage on the container's filesystem. <br/>
* `kubectl exec <pod_name> -- netstat -ap | grep ESTABLISHED` : Checks active network connections within the container. <br/>
* `kubectl exec <pod_name> -- ping 8.8.8.8` : Checks network connectivity from within the container. <br/>
* `kubectl exec <pod_name> -- /bin/bash` : Opens a shell session within the container's main process for live troubleshooting. <br/>
# Application Logging - stdout & stdin
In all container-based applications (not just kubernetes) it is best practice to direct application logs to stdout/stderr  standard output streams. Kubernetes natively captures and manages these streams, making log collection and aggregation straightforward. There is no worry about losing these logs, as kubelet, Kubernetes’ node agent, will collect these streams and write them to a local file behind the scenes, so that it can be accessed with Kubernetes.<br/>
Directing application logs to stdout/stderr is considered a best practice in containerized environments like Kubernetes and when using orchestration tools. Below are the reasons why this practice is recommended.<br/>
* `Reduced operational burden`: Managing log files in a custom folder requires additional administrative tasks, such as monitoring disk usage, ensuring proper permissions, and dealing with data corruption or loss due to hardware failures. Using stdout/stderr eliminates these operational burdens, as logs are automatically managed by the platform. In contrast, logs written to a file can be difficult to access and manage, especially in a distributed environment like Kubernetes. <br/>
* `Containerization Benefits`: When running applications in containers, it is recommended to follow the Twelve-Factor App methodology, which suggests treating logs as event streams and writing them to stdout/stderr. This approach aligns with the principles of containerization, where applications are expected to be stateless and produce logs as part of their standard output. <br/>
* `Standardization & Interoperability` : Writing logs to stdout/stderr follows the Unix/Linux philosophy of using standard streams for communication. Containers are designed to work with these standard streams, making it easier to collect and manage logs from different containers consistently. Logging to custom folders or files might introduce variations in log formats and make log collection and aggregation more challenging. By adhering to this convention, customers can ensure consistency across different applications and components running in Kubernetes.<br/>
* `Efficient Resource Utilization`: Writing logs to a file can consume a significant amount of disk space (container's filesystem), especially in environments with limited storage. In contrast, logs written to stdout/stderr are ephemeral and do not require persistent storage or  extra disk space or I/O operations. Leveraging stdout/stderr eliminates the need for separate logging infrastructure, reducing overall costs and optimizing resource utilization.<br/>
* `Portability & Dynamic Environments`: By directing logs to stdout/stderr, developers adhere to the principle of treating containers as ephemeral and stateless. With this approach, application logs become part of the container's standard output and error streams, making them easy to capture and transport. This portability allows to run containers on different environments without worrying about specific log file locations or configurations. It simplifies deployment across various platforms and orchestrators like Kubernetes, Docker Swarm, or Rancher. Logs written to stdout/stderr are captured by the container runtime, even if the container is ephemeral, allowing for later analysis. Writing logs to files in custom folders can lead to log loss when containers are removed/deleted.<br/>
* `Efficient Log Aggregation & Centralization & Integration with Ecosystem Tools` : Redirecting logs to stdout/stderr enables better log aggregation and consolidation. Log aggregation tools, such as Elasticsearch, FluentBit/Fluentd, or Logstash (EFK/ELK stack), can easily collect and parse logs from these standard streams. Centralized log aggregation simplifies the management and analysis of logs, improving operational efficiency. <br/>
* `Easier Debugging` : Directing logs to stdout/stderr streamlines the debugging process. As logs are written to the standard streams, they are automatically displayed in the container logs, making it convenient to investigate issues during development and troubleshooting. Developers and operators can easily access container logs through commands like kubectl logs in Kubernetes, docker logs in Docker, or using log aggregators. It eliminates the need to ssh into containers or navigate file systems to access application logs, saving time and effort. <br/>
* `Streamlined Containerization`: When using container orchestration platforms like Kubernetes, leveraging stdout/stderr for logs aligns with the platform's conventions and best practices. Kubernetes, for example, treats logs written to stdout/stderr as important signals about the container's health. It allows for log rotation, collection, and integration with other monitoring and logging solutions. Many cloud-native tools and platforms are built with stdout/stderr redirection in mind, making it easier to integrate and leverage their features. <br/>
* `Easy Integration with Kubernetes Logging Solutions`: Kubernetes provides built-in mechanisms to collect logs from stdout/stderr of containers running in pods. These logs can be easily accessed and managed using Kubernetes logging solutions like Fluentd, Fluent Bit, or the Kubernetes API. <br/>
* `Improved scalability`: As applications scale up or down, managing logs in a custom folder becomes increasingly complex. Standardizing logs to stdout/stderr enables easy scaling and minimizes the risk of running out of disk space or experiencing performance degradation due to log volume.

In summary - Directing application logs to stdout/stderr is a widely accepted best practice in containerized environments. By adopting the practice of directing application logs to stdout/stderr, customers can ensure better portability, facilitate log aggregation, simplify debugging, and align with industry best practices. It enhances overall log management and analysis and allows for seamless integration with container orchestration platforms and reduces operational complexities. By adhering to this practice, customer's can make their logging process more efficient, robust, and scalable in their Kubernetes clusters or any other containerized environment. <br/>

# Further references

Below are some additional resources and references for further learning:<br/>

1. [Kubernetes logging best practices](https://www.cncf.io/blog/2023/07/03/kubernetes-logging-best-practices/)<br/>
2. [Bash Scripting Examples](https://www.youtube.com/watch?v=q2z-MRoNbgM)<br/>
3. [Metrics, tracing, and logging](https://peter.bourgon.org/blog/2017/02/21/metrics-tracing-and-logging.html)<br/>
4. [What are Container Runtimes & 3 Types of Container Runtimes](https://humalect.com/blog/container-runtimes)<br/>
5. [Kubectl Exec: How to Execute Shell Commands Into a Container (With Examples)](https://kodekloud.com/blog/kubectl-exec/)<br/>
6. [kubectl logs: How to Get Pod Logs in Kubernetes (With Examples)](https://kodekloud.com/blog/kubectl-logs/)<br/>
7. [A Practical Guide to Kubernetes Logging](https://logz.io/blog/a-practical-guide-to-kubernetes-logging/)<br/>
8. [Monitoring, Logging, and Debugging](https://kubernetes.io/docs/tasks/debug/)<br/>
9. [Docker Containers and Kubernetes Fundamentals – Full Hands-On Course](https://www.youtube.com/watch?v=kTp5xUtcalw&t=14s)<br/>
10. [Docker Tutorial for Beginners [FULL COURSE in 3 Hours]](https://www.youtube.com/watch?v=3c-iBn73dDE)<br/>
11. [Kubernetes Logging Simplified – Pt 1: Applications](https://observiq.com/blog/kubernetes-logging-simplified-pt-1-applications)<br/>
