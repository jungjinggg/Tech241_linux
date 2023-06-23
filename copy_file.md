# Copy a file/directory from a local machine to VM

## 1) Using git/github
1) create a git repo
2) push the folder into Github repo
3) access ssh public key on the local machine
4) create new ssh key by pasting the generated the public key on Github
5) update git on VM terminal
```bash
sudo apt install git
```
6) acquire the repo url
7) on VM terminal
```bash
git clone <repo_url>
```
8) to check if the file/folder has been copied, on VM terminal
```bash
ls
```

## 2) Using scp
This only works on your local machine and it more secures.

You will need:
1) the private key 
2) path to the folder
3) VM IP address

```bash
scp -r -i ~/.ssh/<private-key-path> <destination-folder-path> <username@IPaddress:/home/username>
```