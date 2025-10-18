# rclone remote to gdrive transfer 
A guide on transferring files from a remote PC to Google Drive using rclone—without downloading them locally, for fast and efficient transfers.

# Why this Repo ??
It had been quite since I had been thinking about this. Basically One of my collaborators have some radio astronomical data files which are redudant in size that he wants to transfer to me, overseas. 
To set up the problem Consider I have my local pc named A, He (my collaborator who is overseas) have a local pc named B and he has access to a remote cluster/pc C in which all the data files are . Now he wants to send those files to me without downloading to his local pc and so. 
The problem reduces to how to transfer those files efficiently.
The solution is very straightforward..

## SOLUTION

- In his local pc B , he logs into the remote cluster C.
- Using the rclone programme, he first configures the setup and links the common google drive to rclone so that he can upload the files directly to google drive from remote cluster C.
- After the setup is done, He starts uploading the files to google drive direcly from the remote cluster C without downloading anything to his local pc B.
- Once the upload is complete, I also configure the rclone programme in my local pc A and link it to the same google drive account.
- After this I am done. I can directly copy files from the google drive to my local pc A.


