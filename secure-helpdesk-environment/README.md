# Building a Secure Helpdesk Cloud Environment: A Beginner's Journal

Welcome! If you are brand new to cloud computing, networking, and Linux, this guide is for you. We are going to walk through how we built a secure two-server environment in Google Cloud Platform (GCP), the mistakes we made along the way, and how we fixed every single problem in plain, everyday language.

---

## 1. What Are We Trying to Build?

Imagine you run an office building (your **cloud project**).

* You have a front desk receptionist who checks everyone in. In our cloud, this is called the **bastion host** (or jump box). It faces the public internet so you can securely log into it.
* You have a back office containing sensitive company records. No outside visitors are allowed in here. In our cloud, this is the **private helpdesk VM** (`private-helpdesk-vm`). It has no public internet address, making it super secure.

Our goal was to log into the front desk (bastion host) and then securely connect to the back office (private VM) to make sure our network security rules were working.

---

## 2. Step-by-Step Journey & The Mistakes We Made

### Mistake #1: The Locked Front Door (IAP Connection Blocked)

When we first tried to log into our bastion host using Google Cloud’s browser terminal, we got an error saying **Connection via Cloud Identity-Aware Proxy Failed (Code: 4003)**.

* **Why it happened:** Google Cloud is like a high-security fortress. By default, it locks every door and blocks all incoming traffic. Google's secure tunnel (called IAP) tried to knock on port 22 (the standard door for secure terminal access), but there was no firewall rule welcoming it.
* **How we fixed it:** We created a firewall rule telling Google Cloud: *"Allow incoming traffic on TCP port 22 from Google's special IAP IP address range (`35.235.240.0/20`)"*. Once we applied that, the front door opened right up.

### Mistake #2: The Mismatched Guest List (Target Tag Mismatch)

With our front door open, we logged into the bastion host and tried to "talk" to our private backend machine (`10.1.2.2`) using a network test called a `ping` and an internal `ssh` command. Everything timed out and failed.

* **Why it happened:** We had previously set up a firewall rule named `allow-internal-jump` to allow traffic between our servers, but it looked for a security nametag called `private-server`. However, our private backend machine was actually named `private-helpdesk-vm` and didn't have that specific tag attached to it. The rule was ignoring our machine because it wasn't wearing the right "name tag."
* **How we fixed it:** We went into the settings of our `private-helpdesk-vm` and added the network tag `private-server`. Suddenly, the firewall rule recognized the machine and let traffic flow.

### Mistake #3: Testing with the Wrong Language (ICMP vs. TCP)

Even after fixing the tags, when we ran a `ping` command, it worked, but trying to `ssh` into the private machine gave us a **"Permission denied (publickey)"** error.

* **Why it happened:**
1. **Ping worked** because we updated our firewall rule to allow **ICMP** (the language computers use to play a game of electronic catch).
2. **SSH failed** at the very end because the private machine said: *"I see you coming from the bastion host, but I don't have your cryptographic security key on file, so I won't let you in."*


* **How we fixed it:** This proved our network highway was fully built and working! The connection reached the destination successfully; it was just a security ID check at the final doorway that needed clearance.

---

## 3. Summary of Our Final Working Setup

| Component | What It Does | Final Configuration |
| --- | --- | --- |
| **Bastion Host** | Publicly accessible jump server | Has an External IP and handles incoming management traffic. |
| **Private Helpdesk VM** | Hidden backend server | Has an Internal IP (`10.1.2.2`), sits safely behind the network, and accepts traffic only from authorized sources. |
| **IAP Firewall Rule** | Unlocks the front door | Allows TCP port 22 traffic from Google's IAP range (`35.235.240.0/20`). |
| **Internal Jump Rule** | Connects the servers | Allows traffic from our local subnet (`10.1.1.0/24`) to target machines tagged with `private-server` for TCP and ICMP. |

---
