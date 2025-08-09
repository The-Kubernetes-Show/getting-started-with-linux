# Linux User and Group Management: Beginner's Guide

---

[YouTube Video](TBD)

---

## Introduction

User and group management is fundamental for Linux system administration. It controls who can access the system and its resources and how their permissions are managed.

---

## 1. Basics of Users in Linux

- Linux uses **user accounts** to identify who is logged in and what they can do.
- Each user has a **username**, a **User ID (UID)**, a **home directory**, and a **default shell**.
- User info is stored in `/etc/passwd` (basic info) and `/etc/shadow` (encrypted passwords).

---

## 2. Basics of Groups in Linux

- Groups are collections of users.
- Each group has a **Group ID (GID)** and a name.
- Groups allow you to assign permissions to multiple users easily.
- Group info is stored in `/etc/group`.

---

## 3. Key Commands

| Command         | Purpose                               |
|-----------------|-------------------------------------|
| `useradd`       | Create a new user account            |
| `passwd`        | Set or change a user's password     |
| `usermod`       | Modify user account details          |
| `userdel`       | Delete a user account                |
| `groupadd`      | Create a new group                   |
| `groupmod`      | Modify a group                      |
| `groupdel`      | Delete a group                      |
| `usermod -aG`   | Add a user to one or more groups     |
| `chage` | Control user attributes | 

---

## 4. Creating a User

```

sudo useradd -m -s /bin/bash username
sudo passwd username

```

- `-m` creates the home directory.
- `-s` sets the default shell.
- `passwd` sets the password.

---

## 5. Modifying a User

Add a user to a group:

```

sudo usermod -aG groupname username

```

Change user’s shell:

```

sudo usermod -s /bin/zsh username

```

---

## 6. Deleting a User

Remove user and their home directory:

```

sudo userdel -r username

```

---

## 7. Creating and Managing Groups

Create a group:

```

sudo groupadd groupname

```

Delete a group:

```

sudo groupdel groupname

```

Add a user to multiple groups:

```

sudo usermod -aG group1,group2 username

```

---

## Hands-On Exercises

### Exercise 1: Create a New User

1. Create a user called `student1` with a home directory and bash shell.
2. Set a password for `student1`.
3. Verify the user is created by checking `/etc/passwd`.

### Practice commands:

```

sudo useradd -m -s /bin/bash student1
sudo passwd student1
tail -n 5 /etc/passwd

```

---

### Exercise 2: Create a Group and Add Users

1. Create a group called `devgroup`.
2. Add `student1` to `devgroup`.
3. Check groups of `student1`.

### Practice commands:

```

sudo groupadd devgroup
sudo usermod -aG devgroup student1
groups student1

```

---

### Exercise 3: Modify User Settings

1. Change `student1`'s default shell to `zsh`.
2. Verify the change.

### Practice commands:

```

sudo usermod -s /bin/zsh student1
grep student1 /etc/passwd

```

---

### Exercise 4: Delete a User and Group

1. Delete user `student1` and remove their home directory.
2. Delete the group `devgroup`.

### Practice commands:

```

sudo userdel -r student1
sudo groupdel devgroup

```

---

## Additional Tips

- Always use sudo when performing user/group management.
- Check current users and groups with commands like `cat /etc/passwd` and `cat /etc/group`.
- Use `id username` to see a user's UID, GID, and groups.

---

This guide covers essential commands and concepts to begin managing users and groups in Linux securely and effectively.

#### References 
>Here are official Red Hat and Ubuntu documentation links specifically for user and group management in Linux:
>
> **Red Hat:**
>
>[Red Hat Enterprise Linux 8: Managing users and groups](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/managing-users-and-groups_configuring-basic-system-settings)
>[Red Hat Enterprise Linux 7: Managing users and groups](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-managing_users_and_groups)
>
>**Ubuntu:**
>
>[Ubuntu Server Guide, User and Group Management section (official Ubuntu documentation)](https://documentation.ubuntu.com/server/how-to/security/user-management/)
>
>These official pages contain comprehensive and authoritative information on managing users and groups, including commands, file locations, permissions, and best practices.