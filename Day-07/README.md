
# 𝐃𝐚𝐲 𝟕 𝐨𝐟 𝟏𝟎𝟎 𝐃𝐚𝐲𝐬 𝐨𝐟 𝐃𝐞𝐯𝐎𝐏𝐒 – 𝐊𝐨𝐝𝐞𝐊𝐥𝐨𝐮𝐝 𝐂𝐡𝐚𝐥𝐥𝐞𝐧𝐠𝐞 | 𝐋𝐢𝐧𝐮𝐱 𝐒𝐒𝐇 𝐀𝐮𝐭𝐡𝐞𝐧𝐭𝐢𝐜𝐚𝐭𝐢𝐨𝐧

**🔹 Overview**
SSH key-based authentication uses a public/private key pair to authenticate clients to servers without passwords. The private key stays on the client (jump host) and the public key is placed in the server’s `~/.ssh/authorized_keys`. This is essential for secure, non-interactive automation.

---

**🚀 Steps I Completed:**

### # 1. Create `.ssh` directory on the jump host and set correct permissions

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

### # 2. Generate a 4096-bit RSA key pair (no passphrase for automation)

```bash
ssh-keygen -t rsa -b 4096
```

> When prompted for file, press **Enter** to accept `/home/thor/.ssh/id_rsa`.
> Leave passphrase empty (press Enter twice) to allow password-less automation.

### # 3. Copy the public key to all target servers for passwordless login

```bash
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03
```

### # 4. Verify SSH key-based authentication

```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

---

**✅ Outcome:**
↳ No password prompts for SSH access.
↳ Secure, efficient foundation for remote automation tasks.
↳ Improved security posture by removing the need for plaintext password storage.

---

