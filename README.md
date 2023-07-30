# [work in progress] Kubernetes Applications - Logging, Debugging & Troubleshooting.
## Introduction
Welcome to the one-stop resource — the definitive guide to Kubernetes application logging and debugging. Whether you are a seasoned Kubernetes expert or just starting your journey into containerization, this comprehensive blog will equip you with the knowledge and tools to master the art of logging and debugging in Kubernetes. <br/><br/>
In the world of modern application development, Kubernetes has emerged as the de facto container orchestration platform, offering unparalleled scalability and flexibility. As developers and architects delve deeper into the Kubernetes ecosystem, the need for robust application logging and efficient debugging becomes increasingly evident.<br/>
This open source blog is intended to be a one stop shop for Kubernetes application debugging and logging purposes. This can also serve as guideline to be followed in Kubernetes implementations as it contains industry standard approaches and best practices.<br/> 
This blog will help to effectively debug and analyze kubernetes applications to make things easy for architects/developers to orchestrate containers by Kubernetes. Given that Kubernetes is complex, knowing where to look for that data and how to interpret it can be tricky.<br/><br/>
The popularity of Kubernetes and its ecosystem grows like a snowball rolling down Mount Everest. Kubernetes is a very complex ecosystem. It is highly distribuuted and dynamic. The nodes/pods/containers can be terminated, restarted, or rescheduled at any point in time. This transient and dynamic nature of the system is a challenge in itself. <br/>
* This article provides an overview of logging for Kubernetes. It covers which types of log data are available in Kubernetes and how to access that data.
* Kubernetes clusters consists of multiple layers that need monitoring, each producing different types of logs. This blog will help to troubleshoot and debug Kubernetes applications by gaining visibility into Kubernetes. There are also various logging tools that integrate natively with Kubernetes to make the task easier.  <br/>
* To be effective in container orchestration using Kubernetes - a good knowledge of Linux is required because it all starts from there. Without Linux its difficult to go far. The good news is that there are lot of free courses to scale up in Linux. Below are some video courses & articles to master it.
    * [Linux crash Course](https://www.youtube.com/playlist?list=PLT98CRl2KxKHKd_tH3ssq0HPrThx2hESW) <br/>
    * [Bash scripting on Linux](https://www.youtube.com/playlist?list=PLT98CRl2KxKGj-VKtApD8-zCqSaN2mD4w) <br/>
    * [Bash Scripting for Beginners](https://www.youtube.com/playlist?list=PLKMOdY6Bhga5fmUcQQwhfL9thR_Yp1hZ7) <br/>  
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
* In Kubernetes, when pods are evicted, crashed, deleted, or scheduled on a different node, the logs from the containers are gone. The system cleans up after itself. Therefore DevOps engineers lose any information about why the issue/anomaly occurred. This transient nature of default logging in Kubernetes makes it crucial to implement a centralized log management solution. <br/>
# Kubernetes Events
* Kubernetes Events, are in a way, logs in themselces. An event is a Kubernetes resource/object. that occurs when a change happens with another Kubernetes resource/object whether it’s Pods, Services, Nodes, etc. It includes information about errors and changes to resource state. For example, events may include scheduler decisions and reasons for pod deletion.<br/>
* Events are API objects stored on the API server. By default, Kubernetes drops event data 60 minutes after events are fired, so customers need to have a mechanism for storing event data in a persistent location.<br/>
# Kubectl "exec"
* [The kubectl exec command](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec) allows to start a shell session inside containers running in Kubernetes cluster. This command allows to inspect the container’s file system, check the state of the environment, and perform advanced debugging tools when logs alone don’t provide enough information.<br/>
* "kubectl exec" lets developers specify the container to connect to without worrying about the Kubernetes node it’s on. This is the biggest strength as there is no need to know the node IP (details). (With SSH node details are needed)<br/>
* By running the shell commands, one can see the container’s entire file system and check if the environment is as expected. It can also help identify whether a critical file is missing or locked, or find instances of misconfigured environment variables.<br/>
*   Below is the format of exec command. One can use "-i" and "-t" flags with it. These two flags combined (-it) allows to execute commands inside the container but from  own local terminal. This means if someone running something like "ls" s/he will see the files in the container and not on his/her system where the terminal is actually running. In below command "sh" can also be used instead of "bash". <br/>
        * kubectl exec -it <POD_NAME> -n <NAME_SPACE>  -- bash <br/>

# Further references

Below are some additional resources and references for further learning:<br/>

1. [Kubernetes logging best practices](https://www.cncf.io/blog/2023/07/03/kubernetes-logging-best-practices/#:~:text=This%20data%20includes%20information%20about,monitor%20and%20maintain%20application%20health.)<br/>
