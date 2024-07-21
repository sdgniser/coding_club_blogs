---
layout: post
title:  "Remote Devlopment on SSH Servers"
date:   2024-01-06 
categories: blogs
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

After that, you will be prompted to type in your password, and after that, you have logged in. To close the remote connection, just type `exit`.

Now, I understand that typing passwords every time might be cumbersome. Thus, we can set up passwordless login using SSH keys. Think of SSH keys as a special pair of keys that unlock a secret door to a remote server. 

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

## __init__(setup):

Now once you in the remote sever three things will be your best friends from now python enviroments, git and tmux. I will breifly talk about all of them. 

### Enviroments

In the fast-paced world of machine learning, where results can depend on minute details, maintaining order and clarity is crucial. This is where environments come in, offering a controlled space for your code to flourish.

Think of environments as **dedicated workbenches**, perfectly equipped for specific tasks. Each project gets its own isolated space, ensuring:

* **Dependency Isolation:** No more conflicts between versions or packages from different projects, leading to cleaner code and reliable results.
    
* **Reproducibility:** Re-running code? Environments guarantee consistency, allowing you to replicate experiments with confidence.
    
* **Flexibility:** Seamlessly switch between projects with distinct requirements, optimizing your workflow.

Here's a tutorial to set up your development environment without conda, covering virtual environment creation and bashrc configuration:

1. **Create a Virtual Environment Using venv:**

    * **Open a terminal:** Access your terminal application.
    * N**avigate to your desired directory:** Use the `cd` command to change to the directory where you want to create the environment (e.g., cd my-project).
    * **Create the environment:** Type `python3 -m venv myenv` (replace myenv with your preferred name).

2. **Activate the Environment:**

    * **Source the activation script**: Type `source myenv/bin/activate`. You'll see the environment's name in parentheses before your prompt.

3. **Install Packages:**

    * **Use pip:** Install packages within the active environment using `pip install <package_name>` (e.g., pip install pytorch torchvision torchaudio).

4. **Edit the bashrc File (Optional):**

    * Open the file with vi ~/.bashrc.

    * Press i to enter insert mode.
    * Add source myenv/bin/activate at the end.
    * Press Esc to exit insert mode.
    * Type :wq to save and exit.


Now, whenever you log in, the environment will automatically activate if you've added it to bashrc.

**Additional Tips:**
* Deactivate the environment using deactivate when you're done.
* Create separate environments for different projects to avoid conflicts.
* Use pip list to view installed packages within an environment.
* Consider using virtualenvwrapper for easier management of multiple environments.

### Git

Ever spend hours searching for missing changes or untangling messy code? Git, your reliable companion, tackles these challenges head-on, keeping your machine learning projects organized and collaborative.

**Why Git? Because it's a code time machine:**

- **Rewind disasters:** Accidentally delete a crucial layer? Undo it with `git checkout HEAD-<commit_hash>`.
- **Collaborate:** Track changes made by others, merge them seamlessly with `git merge`, and avoid conflicts.
- **Share your work:** Upload your work to GitHub with `git push origin master`, showcasing your code and fostering collaboration.


**1. Pull Your Project from GitHub:**

- **Clone the repo:** `git clone https://github.com/yourusername/yourproject.git`

**2. Edit and Push Changes:**

- **Stage specific changes:** `git add <filename>`
- **Stage all changes:** `git add .`
- **Commit with a message:** `git commit -m "Added cool new feature"`
- **Push your changes:** `git push origin master`

**Bonus Level: Unlock Git's Arsenal:**

- **Branch out for experiments:** `git checkout -b new_feature`
- **Merge changes back:** `git checkout master && git merge new_feature`
- **Resolve conflicts like a warrior:** Edit conflicting files manually
- **See your history:** `git log` to travel through time

**Additional Tip:** Master these commands first, then explore Git further. Websites like Atlassian Git Tutorial are your allies.

### Tmux

Imagine training a complex machine learning model, only for your internet to flicker and disrupt everything. Frustrating, right? That's where tmux, your command-line control tower, comes in. Think of it as a **multiplexer**, a magical tool that:

* **Keeps your sessions alive:** Even if your connection drops, your training continues in the background, saving you precious time and frustration.
* **Organizes your workflow:** Split your terminal into multiple panes, each running a different training script, tool, or even monitoring dashboard. Multitasking becomes a breeze!
* **Boosts efficiency:** Switch seamlessly between tasks on different panes, eliminating the need for constantly opening and closing new terminal windows.
* **Detached sessions:** Need a break? Detach your tmux session and reconnect later without interrupting your running tasks. You've got ultimate flexibility!

