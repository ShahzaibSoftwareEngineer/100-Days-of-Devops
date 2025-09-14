𝐃𝐚𝐲 𝟖 𝐨𝐟 𝟏𝟎𝟎 𝐃𝐚𝐲𝐬 𝐨𝐟 𝐃𝐞𝐯𝐎𝐏𝐒 – 𝐊𝐨𝐝𝐞𝐊𝐥𝐨𝐮𝐝 𝐂𝐡𝐚𝐥𝐥𝐞𝐧𝐠𝐞: Install Ansible

Today, I set up Ansible v4.10.0 on the Jump Host to start testing configuration management and automation tasks. Ansible was chosen because of its agentless architecture and simple setup, making it ideal for orchestrating tasks across multiple servers.

📖 What is Ansible?

Ansible is an open-source IT automation tool that allows teams to automate:

Configuration management – ensuring servers and environments are consistent.

Application deployment – automating code deployment across multiple servers.

Orchestration of multi-node environments – coordinating tasks across systems.

Task automation – repetitive tasks can be automated without human error.

Key Advantages:

Agentless: Does not require software installation on target nodes.

SSH-based communication: Uses SSH for Linux/Unix systems and WinRM for Windows.

Idempotent: Running the same playbook multiple times produces the same result.

YAML Playbooks: Human-readable configuration files for automation.

Extensible: Supports custom modules and plugins.

🛠️ Step-by-Step Commands Performed
Step 1: Check Python & pip availability
python3 --version
python3 -m pip --version


Verified Python 3.9.18 and pip 24.0 are installed, ready for Ansible installation.

Step 2: Install Ansible v4.10.0
python3 -m pip install --user ansible==4.10.0


Installed Ansible in the user environment since sudo was not available.

Step 3: Add Ansible to PATH
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc


Ensures the Ansible binary is executable from any terminal session for the current user.

Step 4: Verify Ansible Installation
which ansible
ansible --version


Confirmed installation location: ~/.local/bin/ansible
Ansible version: 4.10.0 (ansible-core 2.11.12)

Step 5: Test Ansible
ansible localhost -m ping


Output shows pong, verifying Ansible is working and ready to manage nodes.

✅ Key Takeaways

Ansible is agentless and easy to set up using pip3.

User-level installation works even when sudo is not available.

Ansible can now be used to manage multiple servers from the Jump Host.