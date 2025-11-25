Below is the **simplest and clear step-by-step guide** to set up **Jenkins Master–Slave architecture** across *three laptops (A = Master, B & C = Slaves)* connected to the **same WiFi network**.

---

# ✅ **High-Level Architecture**

* **Laptop A** → Jenkins **Master (Controller)**
* **Laptop B & C** → Jenkins **Slave (Agent)**
* All three laptops connected to the **same WiFi**, so they are in the **same LAN** → reachable by local IP.

---

# ✅ **Requirements**

### On all laptops:

* Java installed (OpenJDK 11 or 17 recommended)
* Connected to **same WiFi**
* Firewall allows Jenkins port (default: **8080**) and agent communication

### On Laptop A (Master):

* Jenkins installed and running
* IP Address example: **192.168.1.10**

### On Laptop B & C (Slaves/Agents):

* Java installed
* No Jenkins installation required (only agent.jar run by java)

---

# ✅ **STEP 1 — Get Local IP Address of Each Laptop**

Run this on all laptops:

```
ipconfig
```

Note the **IPv4 address** (e.g., 192.168.1.10, 192.168.1.11, 192.168.1.12).

---

# ✅ **STEP 2 — Make Jenkins Master Accessible on Laptop A**

### When starting Jenkins (WAR or Windows service):

Open Jenkins in browser:

```
http://localhost:8080
```

But slaves need to access it using:

```
http://192.168.1.10:8080
```

### Allow firewall rule on Laptop A

Run this in **PowerShell (Admin)**:

```powershell
New-NetFirewallRule -DisplayName "JenkinsPort8080" -Direction Inbound -Protocol TCP -LocalPort 8080 -Action Allow
```

---

# ✅ **STEP 3 — Create New Agent Node in Jenkins (Master)**

### Go to:

**Manage Jenkins → Nodes → New Node**

Enter:

* Node name: `Slave-B`
* Type: **Permanent Agent**

### Fill settings:

* **# of executors:** 1 (or more)
* **Remote root directory:**
  Example on Laptop B: `C:\jenkins-agent`
* **Labels:** `windows` (optional)
* **Launch method:**
  ✔ **Launch agent via Java Web Start / SSH**
  Or
  ✔ **Launch agent via inbound TCP connection** (**recommended**)

Choose:

### ✔ **"Launch agent by connecting it to the controller"** (Inbound mode)

Click Save.

Jenkins will show an instruction screen with:

* `agent.jar` download link
* Secret key
* Connection command

---

# ✅ **STEP 4 — Connect Laptop B (Slave)**

### On Laptop B:

1. Create folder:

```
C:\jenkins-agent
```

2. Download `agent.jar` from:

```
http://192.168.1.10:8080/jnlpJars/agent.jar
```

3. Run the connection command shown by master (example):

```bash
java -jar agent.jar -jnlpUrl http://192.168.1.10:8080/computer/Slave-B/jenkins-agent.jnlp -secret 123456abcdef -workDir "C:\jenkins-agent"
```

If connected successfully → Jenkins dashboard will show **green online icon**.

---

# ✅ **STEP 5 — Repeat SAME Process for Laptop C**

Create node:

* Name: `Slave-C`
* Remote directory: `C:\jenkins-agent`
* Launch method: inbound
* Run command on Laptop C:

```bash
java -jar agent.jar -jnlpUrl http://192.168.1.10:8080/computer/Slave-C/jenkins-agent.jnlp -secret <key> -workDir "C:\jenkins-agent"
```

Both slaves will connect to master.

---

# 📌 **How Nodes Communicate Over the Same WiFi**

Since all machines are on the **same LAN**, they can communicate via:

* Master → `192.168.x.x`
* Slaves → connect back to master using the same IP

No router configuration is required.

---

# ✅ **STEP 6 — Create a Pipeline That Runs on Slave**

Example:

