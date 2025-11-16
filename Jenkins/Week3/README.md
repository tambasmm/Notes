
### **2. Infrastructure Overview**

## **Table of Contents**
- types-of-infrastructure-jenkins-supports
- master-agent-architecture
- distributed-builds
- tool-management-in-jenkins

---

## **Types of Infrastructure Jenkins Supports**
1.  **On-Premise Servers**
    *   Jenkins is installed and runs on physical or virtual machines within your organization’s data center.
    *   Full control over hardware, security, and network.
    *   Best for organizations with strict compliance or internal-only builds.

2.  **Cloud-Based Infrastructure**
    *   Jenkins can run on cloud platforms like **AWS EC2**, **Azure VM**, **Google Cloud**, or inside **Kubernetes clusters**.
    *   Offers scalability, flexibility, and pay-as-you-go cost model.
    *   Ideal for dynamic workloads and distributed teams.

3.  **Hybrid Infrastructure**
    *   Combines on-premise and cloud resources.
    *   Example: Jenkins master on-prem, agents in the cloud for heavy builds.
    *   Useful for gradual migration to cloud or balancing cost and performance.


---

## **Master-Agent Architecture**
1. **Master Node**:
    *   The **central Jenkins server**.
    *   Responsibilities:
    *   Provides the **web UI** for configuration and monitoring.
    *   **Schedules jobs** and decides which agent will run them.
    *   Handles **orchestration** of builds and plugins.
    *   Think of it as the **brain** of Jenkins.

2. **Agent Nodes**:
*   Remote machines (physical or virtual) that **execute the actual build tasks**.
    *   Agents can run on:
    *   Different operating systems (Windows, Linux, macOS).
    *   Different environments (on-prem, cloud).
    *   Purpose:
    *   Offload heavy tasks from the master.
    *   Enable **parallel execution** of multiple jobs.

3. **Communication**:
    *   Agents connect to the master using:
    *   **SSH** (Secure Shell): Common for Linux-based agents.
    *   **JNLP** (Java Network Launch Protocol): Useful when agents cannot initiate SSH or are behind firewalls.
    *   The master sends job instructions; agents return build results.

4. **Benefits**:
    *   **Scalability**: Add more agents to handle larger workloads.
    *   **Parallel Execution**: Run multiple jobs simultaneously across different nodes.
    *   **Flexibility**: Use different OS and hardware for specialized builds.
    *   **Resource Optimization**: Master focuses on orchestration, agents handle execution.

---

## **Distributed Builds**
1. **Why Distributed?** Reduce build time
2. **Configuration**: Add agents → Configure nodes → Assign labels
3. **Use Cases**: Different OS environments, resource-heavy builds

## **Tool Management in Jenkins**
1. **Global Tool Configuration**: JDK, Maven, Gradle, Git
2. **Automatic Installation**
3. **Version Control**: Per job or globally
