# File permissions
## Task 2.8a
### **1) Why is managing file ownership important?**
This is to ensure that only authorised users or processes can access and modify sensitive files.

### **2) What is the command to view file ownership?**
```bash
ls -l
``` 
This command displays various information related to the file permission.

### **3) What permissions are set when a user creates a file or directory? Who does file or directory belong to?**
When a user creates a file or directory the ownership falls onto the user by  default.

### **4) Why does the owner, by default, not recieve X permissions when they create a file?**
This is because execute permissions are usually used for files that are executable.

### **5) What command is used to change the owner of a file or directory?**
*chown* (change owner)
```bash
chown <new_owner> <file_or_directory>
```

## Task 2.8b
### **1) Does being the owner of a file mean you have full permissions on that file?**
No, being an owner does not mean that the owner will have full permissions but it means that they can modify permissions or change file owner of the file.

### **2) If you give permissions to the User entity, what does this mean?**
It means that only the owner of the file has permissions.

### **3) If you give permissions to the Group entity, what does this mean?**
This means for that directory or file, users who belong in the group have permissions. 

### **4) If you give permissions to the Other entity, what does this mean?**
This means everyone have permission associated to that file.

### **5) You give the following permissions to a file: User permissions are read-only, Group permissions are read and write, Other permissions are read, write and execute. You are logged in as the user which is owner of the file. What permissions will you have on this file?**
I will have read-only permissions on this file, thi means I can only read the content but not able to modify or execute the file. Also, the Group and Other permissions which are read and write, and read, write and execute does not applicable to me as the owner of the file. 

### **6) Here is one line from the ls -l. Work everything you can about permissions on this file or directory.**
```
-rwxr-xr-- 1 tcboony staff  123 Nov 25 18:36 keeprunning.sh
```
* User: rwx means read, write and execute
* Group: r-x means read and execute
* Other: r-- means read only

## Task 2.8c  
### **1) What numeric values are assigned to each permission?**
| Number | Permission Represent |
| ----------- | ----------- |
| 0 | No permission |
| 1 | Execute permission |
| 2 | Write permission |
| 3 | Execute and write permission |
| 4 | Read permission  |
| 5 | Read and execute permission |
| 6 | Read and write permission |
| 7 | Execute and write and read permission|

### **2) What numeric values are assigned to each permission?**
* Execute permission: 1
* Write permission: 2
* Read permission: 4

### **3) What value would assign read, write and execute permissions?**
7

### **4) What value would assign read and execute permissions?**
5

### **Often, a file or directory's mode/permissions are represented by 3 numbers. What do you think 644 would mean?**
6: first number represents the permission of the user: read and write only
4: second number represents the permission of the group: 
4: the third number represents the permission of the 

## Task 2.8d
### **1) What command changes file permissions?**
*chmod* (change mode)

### **2) To change permissions on a file what must the end user be?**
to change permissions on a file you must be:  
1) The owner of the file
2) superuser 

### **3) Examples of some different ways/syntaxes to set permissions on a new file (named testfile.txt) to:**

a. Set User to read, Group to read + write + execute, and Other to read and write only

*i) set User to read:*
```bash
chmod u+r testfile.txt
```
*ii) set Group read + write + execute:*
```bash
chmod g+rwx testfile.txt
```
*iii) set Other to read + wrtie only:*
```bash
chmod chmod o+rw testfile.txt 
```

b. Add execute permissions (to all entities)
```bash
chmod +x testfile.txt
```
c. Take write permissions away from Group
```bash
chmod g-w testfile.txt
```
d. Use numeric values to give read + write access to User, read access to Group, and no access to Other
```bash
chmod 640 testfile.txt
