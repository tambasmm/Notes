#### **Week 2: Installation**

*   Platform compatibility and prerequisites
*   Installing Jenkins on various platforms (Windows, Linux)

---

# ✅ **1. Prerequisites for Installing Jenkins**

Regardless of OS, Jenkins needs:

### **✔ Java**

* Jenkins requires **Java 11 or Java 17** (LTS).
* Recommended: **Eclipse Temurin (Adoptium)** or **OpenJDK**.

To check Java version:

```
java -version
```

### **✔ Hardware Requirements**

* RAM: Minimum **512 MB** (Recommended 2 GB+)
* Disk: Minimum **1 GB free**
* CPU: Any dual-core processor+

---

# 🖥️ **2. Installing Jenkins on Windows** [YouTube](https://youtu.be/7WV8gYRCIsY?si=IvnPtSlESIkEM6r4)

### **Step 1 — Install Java**

Download and install **Temurin 17**:
[https://adoptium.net/](https://adoptium.net/)

### **Step 2 — Download Jenkins Installer**

Download the Windows `.msi`:
[https://www.jenkins.io/download/](https://www.jenkins.io/download/)

### **Step 3 — Run the Installer**

* It installs Jenkins as a **Windows Service**.
* Default Jenkins URL:
  👉 **[http://localhost:8080](http://localhost:8080)**

### **Step 4 — Retrieve Initial Admin Password**

Go to:

```
C:\Program Files\Jenkins\secrets\initialAdminPassword
```

Open it → copy the password → paste in browser.

### **Step 5 — Install Plugins**

Choose:

* **Install suggested plugins** (recommended).

Setup your first admin user → Jenkins is ready.

---

# 🐧 **3. Installing Jenkins on Linux (Ubuntu / Debian)** [YouTube](https://youtu.be/bhz8XZTnulw?si=z6eJbgP8dP4Oo8E1)

### **Step 1 — Install Java**

```
sudo apt update
sudo apt install openjdk-17-jdk -y
```

### **Step 2 — Add Jenkins Repository**

```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc >/dev/null
```

Add repository:

```
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

### **Step 3 — Install Jenkins**

```
sudo apt update
sudo apt install jenkins -y
```

### **Step 4 — Start Jenkins**

```
sudo systemctl start jenkins
sudo systemctl status jenkins
```

### **Step 5 — Open Firewall**

```
sudo ufw allow 8080
```

### **Step 6 — Get Admin Password**

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Open in browser:
👉 `http://your-server-ip:8080`

---

There are many installation method mention below which can we used depending on need.

---

# 📋 Jenkins Installation Methods – Comparison Table

| Installation Method                            | Description                                                    | Advantages                                       | Disadvantages                                   | Best For                          |
| ---------------------------------------------- | -------------------------------------------------------------- | ------------------------------------------------ | ----------------------------------------------- | --------------------------------- |
| **Service Installation (Windows/Linux/macOS)** | Installs Jenkins as a system service that starts automatically | Auto-start, stable, recommended for production   | Harder to move or uninstall; needs admin access | Production, long-running servers  |
| **WAR File (Portable Jenkins)**                | Run Jenkins directly using `java -jar jenkins.war`             | No installation, portable, easiest for beginners | Must start manually; stops when terminal closes | Students, learners, testing       |
| **Docker Container**                           | Run Jenkins in an isolated container environment               | Clean setup, isolated, no Java required on host  | Requires Docker; extra learning                 | DevOps, CI/CD users               |
| **Virtual Machine (VM)**                       | Install Jenkins inside VirtualBox/VMware/Hyper-V               | Fully isolated OS, safe environment              | Heavy resource usage                            | Labs, testing environments        |
| **Cloud VM (AWS, Azure, GCP)**                 | Install Jenkins on a cloud server                              | Accessible globally; good for teams              | Might cost money                                | Real projects, production CI/CD   |
| **Linux RPM / ZIP (Manual Installation)**      | Install using ZIP or RPM packages                              | Flexible, manual control                         | More steps required                             | Advanced users                    |
| **Build from Source**                          | Compile Jenkins manually using Maven                           | Full control, modify Jenkins code                | Very complex; not recommended for beginners     | Jenkins developers / contributors |

---

