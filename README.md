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
After the installation is complete, to verify run : 

```bash
rclone --version
```
You will see something like this : 
```bash
rclone v1.71.1
- os/version: darwin 26.0.1 (64 bit)
- os/kernel: 25.0.0 (arm64)
- os/type: darwin
- os/arch: arm64 (ARMv8 compatible)
- go/version: go1.25.1
- go/linking: dynamic
- go/tags: cmount
```
If your rclone version older than v1.62 then update it via : 

```bash
curl https://rclone.org/install.sh | sudo bash
```
## 2.Configure rclone
Run : 
```bash
rclone config
```
It will pop up something like this:
```bash
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q> 
```
Press *n* and enter. After this, something show up like this : 

```bash
Enter name for new remote.
name>

```
Enter the name you want to, I will give the name *gdrive_baijayanta*. And enter.
After this It will ask you to which storage option you want to choose.
```bash
Option Storage.
Type of storage to configure.
Choose a number from below, or type in your own value.
 1 / 1Fichier
   \ (fichier)
 2 / Akamai NetStorage
   \ (netstorage)
 3 / Alias for an existing remote
   \ (alias)
 4 / Amazon S3 Compliant Storage Providers including AWS, Alibaba, ArvanCloud, Ceph, ChinaMobile, Cloudflare, DigitalOcean, Dreamhost, Exaba, FlashBlade, GCS, HuaweiOBS, IBMCOS, IDrive, IONOS, LyveCloud, Leviia, Liara, Linode, Magalu, Mega, Minio, Netease, Outscale, OVHcloud, Petabox, RackCorp, Rclone, Scaleway, SeaweedFS, Selectel, StackPath, Storj, Synology, TencentCOS, Wasabi, Qiniu, Zata and others
   \ (s3)
 5 / Backblaze B2
   \ (b2)
 6 / Better checksums for other remotes
   \ (hasher)
 7 / Box
   \ (box)
 8 / Cache a remote
   \ (cache)
 9 / Citrix Sharefile
   \ (sharefile)
10 / Cloudinary
   \ (cloudinary)
11 / Combine several remotes into one
   \ (combine)
12 / Compress a remote
   \ (compress)
13 / DOI datasets
   \ (doi)
14 / Dropbox
   \ (dropbox)
15 / Encrypt/Decrypt a remote
   \ (crypt)
16 / Enterprise File Fabric
   \ (filefabric)
17 / FTP
   \ (ftp)
18 / FileLu Cloud Storage
   \ (filelu)
19 / Files.com
   \ (filescom)
20 / Gofile
   \ (gofile)
21 / Google Cloud Storage (this is not Google Drive)
   \ (google cloud storage)
22 / Google Drive
   \ (drive)
23 / Google Photos
   \ (google photos)
24 / HTTP
   \ (http)
25 / Hadoop distributed file system
   \ (hdfs)
26 / HiDrive
   \ (hidrive)
27 / ImageKit.io
   \ (imagekit)
28 / In memory object storage system.
   \ (memory)
29 / Internet Archive
   \ (internetarchive)
30 / Jottacloud
   \ (jottacloud)
31 / Koofr, Digi Storage and other Koofr-compatible storage providers
   \ (koofr)
32 / Linkbox
   \ (linkbox)
33 / Local Disk
   \ (local)
34 / Mail.ru Cloud
   \ (mailru)
35 / Mega
   \ (mega)
36 / Microsoft Azure Blob Storage
   \ (azureblob)
37 / Microsoft Azure Files
   \ (azurefiles)
38 / Microsoft OneDrive
   \ (onedrive)
39 / OpenDrive
   \ (opendrive)
40 / OpenStack Swift (Rackspace Cloud Files, Blomp Cloud Storage, Memset Memstore, OVH)
   \ (swift)
41 / Oracle Cloud Infrastructure Object Storage
   \ (oracleobjectstorage)
42 / Pcloud
   \ (pcloud)
43 / PikPak
   \ (pikpak)
44 / Pixeldrain Filesystem
   \ (pixeldrain)
45 / Proton Drive
   \ (protondrive)
46 / Put.io
   \ (putio)
47 / QingCloud Object Storage
   \ (qingstor)
48 / Quatrix by Maytech
   \ (quatrix)
49 / SMB / CIFS
   \ (smb)
50 / SSH/SFTP
   \ (sftp)
51 / Sia Decentralized Cloud
   \ (sia)
52 / Storj Decentralized Cloud Storage
   \ (storj)
53 / Sugarsync
   \ (sugarsync)
54 / Transparently chunk/split large files
   \ (chunker)
55 / Uloz.to
   \ (ulozto)
56 / Union merges the contents of several upstream fs
   \ (union)
57 / Uptobox
   \ (uptobox)
58 / WebDAV
   \ (webdav)
59 / Yandex Disk
   \ (yandex)
60 / Zoho
   \ (zoho)
61 / iCloud Drive
   \ (iclouddrive)
62 / premiumize.me
   \ (premiumizeme)
63 / seafile
   \ (seafile)

```
Find the option *Google Drive*, In my case it is **22** and enter the corresponding number according to your rclone version.
```bash
Storage> 22
```
```bash
Option client_id.
Google Application Client Id
Setting your own is recommended.
See https://rclone.org/drive/#making-your-own-client-id for how to create your own.
If you leave this blank, it will use an internal key which is low performance.
Enter a value. Press Enter to leave empty.
client_id>
```
Press **Enter** and after : 
```bash
Option client_secret.
OAuth Client Secret.
Leave blank normally.
Enter a value. Press Enter to leave empty.
client_secret>
```
Press **Enter** and after this it will request permision of google drive.
```bash
Option scope.
Comma separated list of scopes that rclone should use when requesting access from drive.
Choose a number from below, or type in your own value.
Press Enter to leave empty.
 1 / Full access all files, excluding Application Data Folder.
   \ (drive)
 2 / Read-only access to file metadata and file contents.
   \ (drive.readonly)
   / Access to files created by rclone only.
 3 | These are visible in the drive website.
   | File authorization is revoked when the user deauthorizes the app.
   \ (drive.file)
   / Allows read and write access to the Application Data folder.
 4 | This is not visible in the drive website.
   \ (drive.appfolder)
   / Allows read-only access to file metadata but
 5 | does not allow any access to read or download file content.
   \ (drive.metadata.readonly)
scope>
```
I will give *full access (1st option)* , choose your option accordingly.
```bash
scope> 1
```
After this:
```bash
Option service_account_file.
Service Account Credentials JSON file path.
Leave blank normally.
Needed only if you want use SA instead of interactive login.
Leading `~` will be expanded in the file name as will environment variables such as `${RCLONE_CONFIG_DIR}`.
Enter a value. Press Enter to leave empty.
service_account_file>
```
Press **Enter**. It will pop up
```bash
Edit advanced config?
y) Yes
n) No (default)
y/n>
```
Enter *n*.
```bash
Use web browser to automatically authenticate rclone with remote?
 * Say Y if the machine running rclone has a web browser you can use
 * Say N if running rclone on a (remote) machine without web browser access
If not sure try Y. If Y failed, try N.

y) Yes (default)
n) No
y/n> 
```
Enter *n* again and it displays : 
```bash
Option config_token.
For this to work, you will need rclone available on a machine that has
a web browser available.
For more help and alternate methods see: https://rclone.org/remote_setup/
Execute the following on the machine with the web browser (same rclone
version recommended):
	rclone authorize "drive" "some alphanumeric text"
Then paste the result.
Enter a value.
config_token>
```
Go to any local machine and paste this into terminal ( whatever pops into your screen, the local pc must have **rclone** installed.)
```bash
rclone authorize "drive" "some alphanumeric text"
```
It will open up a browser page where you have to log into the google acoount you want to share and give access to *rclone* for further operations. Once you are done it will print success and there will be a code popped up in the terminal you have to copy and paste to **config_token** prompt and enter.