```groovy
pipeline {
    agent { label 'windows' }
    stages {
        stage('Test') {
            steps {
                echo "Running on: ${env.NODE_NAME}"
            }
        }
    }
}
```

---

# 🧪 **Testing the Setup**

On Jenkins Dashboard → Nodes
You should see:

| Node    | Status | Executors |
| ------- | ------ | --------- |
| Master  | Online | 2         |
| Slave-B | Online | 1         |
| Slave-C | Online | 1         |

---

# 🎉 Your Jenkins Master–Slave Architecture is Ready!

If you want, I can also help you with:

✅ A diagram of your exact setup
✅ Security hardening for agents
✅ Pipeline examples targeting each laptop
✅ Docker-based agents instead of laptops

Would you like that?

# Step-by-step: make Jenkins **Master (Laptop A)** accessible to other laptops on the same Wi-Fi

I’ll give concrete commands and settings for **Windows**, **Linux (Debian/Ubuntu)** and **macOS**. Use the section that matches your Laptop A OS. I’ll also include network and firewall steps and quick troubleshooting.

---

## Quick checklist (what you’ll end up with)

* Jenkins running on Laptop A and listening on port **8080** on all network interfaces.
* Firewall rule allowing inbound TCP **8080** on Laptop A.
* You can open `http://<LaptopA_IP>:8080` from laptops B and C (example IP `192.168.1.10`).

> Replace `192.168.1.10` with the real IPv4 address you get on Laptop A.

---

# 1) Verify Java is installed

Jenkins needs Java (OpenJDK 11+ recommended).

**Windows (PowerShell):**

```powershell
java -version
```

**Linux / macOS (terminal):**

```bash
java -version
```

If not installed:

**Ubuntu/Debian:**

```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
```

**RHEL/CentOS (dnf):**

```bash
sudo dnf install -y java-17-openjdk
```

**Windows:** download and install OpenJDK or Amazon Corretto/AdoptOpenJDK from their websites.

---

# 2) Install and start Jenkins on Laptop A

### Option A — Run Jenkins quickly using the `jenkins.war` (works on any OS)

1. Download `jenkins.war` from Jenkins site (or place a copy you already have) into `C:\jenkins` or `~/jenkins`.
2. Start Jenkins bound to all interfaces (important — so other machines can reach it):

**Command (run where `jenkins.war` is):**

```bash
java -jar jenkins.war --httpPort=8080 --httpListenAddress=0.0.0.0
```

This makes Jenkins listen on **0.0.0.0:8080** (all network interfaces).

> If you omit `--httpListenAddress`, Jenkins may still listen on all interfaces by default — but explicitly setting `0.0.0.0` avoids binding only to localhost.

To run in background:

* On Linux/macOS: run with `nohup` or systemd (see below).
* On Windows: run from an elevated PowerShell and consider installing the MSI for a service.

---

### Option B — Install Jenkins as a service (recommended for production)

**Ubuntu / Debian (recommended):**

```bash
# add Jenkins repo and key
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install -y jenkins
sudo systemctl enable --now jenkins
```

Default listens on 8080. Check `/etc/default/jenkins` (or `/etc/sysconfig/jenkins`) to set `HTTP_HOST=0.0.0.0` if needed.

**RHEL/CentOS (dnf/yum):** follow Jenkins official repo install; same idea — `sudo systemctl start jenkins`.

**Windows (MSI):**

* Download Jenkins Windows installer (MSI) and run it — it will install as a service and open port 8080. You can later configure the service from Services or Jenkins XML config.

**macOS with Homebrew:**

```bash
brew install jenkins-lts
brew services start jenkins-lts
```

Default listens on 8080. For `jenkins.war` method on macOS, use the `java -jar` command above.

---

# 3) Check Jenkins is running locally

Open on Laptop A:

```
http://localhost:8080
```

If that loads, Jenkins is running. If you used `--httpListenAddress=0.0.0.0`, it should also be reachable remotely.

