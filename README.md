# Linux Commands
*Displays files except hidden files in the directory*
```bash
ls
```
*Displays present working directory*
```bash
pwd
```
*Displays all commands which related uname*
```bash
uname --help
```
*Displays all commands used*
```bash
history
```
*Displays the processes that is currently running*
```bash
ps -p $$
```
*Navigate to your home directory*
```bash
cd 
```
*Navigates to / (root) directory*
```bash
cd .
```
*Move a directory level up*
```bash
cd ..
```
Downloads a file from the url 
```bash
curl <url> --output <name>
```
*Checks file type*
```bash
file <filename>
```
*Creates a new directory*
```bash
mkdir <name>
```
*Changes a file name*
```bash
mv <current_filename> <new_filename>
```
*Copies a file*
```bash
cp <current_filename> <new_filename>
```
*Makes a new file*
```bash
touch <name>
```
*nano is an editor where you can edit the file*
```bash
nano <filename>
```
*Shows the detail in the file*
```bash
cat <filename>
```
*Shows the context of first two lines in the file*
```bash
head -2 <filename>
```
*Shows the context of last two lines in the file*
```bash
tail -2 <filename>
```
*Shows numbers of line in the file along with the context*
```bash
nl <filename>
```