# rclone remote to gdrive transfer 
A guide on transferring files from a remote PC to Google Drive using rclone—without downloading them locally, for fast and efficient transfers.




### Why This Repo?

I’ve been thinking about this problem for quite some time.
One of my collaborators has large radio astronomical data files that need to be transferred to me overseas.

To set up the scenario:

* I have a **local PC (A)**.
* My collaborator, located overseas, has **another local PC (B)**.
* He also has access to a **remote cluster/PC (C)** where all the data files are stored.

He wants to send those files to me **without first downloading them to his local PC (B)**.

So, the problem reduces to:

> How can we transfer these large files efficiently and directly?

The solution turns out to be quite straightforward...


## SOLUTION

- In his local pc B , he logs into the remote cluster C.
- Using the rclone programme, he first configures the setup and links the common google drive to rclone so that he can upload the files directly to google drive from remote cluster C.
- After the setup is done, He starts uploading the files to google drive direcly from the remote cluster C without downloading anything to his local pc B.
- Once the upload is complete, I also configure the rclone programme in my local pc A and link it to the same google drive account.
- After this I am done. I can directly copy files from the google drive to my local pc A.


