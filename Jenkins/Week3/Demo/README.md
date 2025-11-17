
### Steps to Configure Agents
[Youtobe](https://youtu.be/VwliiaOI5po?si=gz0Skttd3CkEpErO)
#### **Step 1: Enable Agent Settings on Jenkins Master**

*   Go to **Manage Jenkins → Configure Global Security**.
*   Ensure **TCP port for inbound agents** is set (e.g., `Random` or a fixed port).
*   Save changes.

***

#### **Step 2: Add a New Node**

*   Navigate to **Manage Jenkins → Nodes and Clouds → New Node**.
*   Enter a name for the agent (e.g., `AMG_HPEDU_GENERIC_JNPL`).
*   Select **Permanent Agent** and click **OK**.

***

#### **Step 3: Configure Node Details**

*   **Remote root directory:** Path on the agent machine where Jenkins files will be stored (e.g., `D:\JenkinsNode`).
*   **Number of executors:** How many parallel jobs this agent can run.
*   **Labels:** Add labels like `AMG_HPEDU_JNPL` for targeting jobs.
*   **Usage:** Choose whether Jenkins uses this node freely or only for labeled jobs.
*   **Launch method:**
    *   **SSH:** Recommended for Linux agents.
    *   **JNLP (Java Web Start):** Common for Windows agents.
*   Save configuration.

***

#### **Step 4: Install Agent on Remote Machine**

*   If using **SSH**, Jenkins will handle installation automatically after credentials are provided.
*   If using **JNLP**, download the agent `.jar` file from Jenkins master:
    *   Go to remote root directory(D:\JenkinsNode) 
    *   open cmd and run:
        ```bash
        curl -sO http://localhost:8080/jnlpJars/agent.jar
        ```
    *  Go to https://github.com/winsw/winsw/releases and download requited executable(WinSW-64x.exe).
    *  copy it in remote root directory and rename it to jenkins_agent.exe
    *  create jenkins_agent.xml file and copy peast content from https://github.com/winsw/winsw/blob/v3/samples/jenkins.xml 
    *  Fill field name id, name, discription and argument with 
        ```bash
        java -jar agent.jar -url http://localhost:8080/ -secret <secret_key> -name <agent_name> -webSocket -workDir <remote root directory>
        ```
        ex.
        ```bash
        -jar agent.jar -url http://localhost:8080/ -secret b09b673b36e5d6abeef5756aa4e28d2fbf7611754075a88005fa7b00d2ae8e23 -name "AMG_HPEDU_GENERIC_JNPL" -webSocket -workDir "D:\JenkinsNode"
        ```

    *  Go to remote root directory open cmd and run command:
       ```bash
        jenkins-agent.exe install
        jenkins-agent.exe start
       ```
***

#### **Step 5: Verify Connection**

*   Check the node status under **Manage Jenkins → Nodes**.
*   It should show **Connected** and ready to accept jobs.

***

### **4. Assign Jobs to Agents**

*   In your job configuration:
    *   Under **General → Restrict where this project can be run**, enter the label (e.g., `AMG_HPEDU_JNPL`).
*   Save and build.

***

### **5. Best Practices**

*   Use labels for flexibility.
*   Monitor agent health under **Manage Nodes**.
*   Secure communication (use SSH keys or encrypted JNLP).