Here's a quick guide to using tmux effectively for training models:

1. **Start your session:** Simply type `tmux` in your terminal to enter a new tmux session.

2. **Split your panes:** Use keyboard shortcuts like `Ctrl+b` followed by `h, j, k, or l` to create horizontal or vertical splits. Assign different training scripts, monitoring tools, or even a web browser to each pane for a complete overview.

3. **Navigate and switch panes:** Use `Ctrl+b` followed by arrow keys to move between panes. Keyboard shortcuts like `Ctrl+b` and `d` or `z` let you detach and reattach sessions easily.

4. **Organize your sessions:** Name your sessions with r`ename-window -t <name>` for easy identification. You can also create multiple sessions for different projects or stages of training.

5. **Utilize advanced features:** Tmux offers powerful features like session locking, tab completion, and custom keybindings. Explore them as you become more comfortable to personalize your workflow.


## Essential Linux Monitoring Commands (including GPUs!)

Running high-powered machine learning models? You wouldn't hop in a race car without checking the engine, right? Similarly, keeping your Linux servers healthy and humming requires close monitoring of their resources, **including the mighty GPUs that drive your models to greatness.** Fear not, brave data heroes, for this section equips you with essential commands to peer under the hood and ensure your system purrs like a well-oiled machine.

**1. CPU Usage:** The heart of your server!

* **`top`:** A real-time top-down view of processes hogging CPU cycles. Identify resource-hungry culprits and prioritize tasks.
* **`vmstat`:** Digs deeper, providing statistics like CPU usage by core, memory usage, and disk activity.

**2. Memory Management:** Don't let RAM bottlenecks slow you down!

* **`free -m`:** A quick snapshot of free and used memory, vital for preventing crashes and optimizing allocation.
* **`htop`:** An interactive interface for monitoring memory usage, processes, and even network activity.

**3. Disk Health:** Where your precious data resides!

* **`df -h`:** Checks disk space usage on mounted partitions, ensuring you don't run out of room for those gigabytes of model weights.
* **`iostat`:** Provides detailed I/O statistics, pinpointing bottlenecks and optimizing disk throughput.

**4. Network Traffic:** Keep the data flowing smoothly!

* **`netstat -a`:** Lists all active network connections, ports, and traffic stats, valuable for debugging network issues.
* **`iftop`:** Monitors real-time network traffic on specific interfaces, identifying bandwidth hogs and potential security concerns.

**5. Unleash the GPU Beast:** Don't forget your dedicated training engine!

* **`nvidia-smi`:** The ultimate tool for monitoring GPU stats. Get details like utilization, temperature, memory usage, and power draw for all your GPUs.
* **`nvidia-smi -q`:** For quick checks, use this query to display temperature, fan speed, and power draw for each GPU.

**Bonus Tools:**

* **`sar`:** A Swiss Army knife for system monitoring, collecting stats on CPU, memory, disk, and network, perfect for generating historical reports and identifying trends.
* **`htop` with GPU support:** Some versions of `htop` display basic GPU utilization alongside other system metrics.

Remember, these are just the tip of the iceberg! Explore resources like `man` pages, online tutorials, and NVIDIA's developer portal to delve deeper into these commands and discover even more monitoring magic.

## Remote Dev Do's and Don'ts

Working on omplex projects on remote servers necessitates both technical expertise and effective resource management. By adopting these key principles, you can enchnace your server experience and efficiency.

**1. Implement Regular Code Backups:** Losing code is never an option. Utilize version control systems like Git to commit your work frequently.


**2. Monitor and Communicate Resource Usage:** Server resources are a shared commodity. Actively monitor resource usage with tools like `top` and `nvidia-smi` for GPUs to understand your footprint. Proactive communication is crucial for resource transparency. Inform your fellow developers of resource-intensive tasks so they can adjust their work accordingly. Remember, harmonious server usage requires coordinated efforts.

**3. Document Everything:** Don't leave your fellow explorers lost in the server labyrinth. Document your code, scripts, and VM configurations meticulously. Leave clear instructions for utilizing your work, paving the path for future adventurers. Think of it as a detailed map, guiding those who follow in your digital footsteps.

Treat your servers with respect. Maintain a clean and organized environment, update software regularly, and prioritize security best practices. Remember, a well-maintained server is a resilient server, and a resilient server unlocks the secrets of innovation, one line of code at a time.


**Happy coding!**

