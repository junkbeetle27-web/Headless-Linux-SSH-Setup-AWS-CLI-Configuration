# 💻 Headless Linux SSH & AWS CLI Configuration

A beginner-friendly guide to remotely accessing a Linux machine that has no monitor (headless) via SSH, and setting up AWS CLI for cloud development.

---

## What is a Headless Server?

A **headless server** is a computer running Linux that has no monitor, keyboard, or mouse attached to it. You control it entirely from another computer over the network using **SSH (Secure Shell)**.

---

## 🔧 Prerequisites

- A Linux machine (this guide uses **Ubuntu**)
- A Windows computer on the same network
- Both machines connected to the same WiFi or router

---

## 🔍 Step 1 — Find Your Linux Machine's IP Address

Since there is no monitor, you need to find the Linux machine's IP address from your router.

1. On your Windows computer, open a browser and go to:
   ```
   192.168.1.1
   ```
   or
   ```
   192.168.0.1
   ```
2. Log in using the credentials printed on the back of your router
3. Look for a section called **Connected Devices** or **DHCP Clients**
4. Find your Linux machine and note its IP address (e.g. `192.168.68.60`)


If you manage your own router you can also log into the your device providers application and check the devices that are connected
to you home router.
You will need to know your host name so that you can find the IP for the intended device. 

> **Note:** If the browser shows a "connection not private" warning, click **Advanced → Proceed**. This is normal for home routers.

---

## 🔒 Step 2 — Enable SSH on the Linux Machine

SSH allows your Windows machine to connect to and control your Linux machine remotely.

Temporarily plug in a monitor to the Linux machine and run:

```bash
sudo apt install openssh-server
sudo systemctl enable ssh --now
```

Once SSH is enabled you will never need the monitor again.

---

## 🛡️ Step 3 — Open Port 22 in the Firewall

Port 22 is the port SSH uses. If it is blocked, connections will time out.

```bash
sudo ufw allow ssh
sudo ufw reload
```

Check the firewall status with:

```bash
sudo ufw status
```

---

## 🖥️ Step 4 — SSH Into Your Linux Machine From Windows

1. Open **Command Prompt** on your Windows machine
2. Run the following command:

```bash
ssh username@192.168.68.60
```

Replace `username` with your Linux account name and the IP with the one you found in Step 1.

> **Not sure what your username is?** On the Linux machine run:
> ```bash
> whoami
> ```

3. The first time you connect it will ask:
   ```
   Are you sure you want to continue connecting? (yes/no)
   ```
   Type `yes` and press Enter

4. Enter your Linux password when prompted (you will not see characters as you type — this is normal)

You are now connected!

---

## ⬆️ Step 5 — Update Your Linux Machine

Before installing anything, update your package list and upgrade existing software:

```bash
sudo apt update
sudo apt upgrade -y
```

---

## ☁️ Step 6 — Install AWS CLI

AWS CLI lets you manage your Amazon Web Services resources from the command line.

**Download the installer:**
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

**Unzip it:**
```bash
unzip awscliv2.zip
```

**Install it:**
```bash
sudo ./aws/install
```

**Verify the installation:**
```bash
aws --version
```

---

## ⚙️ Step 7 — Configure AWS CLI

Connect AWS CLI to your AWS account using your credentials:

```bash
aws configure
```

You will be prompted to enter:

| Field | Description |
|---|---|
| AWS Access Key ID | Found in AWS Console → IAM → Users → Security Credentials |
| AWS Secret Access Key | Generated alongside your Access Key |
| Default region | e.g. `us-east-1` |
| Output format | Press Enter to use the default |

**Verify the connection:**
```bash
aws sts get-caller-identity
```

This should return your AWS Account ID and user details confirming everything is working.

---

## 🛠️ Troubleshooting

| Problem | Solution |
|---|---|
| `Connection timed out` | SSH is not running or port 22 is blocked. Enable SSH and open the firewall |
| `Connection refused` | SSH is installed but not running. Run `sudo systemctl start ssh` |
| Ping works but SSH does not | Firewall is likely blocking port 22. Run `sudo ufw allow ssh` |
| Wrong username | Run `whoami` on the Linux machine to confirm your username |

---

## Tech Stack

- **OS:** Ubuntu Linux
- **Remote Access:** OpenSSH
- **Cloud CLI:** AWS CLI v2
- **Client:** Windows Command Prompt
