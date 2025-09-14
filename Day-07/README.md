
# 𝐃𝐚𝐲 𝟕 𝐨𝐟 𝟏𝟎𝟎 𝐃𝐚𝐲𝐬 𝐨𝐟 𝐃𝐞𝐯𝐎𝐩𝐬 – 𝐊𝐨𝐝𝐞𝐊𝐥𝐨𝐮𝐝 𝐂𝐡𝐚𝐥𝐥𝐞𝐧𝐠𝐞 | 𝐋𝐢𝐧𝐮𝐱 𝐒𝐒𝐇 𝐀𝐮𝐭𝐡𝐞𝐧𝐭𝐢𝐜𝐚𝐭𝐢𝐨𝐧

### 🔹 Overview

SSH (Secure Shell) provides a secure way to access remote servers. Instead of relying on passwords (which are vulnerable to brute-force attacks), we use **key-based authentication**. This improves both security and automation by removing the need for entering passwords every time.

Key points:

* **Private key** stays on the client machine.
* **Public key** is placed on the target server.
* Once configured, login happens securely without a password.

---

### 🔹 Lab Steps (Commands I Performed)

```bash
# 1. Create .ssh directory on the jump host and set correct permissions
mkdir -p ~/.ssh
chmod 700 ~/.ssh  

# 2. Generate a 4096-bit RSA key pair (no passphrase for automation)
ssh-keygen -t rsa -b 4096  

# 3. Copy the public key to all target servers for passwordless login
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03  

# 4. Verify SSH key-based authentication
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

---

### ✅ Outcome

* Successfully logged in to all servers without password prompts.
* Improved security by removing plaintext password usage.
* Ready for automation tasks like Ansible, CI/CD deployments, and remote scripts.

---

