What is Jenkins?
Jenkins is an open-source tool written in Java that automates software development lifecycle tasks like build, test, deploy, and more. In this article, we will discuss how to configure Jenkins master-slave setup also called master-slave or master-agent architecture.


Jenkins
Why do we need Master-Slave architecture?
When we build the Jenkins job in a single Jenkins master node then Jenkins uses the resource of the base machine and If no executor is available then the jobs are queued in the Jenkins server. Sometimes you might need several different environments to test your builds. A single Jenkins server cannot do this. It is recommended not to run different jobs in the same system that required a different environment. In such scenarios where we need a different machine with a different environment that takes the specific job from the master to build.

On the same Jenkins setup, multiple teams are working on their jobs. All jobs are running on the same base operating system and the base operating system has limited resources. Also, we don’t want to put our data on the same system where other teams can read it.


Jenkins Master-Slave
Jenkins Distributed Architecture
Jenkins uses A Master-Slave architecture to manage distributed builds. The machine where we install Jenkins software will be Jenkins master and that runs on port 8080 by default. On the slave machine, we install a program called Agent. This agent requires JVM. This agent executes the tasks provided by the Jenkins master. We can launch n numbers of agents and we can configure which task will be run on which agent server from Jenkins master by assigning the agent to the task.

Prerequisites:
2 Linux servers
For this tutorial, I have already created 2 linux machines and named them as Jenkins-Master & Jenkins-Slave.


Ubuntu Servers
Now, let’s start this setup by installing Jenkins and configuring further.

Step 1: Install Jenkins on Master Node
Jenkins is built on Java so, to install Jenkins we first need to install Java on the linux server.
Run the below command on the Jenkins master node.
sudo apt install openjdk-11-jdk -y
The above commands will install Java on the Jenkins master node.
Run the below commands on the Jenkins master node.
wget -p -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
The above commands will install Jenkins on the Jenkins master node.
Step 2: Install Java on the Agent Node
On the agent node we don’t actually need to install the Jenkins however, jenkins only works when java is installed so we just need to install java on the agent node.
Run the below command to install java on the agent node
sudo apt install openjdk-11-jdk -y
Now, our setup is ready. We will configure the master-agent architecture now.

Step 3: Configuring the Master-Agent setup
Go the Jenkins UI and click on Set up and agent

Add agent
Add the node name as per your choice

Agent node
Add the below details

Add the description as per your need
Number of executors: This indicates that how many parallel jobes this node will execute. As of now I have set it to 2
Remote root directory: Whenever we create any job in the jenkins it will create workspace in the backend, however we have not installed Jenkins in the agent node so, we need to define any directory for the workspace. As of now I have set it to /opt/build
Labels: Add some label so that we can select node based on the label for executing the job
Usage: Set this parameter to Use the node as much as possible so that we can fully utilise the node
Launch method: Set this parameter to Launch agent by connecting the controller
Click on Disable WorkDir
Availability: Set this parameter to Keep the agent online as much as possible. So, it will make sure to keep the node online as much as possible
Click on save.
After that you will see the below page.

Run the Run from agent command line: (Unix) on the master node
After running the command you can see the below page and you can see agent is connected over there.

Now, our master slave setup is ready. We can execute the job on the agent node
Create a new job and configure as per the below image

Click on the Restrict where the project can be run and under the label expression select the label of the agent node
Under the build steps select execute shell and add the below command
uptime
echo $WORKSPACE
Click on save and apply.
Before running the job you can verify the uptime of your agent node.

uptime
For my agent node it is 33 min.
Click on build now and execute the job

In the above image you can see the uptime is 33 min and workspace is also /opt/build/workspace/Demo-Job
That’s it. Our setup is ready. Now, you can create different jobs and execute on the agent node as per your need.
