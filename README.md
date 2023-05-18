# Kubernetes Applications - Logging, Debugging & Troubleshooting.
The popularity of Kubernetes and its ecosystem grows like a snowball rolling down Mount Everest. However Kubernetes is a very complex ecosystem. It is highly distribuuted and dynamic. The nodes/pods/containers can be terminated, restarted, or rescheduled at any point in time. This transient and dynamic nature of the system is a challenge in itself. This blog will help to effectively debug and analyze kubernetes pods (applications) to make things easy. Given that Kubernetes is complex, knowing where to look for that data and how to interpret it can be tricky.<br/>
* This article provides an overview of logging for Kubernetes. It covers which types of log data are available in Kubernetes and how to access that data.
* Kubernetes clusters consists of multiple layers that need monitoring, each producing different types of logs. This blog will help to troubleshoot and debug Kubernetes applications by gaining visibility into Kubernetes. There are also various logging tools that integrate natively with Kubernetes to make the task easier.  <br/>
* Below are the official Kubernetes links which would aid in troubleshooting.
   * [Kubernetes Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)<br/>
   * [Troubleshooting Applications](https://kubernetes.io/docs/tasks/debug/debug-application/)<br/>
   * [Debug Running Pods](https://kubernetes.io/docs/tasks/debug/debug-application/debug-running-pod/)<br/>
   * [Define a Command and Arguments for a Container](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/)<br/>
   * [Get a Shell to a Running Container](https://kubernetes.io/docs/tasks/debug/debug-application/get-shell-running-container/)<br/>
   * [Some things you didn’t know about kubectl](https://kubernetes.io/blog/2015/10/some-things-you-didnt-know-about-kubectl_28/#attach-to-existing-containers) <br/>
   
* Kubernetes is the de-facto standard for container orchestration. It provides the required abstraction for efficiently managing large-scale containerized      applications with declarative configurations, an easy deployment mechanism, and both scaling and self-healing capabilities. However it also introduces new challenges due to its ephemeral and dynamic nature. One of the main challenges is how to centralize Kubernetes logs. <br/>
* In Kubernetes, when pods are evicted, crashed, deleted, or scheduled on a different node, the logs from the containers are gone. The system cleans up after itself. Therefore DevOps engineers lose any information about why the issue/anomaly occurred. This transient nature of default logging in Kubernetes makes it crucial to implement a centralized log management solution. <br/>
# Kubectl "exec"
* [The kubectl exec command](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec) allows to start a shell session inside containers running in Kubernetes cluster. This command allows to inspect the container’s file system, check the state of the environment, and perform advanced debugging tools when logs alone don’t provide enough information.<br/>
* "kubectl exec" lets developers specify the container to connect to without worrying about the Kubernetes node it’s on. This is the biggest strength as there is no need to know the node IP (details). (With SSH node details are needed)<br/>
* By running the shell commands, one can see the container’s entire file system and check if the environment is as expected. It can also help identify whether a critical file is missing or locked, or find instances of misconfigured environment variables.<br/>
*   Below is the format of exec command. One can use "-i" and "-t" flags with it. These two flags combined (-it) allows to execute commands inside the container but from  own local terminal. This means if someone running something like "ls" s/he will see the files in the container and not on his/her system where the terminal is actually running. In below command "sh" can also be used instead of "bash". <br/>
        * kubectl exec -it <POD_NAME> -n <NAME_SPACE>  -- bash <br/>