After this
```bash
Configure this as a Shared Drive (Team Drive)?

y) Yes
n) No (default)
y/n>
```
Press *n* and it will display : 
```bash
Configuration complete.
Options:
- type: drive
- scope: drive
- token: {"access_token":"}
- team_drive: 
Keep this "gdrive_baijayanta" remote?
y) Yes this is OK (default)
e) Edit this remote
d) Delete this remote
y/e/d>
```
Press *y*.
```bash
y/e/d> y
```
```bash
Current remotes:

Name                 Type
====                 ====
gdrive_baijayanta    drive

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q>
```
Press *q*.
```bash
e/n/d/r/c/s/q> q
```
And we have succesfully configured **rclone**.

## 3.Upload files to google drive

```bash
rclone copy /path/to/data gdrive_baijayanta:/destination_folder -P
```
**-P** shows the progress of the transfer. Keep in mind **/path/to/data** is in local/remote pc and **destination_folder** is the folder you created in **google drive** beforehand. If they do not exist then It will throgh *ERROR*.

rclone copy gd_shared:/destination_folder /local/path -P

## 4.Download files from google drive

```bash
rclone copy gdrive_baijayanta:/destination_folder /local/path -P
```


## Advantages

* No need to download files locally before uploading.
* Saves bandwidth, time, and storage space.
* Secure and scriptable using rclone.
* Ideal for transferring **large scientific datasets** across continents.

---

## References

* [rclone official documentation](https://rclone.org/)
* [rclone Google Drive guide](https://rclone.org/drive/)

# Simple Example

- I have a file named 1.py which contains the following script.
```python
#1.py
# This is a simple .py file to print "Hello World"

print("Hello World")
```
- The file is in **/Users/baijayantabhattacharyya/Downloads**.
- Also I have created a folder in my google drive named **python_test**.
- Run
```bash
rclone copy ~/Downloads/1.py  gdrive_baijayanta:/python_test -P
```
It will show : 
```bash
Transferred:   	         73 B / 73 B, 100%, 24 B/s, ETA 0s
Transferred:            1 / 1, 100%
Elapsed time:         3.1s
```
And job is done.

For example I delete the file **1.py**  in my local/remote pc amd want to copy from my gdrive folder where I have uploaded before. Run : 

```bash
rclone copy  gdrive_baijayanta:/python_test/1.py ~/Downloads/ -P
```

