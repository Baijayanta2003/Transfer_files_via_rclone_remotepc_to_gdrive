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


## SOLUTION

- In his local pc B , he logs into the remote cluster C.
- Using the rclone programme, he first configures the setup and links the common google drive to rclone so that he can upload the files directly to google drive from remote cluster C.
- After the setup is done, He starts uploading the files to google drive direcly from the remote cluster C without downloading anything to his local pc B.
- Once the upload is complete, I also configure the rclone programme in my local pc A and link it to the same google drive account.
- After this I am done. I can directly copy files from the google drive to my local pc A.


