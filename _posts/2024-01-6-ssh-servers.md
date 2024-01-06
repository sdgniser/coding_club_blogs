---
layout: post
title:  "Remote Devlopment on SSH Servers"
date:   2024-01-06 
categories: jekyll update
author: 'Sagar Prakash Barad'
---

In an era where the landscape of remote development is no longer exclusive to large corporations, SSH servers have emerged as indispensable tools for a broader audience, particularly in academia. As researchers undertake increasingly complex projects, the demand for substantial computational power becomes more pronounced—whether for analyzing vast datasets or conducting resource-intensive simulations. SSH servers, or Secure Shell servers, are used to provide secure and encrypted access to remote servers. Beyond just connecting, SSH helps users handle server resources, work with files, and run commands from a distance. In response to the growing demand for remote access, data security, and efficient resource use, SSH servers have become vital. They offer a safe and centralized space for coding, modeling, and collaborative work.

This blog aims to be a comprehensive guide to remote development on these servers, covering accessing them, setting up environments, running code, monitoring resources, updating projects, and offering a few tips for collaborative and efficient development using SSH servers. Although the talk was supposed to be essentially a remote development tutorial for Machine and Deep Learning, I believe the contents here will be helpful even for non-ML tasks.

## SSH Servers

SSH, which stands for Secure Shell, is a cryptographic network protocol used for secure communication over an unsecured network. The servers which use these protocols are naturally called ssh servers. These servers act as gatekeepers, providing a secure and encrypted channel for communication between your local computer and a remote server. Think of them as digital fortresses that safeguard your data and commands as they travel across networks.

### Key Features of SSH Servers:

* **Secure Encryption:** All data exchanged between your device and the server is encrypted using powerful cryptographic algorithms, preventing unauthorized access or eavesdropping.
    
* **Remote Command Execution:** You can execute commands on the remote server as if you were sitting right in front of it, allowing you to manage files, install software, and run applications remotely.

* **File Transfer:** Securely transfer files between your local machine and the remote server using secure file transfer protocols like SFTP (SSH File Transfer Protocol).

* **Port Forwarding:** Tunnel traffic from other applications through the encrypted SSH connection, enhancing security for web browsing, email, and other services.

### Accessing SSH Servers Across Operating Systems:

Now, let's see how to connect to SSH servers from different types of computers.

* **Windows**

If you're on Windows 10 or 11, there's a tool called OpenSSH that comes with it. All you have to do is open a PowerShell or Command Prompt window and type:

```powershell

ssh username@hostname
```


needless to say replace "username" and "hostname" with your actual username and server address. 

*  **Linux and macOS**

Now, if you're using a Mac or some version of Linux, open a terminal window and type:

```bash

ssh username@hostname
```

After that, you will be prompted to type in your password, and after that, you have logged in. To close the remote connection, just type exit.

Now, I understand that typing passwords every time might be cumbersome. Thus, we can set up passwordless login using SSH keys. Think of SSH keys as a special pair of keys that unlock a secret door to a remote server. They're a more secure way to log in than using traditional passwords.


**Public Key: Your "Spare Key" for the Server**

* This key is like a spare key you give to a friend so they can enter your house.
* It's publicly shared with the remote server, added to a special file called authorized_keys.
* When you try to connect, the server uses this key to verify your identity.
* Anyone with your public key can send you encrypted messages, but only you can decrypt them with your private key.

**Private Key: Your Top-Secret Master Key**

* This key is like your personal house key—keep it safe and never share it!
* It's stored on your local computer and never leaves it.
* It's used to unlock your encrypted communication with the server and prove that you're the rightful owner of the public key.
* If someone gets ahold of your private key, they could potentially access any server that trusts your public key.


### Passwordless Login Set Up:

1. Create Your Secret Key Pair:

    * Open a terminal on your computer and type `ssh-keygen`.
    * Press Enter a few times to accept the default settings (or choose a different location and password if you prefer).
    * This creates two keys: a private one (like your house key) and a public one (like a spare key for a friend).

2. Send Your Public Key to the Remote Server:

    * Use this command to copy your public key to the server: `ssh-copy-id username@remote_server_address`
    * Enter your password when prompted.
    * Now the server knows your "spare key" and can let you in without asking for the password.

3. Test the Magic:

    * Try logging in to the server using `ssh username@remote_server_address`.
    * If everything's set up right, you should be in like a VIP, no password needed!


Until now, you might be missing your fancy code editor and its tools. Case in point: VS Code's Remote - SSH extension is here to save the day (and your sanity). For that, we quickly look at [VS Code's official website](https://code.visualstudio.com/) for [remote development](https://code.visualstudio.com/docs/remote/remote-overview).
