Here’s a sample `README.md` file for your GitHub repository based on the Jenkins Master-Slave setup you described:

```
# Jenkins Master-Slave Architecture Setup

This repository provides step-by-step instructions for configuring a Jenkins Master-Slave (Master-Agent) architecture. This setup allows distributed builds and resource optimization by using multiple nodes for job execution.

---

## **Why Master-Slave Architecture?**
- Enables job distribution across multiple environments.
- Prevents resource bottlenecks on the Jenkins master node.
- Supports different operating systems and isolated environments for jobs.
- Enhances security by isolating team-specific data.

---

## **Prerequisites**
1. **Two Linux Servers**: 
   - **Master Node**: Runs Jenkins master software.
   - **Agent Node**: Executes tasks assigned by the master.
2. **Java Installed**: Jenkins requires Java (OpenJDK 11) on both nodes.

---

## **Setup Instructions**



### Step 1: Install Jenkins on Master Node
1. Install Java:
   ```bash
   sudo apt install openjdk-11-jdk -y

   ```
2. Add Jenkins repository and install:
   ```bash
   wget -p -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
   sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   sudo curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
     /usr/share/keyrings/jenkins-keyring.asc > /dev/null
   echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
     https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
     /etc/apt/sources.list.d/jenkins.list > /dev/null
   sudo apt-get update
   sudo apt-get install jenkins -y
   ```

### Step 2: Install Java on Agent Node
1. Install Java on the agent node:
   ```bash
   sudo apt install openjdk-11-jdk -y
   ```

---

## **Configuring Master-Agent Architecture**

1. **Navigate to Jenkins UI**: Access it via `<http://<master_ip>:8080>`.
2. **Add a New Node**:
   - Go to `Manage Jenkins` > `Manage Nodes and Clouds` > `New Node`.
   - Enter a name for the agent node and configure:
     - **Number of Executors**: `2`
     - **Remote Root Directory**: `/opt/build`
     - **Labels**: Use labels to identify this node.
     - **Launch Method**: Select `Launch agent by connecting the controller`.
     - **Availability**: `Keep the agent online as much as possible`.
3. **Run the Agent Command**:
   - Execute the provided command from the master node's terminal.

---

## **Running Jobs on the Agent Node**

1. **Create a New Job**:
   - Go to `New Item` > Enter Job Name > Select `Freestyle Project`.
2. **Restrict Job to Node**:
   - Enable `Restrict where this project can be run` and set the node label.
3. **Add Build Steps**:
   - Select `Execute shell` and add commands like:
     ```bash
     uptime
     echo $WORKSPACE
     ```
4. **Build the Job**:
   - Click `Build Now` and verify output in the build logs.

---

## **Verification**
- Check uptime on the agent node:
  ```bash
  uptime
  ```
- Confirm workspace directory matches `/opt/build/workspace/<job_name>`.

---

## **Conclusion**
This Jenkins Master-Slave setup enables distributed job execution and resource optimization. Use this architecture to efficiently manage builds across different environments.