---

# 4) Find Laptop A’s local IP address

**Windows (PowerShell / CMD):**

```powershell
ipconfig
# Look for IPv4 Address under the active Wi-Fi adapter, e.g. 192.168.1.10
```

**Linux / macOS:**

```bash
ip addr show
# or
ifconfig
```

Note the IPv4 address (example: `192.168.1.10`). This is the IP other laptops will use.

---

# 5) Allow inbound port 8080 on Laptop A (firewall)

### Windows PowerShell (Admin):

```powershell
New-NetFirewallRule -DisplayName "Jenkins 8080" -Direction Inbound -Protocol TCP -LocalPort 8080 -Action Allow
```

### Ubuntu (ufw):

```bash
sudo ufw allow 8080/tcp
sudo ufw reload
```

### RHEL/CentOS (firewalld):

```bash
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload
```

### macOS (if using pf or third-party firewall): ensure app/port allowed or disable blocking for testing.

---

# 6) (Optional but recommended) Assign a stable IP or hostname

If your router assigns dynamic IPs, the Laptop A IP may change after reboot. Options:

* Configure static IP on Laptop A (via OS network settings) inside your LAN range.
* Reserve a DHCP lease for Laptop A’s MAC address on the Wi-Fi router (recommended).
* Or use the laptop hostname (e.g., `laptop-a.local`) — works if mDNS/Bonjour is available.

---

# 7) Test access from laptops B and C

On Laptop B / C open a browser and visit:

```
http://192.168.1.10:8080
```

(replace `192.168.1.10` with Laptop A’s IP)

If it loads, success.

If it fails, see troubleshooting below.

---

# Troubleshooting checklist (if other laptops can't reach it)

1. **Ping Laptop A from B/C**

   ```bash
   ping 192.168.1.10
   ```

   If ping fails, network or firewall is blocking ICMP — still possible that TCP may work, but check network connectivity first.

2. **Check Jenkins is listening on 0.0.0.0:8080**
   **Windows (PowerShell):**

   ```powershell
   netstat -ano | findstr :8080
   ```

   **Linux:**

   ```bash
   sudo ss -tlnp | grep 8080
   ```

   You want to see `0.0.0.0:8080` or `:: :8080` — *not* `127.0.0.1:8080` (that would be localhost-only).

   If it’s `127.0.0.1:8080`, restart Jenkins with `--httpListenAddress=0.0.0.0` or edit the config to set the host.

3. **Check firewall again** (on Laptop A). Temporarily disable firewall for testing (only if safe) to confirm it’s the firewall.

4. **Check router client isolation** — some Wi-Fi routers have “AP isolation” / “client isolation” which prevents devices from talking to each other. Disable that in router settings.

5. **Confirm same subnet** — ensure all laptops are on same 192.168.x.x subnet and same Wi-Fi network.

6. **Check proxy settings** — browsers with a proxy may block LAN addresses.

7. **Check Jenkins logs**

   * Linux: `sudo journalctl -u jenkins -f` or check `/var/log/jenkins/jenkins.log`
   * Windows: check service logs or the folder where `jenkins.war` prints its console.

---

# 8) Security notes (before adding agents)

* Don’t expose Jenkins port 8080 to the public internet unless secured (reverse proxy + HTTPS + auth).
* Use Jenkins credentials and agent secrets for agent connections.
* Consider restricting inbound firewall to your LAN subnet (e.g., `192.168.1.0/24`) rather than allowing all sources.

Example (ufw restricting to subnet):

```bash
sudo ufw allow from 192.168.1.0/24 to any port 8080 proto tcp
```

---

If you tell me which OS is on **Laptop A** (Windows, Ubuntu, macOS), I can give a single, fully-tailored command list and a one-click set of commands you can paste — but the above contains everything to make Jenkins reachable on your LAN. Want that OS-specific pasteable script?

