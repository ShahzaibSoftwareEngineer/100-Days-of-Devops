𝐃𝐚𝐲 𝟖 𝐨𝐟 𝟏𝟎𝟎 𝐃𝐚𝐲𝐬 𝐨𝐟 𝐃𝐞𝐯𝐎𝐏𝐒 – 𝐊𝐨𝐝𝐞𝐊𝐥𝐨𝐮𝐝 𝐂𝐡𝐚𝐥𝐥𝐞𝐧𝐠𝐞: Install Ansible

Today, I set up Ansible v4.10.0 on the Jump Host to start exploring configuration management and automation tasks. Ansible was chosen for its agentless design and minimal setup, making it ideal for managing multiple servers efficiently.

📖 What is Ansible?

Ansible is an open-source automation tool that helps teams automate:

Configuration management – maintain consistent environments.

Application deployment – deploy code across servers automatically.

Orchestration – coordinate tasks across multiple nodes.

Task automation – reduce repetitive work and human errors.

Advantages of Ansible:

Agentless – no software required on target servers.

SSH-based communication – uses SSH for Linux/Unix, WinRM for Windows.

Idempotent – ensures consistent results even after multiple runs.

YAML Playbooks – human-readable automation scripts.

Extensible – supports custom modules and plugins.

🛠️ Step-by-Step Commands Performed
Step 1: Check Python & pip
python3 --version
python3 -m pip --version


Verified Python 3.9.18 and pip 24.0 are installed.

Step 2: Install Ansible v4.10.0 (user installation)
python3 -m pip install --user ansible==4.10.0


Installed Ansible in the user environment as sudo access was not available.

Step 3: Add Ansible to PATH
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc


Makes the Ansible binary accessible in any terminal session for the current user.

Step 4: Verify Installation
which ansible
ansible --version


Installation confirmed: ~/.local/bin/ansible
Version: 4.10.0 (ansible-core 2.11.12)

Step 5: Test Ansible
ansible localhost -m ping


Output pong confirms Ansible is working and ready to manage nodes.

✅ Key Takeaways

Ansible is easy to install using pip3 without root access.

User-level installation works perfectly when sudo is unavailable.

The Jump Host is now ready to orchestrate tasks across multiple servers.