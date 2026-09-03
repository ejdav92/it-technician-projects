# cloudbased-active-directory-and-identity-lab-gcp

### Step 1: Getting the Cloud Computer (`e2-micro`)

* **What I did:** We went to Google Cloud and asked for a brand-new, tiny computer running a computer language called **Ubuntu**.
* **Why I did it:** Every great clubhouse needs a home! We used an `e2-micro` computer because Google lets us borrow it completely for free so we don't have to spend any allowance money.

### Step 2: Giving the Clubhouse a Permanent Address (`Static IP`)

* **What I did:** We told Google Cloud to lock down a permanent address numbers for our computer (`10.128.0.3`) so it never changes.
* **Why I did it:** Imagine if your house got a completely new street address every single time you woke up from a nap—your friends would never know where to find you! Computers in a network need a permanent address so they don't get lost.

### Step 3: Giving the Computer a Name (`Hostname` & `Hosts File`)

* **What I did:** We opened up a secret file inside our computer and wrote down its name: `samba-ad` and its full title: `samba-ad.corp.local`.
* **Why I did it:** This is like writing your computer's name on its school backpack. It helps the computer look in its own built-in phonebook so it instantly knows who it is whenever it wakes up.

### Step 4: Installing the Magic Software (`Samba`, `Kerberos`, & `Winbind`)

* **What I did:** We downloaded special toolboxes called **Samba**, **Kerberos**, and **Winbind** using a magic terminal command (`sudo apt install`).
* **Why I did it:**
* **Samba** is like the head security guard who runs the membership list.
* **Kerberos** is the secret decoder ring that checks secret passwords safely.
* **Winbind** is the translator who helps Windows computers talk smoothly to our Linux computer.



### Step 5: Clearing Out the Old Mess (`Removing smb.conf`)

* **What I did:** We deleted an old, messy template file that came with the computer (`sudo rm /etc/samba/smb.conf`).
* **Why I did it:** The computer came with boring, messy toy boxes already built in. To build our real, professional clubhouse, we had to throw away the toy box so we had a completely clean room to build our master plan.

### Step 6: Building the Kingdom (`Domain Provisioning`)

* **What I did:** We ran a special command (`samba-tool domain provision`) and named it **CORP.LOCAL**, choosing a super-strong password for the head Administrator.
* **Why I did it:** This command automatically builds all the filing cabinets, user lists, and secret security maps inside the computer. It turns our plain server into a full-blown **Active Directory Domain Controller**—meaning it is now the boss computer that manages accounts, passes out keys, and keeps everything organized!

### Step 7: Turning On the Security System (`Kerberos Config`)

* **What I did:** We copied a special security ticket file (`krb5.conf`) into the computer's secure system folder.
* **Why I did it:** This teaches the computer how to stamp and check official security badges so nobody can sneak into the clubhouse without a valid password.

### Step 8: Waking Up the Security Guard (`samba-ad-dc`)

* **What I did:** We turned on the master background service (`sudo systemctl start samba-ad-dc`) and told it to wake up automatically every time the cloud computer turns on.
* **Why I did it:** This is the most important part! It wakes up the background daemon—the electronic brain that sits there 24/7 listening for people trying to log in, verify passwords, and enter the network.<img width="1408" height="768" alt="samba diagram" src="https://github.com/user-attachments/assets/64a85614-6c58-4814-aafb-cfdb4733750b" />
<img width="1012" height="824" alt="terminal success" src="https://github.com/user-attachments/assets/6f2fca66-f582-4088-b692-b6f24f73d1a1" />
<img width="1552" height="936" alt="console samba-ad-server vm" src="https://github.com/user-attachments/assets/d807cca5-bdec-45c0-b53a-54ce96d56147" />
