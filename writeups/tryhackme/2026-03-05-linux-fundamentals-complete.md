# 🐧 TryHackMe Writeup: Linux Fundamentals (Part 1–3)

📅 Date: 2026-03-05  
🌐 Platform: TryHackMe  
📚 Rooms:  
- https://tryhackme.com/room/linuxfundamentalspart1  
- https://tryhackme.com/room/linuxfundamentalspart2  
- https://tryhackme.com/room/linuxfundamentalspart3  

<img width="668" height="224" alt="image" src="https://github.com/user-attachments/assets/6731c903-05f2-4d00-a5d7-657758f31bf2" />

---

# 🎯 Objective

The **Linux Fundamentals** series introduces the basic concepts required to work with the Linux operating system.

The rooms focus on:

- basic Linux commands
- navigating the filesystem
- searching for files
- shell operators
- connecting to machines via SSH
- understanding flags and switches
- managing users and permissions
- interacting with the Linux system

These skills are essential for anyone working in **cybersecurity or penetration testing**, as Linux is widely used in security environments.

---

# 🧠 Part 1 — Linux Basics

## Introduction to Linux

Linux is an open-source operating system created by **Linus Torvalds**.

Research question in the room:

**What year was the first release of Linux?**

Answer:

```
1991
```

Linux is widely used across many systems, including:

- servers
- embedded devices
- Android devices
- cybersecurity tools

---

# 💻 Running Your First Commands

Two of the first commands introduced are:

### echo

The `echo` command outputs text to the terminal.

Example:

```
echo TryHackMe
```

Output:

```
TryHackMe
```

---

### whoami

The `whoami` command displays the current logged-in user.

Example:

```
whoami
```

Example output:

```
tryhackme
```

---

# 📂 Interacting With the Filesystem

Linux provides several commands for navigating directories and interacting with files.

### List Files

```
ls
```

Displays the contents of the current directory.

---

### Change Directory

```
cd directory_name
```

Used to navigate between directories.

---

### Print Working Directory

```
pwd
```

Displays the full path of the current directory.

---

### View File Contents

```
cat filename
```

Outputs the contents of a file to the terminal.

---

# 🔎 Searching for Files

The `grep` command can be used to search for text patterns inside files.

Example task in the room:

```
grep THM access.log
```

Example result:

```
THM{ACCESS}
```

---

# ⚙️ Shell Operators

Linux uses operators to control command execution.

### Run Command in Background

```
&
```

Example:

```
command &
```

This runs the command in the background.

---

### Overwrite File Contents

```
>
```

Example:

```
echo password123 > passwords
```

This replaces the contents of the file.

---

### Append to File

```
>>
```

Example:

```
echo tryhackme >> passwords
```

This adds text to the end of the file without overwriting existing data.

---

# 🔐 Part 2 — Accessing Linux Machines

The second room introduces connecting to Linux machines remotely.

### SSH Connection

Example command:

```
ssh username@IP_ADDRESS
```

Example:

```
ssh tryhackme@10.10.x.x
```

SSH allows secure remote access to a Linux system.

---

# 📚 Flags and Switches

Many Linux commands use **flags** to modify behaviour.

Example:

```
ls -la
```

Flags allow commands to provide more detailed output or different functionality.

To learn more about commands, Linux provides manual pages.

```
man command_name
```

Example:

```
man ls
```

This displays the documentation for the command.

---

# 👤 Part 3 — Users and Permissions

Linux systems support multiple users with different permissions.

Key concepts include:

- users
- groups
- file ownership
- permissions

---

# 🔑 File Permissions

Linux permissions determine who can:

- read
- write
- execute files

Permissions are commonly modified using:

```
chmod
```

Example:

```
chmod 755 script.sh
```

This command changes the permissions of a file.

---

# 👥 Changing Ownership

File ownership can be modified with:

```
chown user:group file
```

Example:

```
chown user file.txt
```

This assigns ownership of the file to a specific user.

---

# 📌 Key Takeaways

Important lessons from the Linux Fundamentals rooms:

- Linux is a core operating system used in cybersecurity.
- Understanding the command line is essential for security professionals.
- Filesystem navigation is a fundamental Linux skill.
- Commands such as `ls`, `cd`, `pwd`, and `cat` are used frequently.
- Tools like `grep` allow searching within files.
- Shell operators control command execution and file output.
- SSH allows secure remote access to Linux systems.
- File permissions and user management are critical for system security.

---

# 🚀 Conclusion

The **Linux Fundamentals** series provides a foundation for working with Linux systems.

These skills are essential for:

- cybersecurity
- penetration testing
- system administration
- working with security tools such as Kali Linux
