# Linux File Permissions and Ownership

---

## File Permissions and Ownership

### Understanding Permissions (Read, Write, Execute)

Linux controls access to files and directories using a permission system. Each file and directory has three types of permissions:

- **Read (r):** Allows viewing the contents of a file or listing a directory.
- **Write (w):** Allows modifying the contents of a file or adding/removing files in a directory.
- **Execute (x):** Allows running a file as a program or entering a directory.

Permissions are set for three categories:

- **User (u):** The file’s owner.
- **Group (g):** Users in the file’s group.
- **Others (o):** All other users.

You can view permissions with:

```bash
ls -l
```

For more details, see the [GNU Coreutils documentation](https://www.gnu.org/software/coreutils/manual/html_node/What-information-is-listed.html).

---

### Changing Permissions with `chmod`

The `chmod` command changes file and directory permissions. There are two main ways to use it:

- **Symbolic mode:**  
  - Add permission: `chmod u+x file.txt` (adds execute for user)
  - Remove permission: `chmod g-w file.txt` (removes write for group)
- **Numeric mode:**  
  - Permissions are represented as numbers:  
    - Read = 4, Write = 2, Execute = 1  
    - Example: `chmod 755 file.txt` sets user to rwx (7), group to r-x (5), others to r-x (5)

Learn more about `chmod` in the [chmod manual](https://man7.org/linux/man-pages/man1/chmod.1.html).

---

### Changing Ownership with `chown` and `chgrp`

Ownership determines who controls a file. There are two types:

- **Owner:** Typically the creator of the file.
- **Group:** A set of users.

Change the owner:

```bash
sudo chown newuser file.txt
```

Change the group:

```bash
sudo chgrp newgroup file.txt
```

Change both:

```bash
sudo chown newuser:newgroup file.txt
```

See the [chown manual](https://man7.org/linux/man-pages/man1/chown.1.html) and [chgrp manual](https://man7.org/linux/man-pages/man1/chgrp.1.html) for more information.

---

Following Table summarizes the effects of Linux permissions on files and directories:

| Permission/Bit | Symbol | Numeric Notation | Effect on Files                                              | Effect on Directories                                                               |
|----------------|--------|------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Read           | r      | 4                | View file contents                                          | List directory contents                                                             |
| Write          | w      | 2                | Modify, delete, or rename file                              | Create, delete, or rename files within the directory                                |
| Execute        | x      | 1                | Execute the file as a program                               | Enter the directory (`cd`), access files and subdirectories                         |
| Sticky Bit     | t      | 1 (in special bits, octal 1000) | **No effect** (ignored on modern Linux)                     | Only file owner, directory owner, or root can delete/rename files inside            |
| setgid         | s (g)  | 2 (in special bits, octal 2000) | If set on executable: run with group’s privileges           | New files/directories inherit group ownership from parent directory                 |
| setuid         | s (u)  | 4 (in special bits, octal 4000) | If set on executable: run with file owner’s privileges      | **No effect** (ignored on directories)                                              |

### Notes on Numeric Notation:

- The basic permissions (read, write, execute) combine to form a three-digit number representing user, group, and others (e.g., 755).

- The special bits (setuid, setgid, sticky) add a leading digit to the permission number:
  - setuid = 4xxx
  - setgid = 2xxx
  - sticky = 1xxx

Example:  
`chmod 2755 directory` sets setgid, with rwxr-xr-x (755) permissions.

---

### The Significance of Root and Regular Users

- **Root user:** Has unrestricted access to all files and commands.
- **Regular users:** Have limited access, usually only to their own files.

Administrative tasks (like changing ownership) require root privileges, often accessed using `sudo`.

Read more about root and user management in [The Linux Documentation Project](https://tldp.org/LDP/intro-linux/html/sect_03_01.html).

---

## Further Reading

- [Linux File Permissions Explained](https://www.redhat.com/en/blog/linux-file-permissions-explained)

- [Introduction to Linux Permissions](https://phoenixnap.com/kb/linux-file-permissions)

- [Wikipedia: Sticky bit](https://en.wikipedia.org/wiki/Sticky_bit)
---

### Summary

This comprehensive guide covers Linux file permissions, ownership, and file editing through a series of practical exercises. Key topics include:

1. File Permissions
    - Understanding read, write, and execute permissions
    - Using `chmod` in both numeric and symbolic modes
    - Working with special permissions (setuid, setgid, sticky bit)

2. File Ownership
    - Managing user and group ownership with `chown` and `chgrp`
    - Understanding root and regular user privileges
    - Working with recursive ownership changes

3. Default Permissions
    - Understanding and configuring `umask`
    - Working with default file and directory permissions
    - Managing permission inheritance

These exercises provide hands-on experience essential for Linux system administration and file management.

## Exercises

### Exercise 1: Change Permissions

1. Create a file named `testfile.txt`.
2. Set the permissions to allow the user to read and write, the group to read, and others to have no permissions.

```bash
touch testfile.txt
chmod 640 testfile.txt

```

### Exercise 2: Change Ownership

1. Create a file named `ownfile.txt`.
2. Change the owner to a user named `newuser` and the group to `newgroup`.

```bash
sudo usersadd newuser
touch ownfile.txt
sudo chown newuser:newgroup ownfile.txt
```

### Exercise 3: Check Permissions and Ownership

1. Check the permissions and ownership of `testfile.txt`.

```bash
ls -l testfile.txt
```

### Exercise 4: Change Permissions Recursively

1. Create a directory named `testdir` and a file inside it named `nestedfile.txt`.

```bash
mkdir testdir
touch testdir/nestedfile.txt
```

2. Change the permissions of `testdir` to allow the user to read, write, and execute, the group to read and execute, and others to have no permissions.

```bash
chmod 750 testdir
```

3. Change the permissions of `nestedfile.txt` to allow the user to read and write, the group to read, and others to have no permissions.

```bash
chmod 640 testdir/nestedfile.txt
```

### Exercise 5: Change Ownership Recursively

1. Change the ownership of `testdir` and all its contents to `newuser:newgroup`.

```bash
sudo chown -R newuser:newgroup testdir
```

### Exercise 6: Create a Script

1. Create a script named `permissions_script.sh` that:
   - Creates a file named `scriptfile.txt`.
   - Sets the permissions to allow the user to read and write, the group to read, and others to have no permissions.
   - Changes the ownership to `newuser:newgroup`.

```bash
echo '#!/bin/bash' > permissions_script.sh
echo 'touch scriptfile.txt' >> permissions_script.sh
echo 'chmod 640 scriptfile.txt' >> permissions_script.sh
echo 'sudo chown newuser:newgroup scriptfile.txt' >> permissions_script.sh
chmod +x permissions_script.sh
```

### Exercise 7: Run the Script

1. Execute the script you created in Exercise 11.

```bash
./permissions_script.sh
```

### Exercise 8: Review and Reflect

1. Review the permissions and ownership of `scriptfile.txt`.

```bash
ls -l scriptfile.txt
```

2. Reflect on what you learned about file permissions, ownership, and editing files in Linux.

```bash
# Reflect on your learning experience and note any challenges or insights
```

### Exercise 9: Create a Directory Structure

1. Create a directory structure with a parent directory named `project`, and two subdirectories named `src` and `docs`.

```bash
mkdir -p project/{src,docs}
```

2. Inside `src`, create a file named `main.c` and set its permissions to allow the user to read, write, and execute, the group to read and execute, and others to have no permissions.

```bash
touch project/src/main.c
chmod 750 project/src/main.c
```

3. Inside `docs`, create a file named `README.md` and set its permissions to allow the user to read and write, the group to read, and others to have no permissions.

```bash
touch project/docs/README.md
chmod 640 project/docs/README.md
```

### Exercise 10: Change Ownership of the Directory Structure

1. Change the ownership of the entire `project` directory and its contents to `newuser:newgroup`.

```bash
sudo groupadd newgroup
sudo chown -R newuser:newgroup project
```

### Exercise 11: Verify Permissions and Ownership

1. Verify the permissions and ownership of the `project` directory and its contents.

```bash
ls -l project
```

2. Check the permissions and ownership of `main.c` and `README.md`.

```bash
ls -l project/src/main.c
ls -l project/docs/README.md
```

### Exercise 12: Create a Backup Script

1. Create a script named `backup_script.sh` that:
   - Creates a backup of the `project` directory.
   - Sets the permissions of the backup to allow the user to read and write, the group to read, and others to have no permissions.
   - Changes the ownership of the backup to `newuser:newgroup`.

```bash
echo '#!/bin/bash' > backup_script.sh
echo 'cp -r project project_backup' >> backup_script.sh
echo 'chmod 640 project_backup' >> backup_script.sh
echo 'sudo chown -R newuser:newgroup project_backup' >> backup_script.sh
chmod +x backup_script.sh
```

### Exercise 13: Run the Backup Script

1. Execute the backup script you created in Exercise 17.

```bash
./backup_script.sh
```

### Exercise 14: Verify Backup Permissions and Ownership

1. Verify the permissions and ownership of the `project_backup` directory.

```bash
ls -l project_backup
```

2. Check the permissions and ownership of `main.c` and `README.md` in the backup.

```bash
ls -l project_backup/src/main.c
ls -l project_backup/docs/README.md
```

### Exercise 15: Clean Up

1. Remove the `project`, `project_backup`, and `testfile.txt` files and directories to clean up your workspace.

```bash
rm -rf project project_backup testfile.txt ownfile.txt scriptfile.txt
```

### Exercise 16: Explore `chmod` Numeric Mode

1. Create a file named `numericfile.txt`.

```bash
touch numericfile.txt
```

2. Set the permissions using numeric mode to allow the user to read, write, and execute, the group to read and execute, and others to have no permissions.

```bash
chmod 750 numericfile.txt
```

### Exercise 17: Explore `chown` with Numeric User and Group IDs

1. Create a file named `uidgidfile.txt`.

```bash
touch uidgidfile.txt
```

2. Change the ownership using numeric user and group IDs (replace `1001` and `1002` with actual IDs).

```bash
sudo chown 1001:1002 uidgidfile.txt
```

### Exercise 18: Explore `chgrp` with Numeric Group ID

1. Create a file named `chgrpfile.txt`.

```bash
touch chgrpfile.txt
```

2. Change the group ownership using a numeric group ID (replace `1002` with an actual group ID).

```bash
sudo chgrp 1002 chgrpfile.txt
```

### Exercise 19: Explore `chmod` with Special Permissions

1. Create a file named `specialfile.txt`.

```bash
touch specialfile.txt
```

2. Set the permissions to allow the user to read, write, and execute, the group to read and execute, and others to have no permissions, while also setting the setuid bit.

```bash
chmod 4750 specialfile.txt
```

### Exercise 20: Explore `chmod` with Sticky Bit

1. Create a directory named `sticky_dir`.

```bash
mkdir sticky_dir
```

2. Set the sticky bit on the directory to allow only the owner of a file within it to delete or rename that file.

```bash
chmod +t sticky_dir
```

### Exercise 21: Explore `chmod` with Setgid on a Directory

1. Create a directory named `setgid_dir`.

```bash
mkdir setgid_dir
```

2. Set the setgid bit on the directory so that files created within it inherit the group ownership of the directory.

```bash
chmod g+s setgid_dir
```

### Exercise 22: Explore `chmod` with Read-Only Permissions

1. Create a file named `readonlyfile.txt`.

```bash
touch readonlyfile.txt
```

2. Set the permissions to allow the user to read, the group to read, and others to have no permissions.

```bash
chmod 440 readonlyfile.txt
```

### Exercise 23: Explore `chmod` with Write-Only Permissions

1. Create a file named `writeonlyfile.txt`.

```bash
touch writeonlyfile.txt
```

2. Set the permissions to allow the user to write, the group to have no permissions, and others to have no permissions.

```bash
chmod 200 writeonlyfile.txt
```

### Exercise 24: Explore `chmod` with Execute-Only Permissions

1. Create a file named `executeonlyfile.txt`.

```bash
touch executeonlyfile.txt
```

2. Set the permissions to allow the user to execute, the group to have no permissions, and others to have no permissions.

```bash
chmod 100 executeonlyfile.txt
```

### Exercise 25: Explore `chmod` with All Permissions for User

1. Create a file named `allpermissionsfile.txt`.

```bash
touch allpermissionsfile.txt
```

2. Set the permissions to allow the user to read, write, and execute, while the group and others have no permissions.

```bash
chmod 700 allpermissionsfile.txt
```

### Exercise 26: Explore `chmod` with All Permissions for Group

1. Create a file named `grouppermissionsfile.txt`.

```bash
touch grouppermissionsfile.txt
```

2. Set the permissions to allow the user to read, while the group has read, write, and execute permissions, and others have no permissions.

```bash
chmod 740 grouppermissionsfile.txt
```

### Exercise 27: Explore `chmod` with All Permissions for Others

1. Create a file named `otherpermissionsfile.txt`.

```bash
touch otherpermissionsfile.txt
```

2. Set the permissions to allow the user to read and write, the group to read, and others to have read, write, and execute permissions.

```bash
chmod 777 otherpermissionsfile.txt
```

### Exercise 28: Explore `chmod` with No Permissions

1. Create a file named `nopermissionsfile.txt`.

```bash
touch nopermissionsfile.txt
```

2. Set the permissions to allow no access for the user, group, and others.

```bash
chmod 000 nopermissionsfile.txt
```

### Exercise 29: Explore `chmod` with Mixed Permissions

1. Create a file named `mixedpermissionsfile.txt`.

```bash
touch mixedpermissionsfile.txt
```

2. Set the permissions to allow the user to read and write, the group to read, and others to have no permissions.

```bash
chmod 640 mixedpermissionsfile.txt
```

### Exercise 30: Explore `chmod` with Default Permissions

1. Create a file named `defaultpermissionsfile.txt`.

```bash
touch defaultpermissionsfile.txt
```

2. Set the permissions to allow the user to read and write, the group to read, and others to have no permissions, which is a common default.

```bash
chmod 644 defaultpermissionsfile.txt
```

### Exercise 31: Explore `chmod` with Custom Permissions

1. Create a file named `custompermissionsfile.txt`.

```bash
touch custompermissionsfile.txt
```

2. Set the permissions to allow the user to read, write, and execute, the group to read and execute, and others to have no permissions.

```bash
chmod 750 custompermissionsfile.txt
```

### Exercise 32: Explore `chmod` with Permissions for All

1. Create a file named `allpermissionsfile2.txt`.

```bash
touch allpermissionsfile2.txt
```

2. Set the permissions to allow the user, group, and others to read, write, and execute.

```bash
chmod 777 allpermissionsfile2.txt
```

### Exercise 33: Explore `chmod` with Permissions for User and Group

1. Create a file named `usergrouppermissionsfile.txt`.

```bash
touch usergrouppermissionsfile.txt
```

2. Set the permissions to allow the user to read, write, and execute, and the group to read and execute, while others have no permissions.

```bash
chmod 750 usergrouppermissionsfile.txt
```

### Exercise 34: Explore `chmod` with Permissions for User and Others

1. Create a file named `userotherspermissionsfile.txt`.

```bash
touch userotherspermissionsfile.txt
```

2. Set the permissions to allow the user to read, write, and execute, while the group has no permissions and others can read and execute.

```bash
chmod 751 userotherspermissionsfile.txt
```

### Exercise 35: Explore `chmod` with Permissions for Group and Others

1. Create a file named `groupotherspermissionsfile.txt`.

```bash
touch groupotherspermissionsfile.txt
```

2. Set the permissions to allow the user to read, while the group can read, write, and execute, and others can read and execute.

```bash
chmod 755 groupotherspermissionsfile.txt
```

### Exercise 36: Explore `chmod` with Permissions for User, Group, and Others

1. Create a file named `fullpermissionsfile.txt`.

```bash
touch fullpermissionsfile.txt
```

2. Set the permissions to allow the user to read, write, and execute, the group to read and execute, and others to read.

```bash
chmod 755 fullpermissionsfile.txt
```

### Exercise 37: Explore `chmod` with Permissions for User and Group Only

1. Create a file named `usergrouponlyfile.txt`.

```bash
touch usergrouponlyfile.txt
```

2. Set the permissions to allow the user to read, write, and execute, and the group to read and execute, while others have no permissions.

```bash
chmod 750 usergrouponlyfile.txt
```

### Exercise 38: Explore `chmod` with Permissions for Group Only

1. Create a file named `grouponlyfile.txt`.

```bash
touch grouponlyfile.txt
```

2. Set the permissions to allow the user to read, while the group can read, write, and execute, and others have no permissions.

```bash
chmod 740 grouponlyfile.txt
```

### Exercise 39: Explore `chmod` with Permissions for Others Only

1. Create a file named `othersonlyfile.txt`.

```bash
touch othersonlyfile.txt
```

2. Set the permissions to allow the user to read and write, while the group has no permissions and others can read and execute.

```bash
chmod 751 othersonlyfile.txt
```

### Exercise 40: Explore `chmod` with Permissions for User, Group, and Others with Special Bits

1. Create a file named `specialbitsfile.txt`.

```bash
touch specialbitsfile.txt
```

2. Set the permissions to allow the user to read, write, and execute, the group to read and execute, and others to read, while also setting the setuid bit.

```bash
chmod 4755 specialbitsfile.txt
```

### Exercise 41: Explore `chmod` with Permissions for User, Group, and Others with Sticky Bit

1. Create a directory named `sticky_special_dir`.

```bash
mkdir sticky_special_dir
```

2. Set the sticky bit on the directory to allow only the owner of a file within it to delete or rename that file, while also allowing the user to read, write, and execute

```bash
chmod 1777 sticky_special_dir
```

### Exercise 42: Explore `chmod` with Permissions for User, Group, and Others with Setgid

1. Create a directory named `setgid_special_dir`.

```bash
mkdir setgid_special_dir
```

2. Set the setgid bit on the directory so that files created within it inherit the group ownership of the directory, while also allowing the user to read, write, and execute, and the group to read and execute.

```bash
chmod 2775 setgid_special_dir
```

### Exercise 43: Explore `chmod` with Permissions for User, Group, and Others with Setuid

1. Create a file named `setuid_special_file.txt`.

```bash
touch setuid_special_file.txt
```

2. Set the permissions to allow the user to read, write, and execute, the group to read and execute, and others to read, while also setting the setuid bit.

```bash
chmod 4755 setuid_special_file.txt
```

### Exercise 44: Explore `chmod` with Permissions for User, Group, and Others with Setgid on a File

1. Create a file named `setgid_special_file.txt`.

```bash
touch setgid_special_file.txt
```

2. Set the permissions to allow the user to read, write, and execute, the group to read and execute, and others to read, while also setting the setgid bit.

```bash
chmod 2755 setgid_special_file.txt
```

### Exercise 45: Explore `chmod` with Permissions for User, Group, and Others with Sticky Bit on a File

1. Create a file named `sticky_special_file.txt`.

```bash
touch sticky_special_file.txt
```

2. Set the permissions to allow the user to read, write, and execute, the group to read and execute, and others to read, while also setting the sticky bit.

```bash
chmod 1775 sticky_special_file.txt
```

### Exercise 46: Explore `chmod` with Permissions for User, Group, and Others with All Special Bits

1. Create a file named `all_special_file.txt`.

```bash
touch all_special_file.txt
```

2. Set the permissions to allow the user to read, write, and execute, the group to read and execute, and others to read, while setting all special bits (setuid, setgid, sticky).

```bash
chmod 4777 all_special_file.txt
```

### Exercise 47: Explore `chmod` with Permissions for User, Group, and Others with No Special Bits

1. Create a file named `no_special_file.txt`.

```bash
touch no_special_file.txt
```

2. Set the permissions to allow the user to read, write, and execute, the group to read and execute, and others to read, without any special bits.

```bash
chmod 755 no_special_file.txt
```

### Exercise 48: Explore `chmod` with Permissions for User, Group, and Others with Custom Special Bits

1. Create a file named `custom_special_file.txt`.

```bash
touch custom_special_file.txt
```

2. Set the permissions to allow the user to read, write, and execute, the group to read and execute, and others to read, while setting a custom combination of special bits (e.g., setuid and sticky).

```bash
chmod 4755 custom_special_file.txt
```

### Exercise 49: Explore `chmod` with Permissions for User, Group, and Others with Custom Numeric Mode

1. Create a file named `custom_numeric_file.txt`.

```bash
touch custom_numeric_file.txt
```

2. Set the permissions using a custom numeric mode that allows the user to read, write, and execute, the group to read and execute, and others to have no permissions.

```bash
chmod 750 custom_numeric_file.txt
```

### Exercise 50: Explore `chmod` with Permissions for User, Group, and Others with Custom Symbolic Mode

1. Create a file named `custom_symbolic_file.txt`.

```bash
touch custom_symbolic_file.txt
```

2. Set the permissions using symbolic mode to allow the user to read, write, and execute, the group to read and execute, and others to have no permissions.

```bash
chmod u=rwx,g=rx,o= custom_symbolic_file.txt
```

### Exercise 51: Explore `chmod` with Permissions for User, Group, and Others with Custom Recursive Permissions

1. Create a directory named `custom_recursive_dir`.

```bash
mkdir custom_recursive_dir
```

2. Inside the directory, create a file named `recursive_file.txt`.

```bash
touch custom_recursive_dir/recursive_file.txt
```

3. Set the permissions recursively to allow the user to read, write, and execute and the group to read and execute, while others have no permissions.

```bash
chmod -R 750 custom_recursive_dir
```

### Exercise 52: Explore `chmod` with Permissions for User, Group, and Others with Custom Recursive Symbolic Mode

1. Create a directory named `custom_recursive_symbolic_dir`.

```bash
mkdir custom_recursive_symbolic_dir
```

2. Inside the directory, create a file named `recursive_symbolic_file.txt`.

```bash
touch custom_recursive_symbolic_dir/recursive_symbolic_file.txt
```

3. Set the permissions recursively using symbolic mode to allow the user to read, write, and execute, the group to read and execute, and others to have no permissions.

```bash
chmod -R u=rwx,g=rx,o= custom_recursive_symbolic_dir
```

### Exercise 53: Explore `chmod` with Permissions for User, Group, and Others with Custom Recursive Numeric Mode

1. Create a directory named `custom_recursive_numeric_dir`.

```bash
mkdir custom_recursive_numeric_dir
```

2. Inside the directory, create a file named `recursive_numeric_file.txt`.

```bash
touch custom_recursive_numeric_dir/recursive_numeric_file.txt
```

3. Set the permissions recursively using numeric mode to allow the user to read, write, and execute, the group to read and execute, and others to have no permissions.

```bash
chmod -R 750 custom_recursive_numeric_dir
```

### Exercise 54: Explore `chmod` with Permissions for User, Group, and Others with Custom Recursive Special Bits

1. Create a directory named `custom_recursive_special_dir`.

```bash
mkdir custom_recursive_special_dir
```

2. Inside the directory, create a file named `recursive_special_file.txt`.

```bash
touch custom_recursive_special_dir/recursive_special_file.txt
```

3. Set the permissions recursively to allow the user to read, write, and execute and the group to read and execute, while also setting the setgid bit.

```bash
chmod -R 2750 custom_recursive_special_dir
```

### Excercise 55: Explorer `umask` and Default Permissions

1. Check the current `umask` value.

```bash
umask
```

2. Create a file named `umask_file.txt` and observe the default permissions applied.

```bash
touch umask_file.txt
ls -l umask_file.txt
```

3. Change the `umask` value to `022` and create another file named `umask_file2.txt`.

```bash
umask 022
touch umask_file2.txt
ls -l umask_file2.txt
```

### Exercise 56: Explore `umask` with Custom Permissions

1. Set a custom `umask` value to `027` and create a file named `custom_umask_file.txt`.

```bash
umask 027
touch custom_umask_file.txt
ls -l custom_umask_file.txt
```

### Exercise 57: Explore `umask` with Recursive Directory Creation

1. Create a directory named `umask_recursive_dir` with the current `umask` value.

```bash
mkdir umask_recursive_dir
ls -ld umask_recursive_dir
```

2. Change the `umask` value to `027` and create a subdirectory named `subdir` inside `umask_recursive_dir`.

```bash
umask 027
mkdir umask_recursive_dir/subdir
ls -ld umask_recursive_dir/subdir
```

### Exercise 58: Explore `umask` with File Creation in a Directory

1. Create a file named `umask_file_in_dir.txt` inside `umask_recursive_dir`.

```bash
touch umask_recursive_dir/umask_file_in_dir.txt
ls -l umask_recursive_dir/umask_file_in_dir.txt
```

2. Change the `umask` value to `022` and create another file named `umask_file_in_dir2.txt`.

```bash
umask 022
touch umask_recursive_dir/umask_file_in_dir2.txt
ls -l umask_recursive_dir/umask_file_in_dir2.txt
```

### Exercise 59: Explore `umask` with Special Permissions

1. Create a file named `umask_special_file.txt` with the current `umask` value.

```bash
touch umask_special_file.txt
chmod 644 umask_special_file.txt
ls -l umask_special_file.txt
```

2. Change the `umask` value to `027` and create another file named `umask_special_file2.txt`.

```bash
umask 027
touch umask_special_file2.txt
chmod 644 umask_special_file2.txt
ls -l umask_special_file2.txt
```

### Exercise 60: Explore `umask` with Special Permissions on Directories

1. Create a directory named `umask_special_dir` with the current `umask` value.

```bash
mkdir umask_special_dir
ls -ld umask_special_dir
```

2. Change the `umask` value to `027` and create a subdirectory named `subdir_special` inside `umask_special_dir`.

```bash
umask 027
mkdir umask_special_dir/subdir_special
ls -ld umask_special_dir/subdir_special
```

### Exercise 61: Explore `umask` with Special Permissions on Files in a Directory

1. Create a file named `umask_special_file_in_dir.txt` inside `umask_special_dir`.

```bash
touch umask_special_dir/umask_special_file_in_dir.txt
ls -l umask_special_dir/umask_special_file_in_dir.txt
```

2. Change the `umask` value to `022` and create another file named `umask_special_file_in_dir2.txt`.

```bash
umask 022
touch umask_special_dir/umask_special_file_in_dir2.txt
ls -l umask_special_dir/umask_special_file_in_dir2.txt
```

### Exercise 62: Explore `umask` with Special Permissions on Files with Setuid

1. Create a file named `umask_setuid_file.txt` with the current `umask` value.

```bash
touch umask_setuid_file.txt
chmod 4755 umask_setuid_file.txt
ls -l umask_setuid_file.txt
```

2. Change the `umask` value to `027` and create another file named `umask_setuid_file2.txt`.

```bash
umask 027
touch umask_setuid_file2.txt
chmod 4755 umask_setuid_file2.txt
ls -l umask_setuid_file2.txt
```

### Final Clean up

1. Remove all files and directories created during the exercises to clean up your workspace.

```bash
rm -rf testfile.txt ownfile.txt scriptfile.txt project project_backup numericfile.txt uidgidfile.txt chgrpfile.txt specialfile.txt sticky_dir setgid_dir setuid_special_file.txt setgid_special_file.txt sticky_special_file.txt all_special_file.txt no_special_file.txt custom_special_file.txt custom_numeric_file.txt custom_symbolic_file.txt custom_recursive_dir custom_recursive_symbolic_dir custom_recursive_numeric_dir custom_recursive_special_dir umask_file.txt umask_file2.txt custom_umask_file.txt umask_recursive_dir umask_recursive_dir/subdir umask_recursive_dir/umask_file_in_dir.txt umask_recursive_dir/umask_file_in_dir2.txt umask_special_dir umask_special_dir/subdir_special umask_special_dir/umask_special_file_in_dir.txt umask_special_dir/umask_special_file_in_dir2.txt umask_setuid_file.txt umask_setuid_file2.txt
```

```bash
# delete the user and group created for the exercises
sudo userdel newuser
sudo groupdel newgroup
```