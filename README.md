# rclone remote to gdrive transfer 
A guide on transferring files from a remote PC to Google Drive using rclone—without downloading them locally, for fast and efficient transfers.





# Why This Repository?

This repository addresses a practical problem I encountered while collaborating on a radio astronomy project.

One of my collaborators needed to transfer large radio astronomical data files to me overseas.

Here’s the setup:

* **PC A:** My local machine
* **PC B:** My collaborator’s local machine (overseas)
* **PC C:** A remote cluster/server where all the data files are stored

The goal was to transfer the files from **PC C → Google Drive** (accessible to me)
**without first downloading them to PC B (his local system).**

In short, the challenge was:

> How to transfer large files efficiently from a remote system to Google Drive without local downloads?

The solution is surprisingly simple and efficient — using **rclone**.


##  Solution

* On **PC B (collaborator’s local machine)**, he logs into the **remote cluster (C)**.
* Using **rclone**, he configures a connection to our shared **Google Drive**.
* Once setup is complete, he uploads the files **directly from the remote cluster (C)** to Google Drive —
  no downloads to PC B are required.
* After the upload finishes, I configure **rclone** on **PC A (my local machine)** and connect to the same Google Drive.
* From there, I can **copy, sync, or download** the files directly to my local system.

This method enables seamless data transfer:

> **Remote Cluster → Google Drive → Local PC**,
> without redundant data movement or bandwidth waste.


##  Prerequisites

* A working installation of **rclone** on all systems involved.
* Access to the **remote cluster (via SSH)**.
* A **shared Google Drive** account or folder accessible to both parties.


# rclone setup 

## 1. Install rclone

Before this first verify that **rclone** is installed or not. Run - 

```bash
rclone
```
If it's not there then it will show up something like this: 

```bash
Command 'rclone' not found, but can be installed with:
sudo snap install rclone  # version 1.71.1, or
sudo apt  install rclone  # version 1.60.1+dfsg-3ubuntu0.24.04.3
See 'snap info rclone' for additional versions.
```
Then install via the command:

```bash
sudo apt install rclone
```

If your device is mac as in my case, run:

```bash
curl https://rclone.org/install.sh | sudo bash
```

To verifty



