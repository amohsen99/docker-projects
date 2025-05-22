Here’s the updated summary of commands for both Oracle Linux and Ubuntu, including the pre-installation step to ensure `openssh` and `git` are installed, followed by the steps to create an SSH key and link it to your GitHub account with `a.badr@laplacesoftware.com`.

### Summary Commands (Shared for Oracle Linux and Ubuntu, with Pre-Installation)

**Pre-Installation**  
   Ensure required tools are installed:  
   - *Oracle Linux*:  
     ```bash
     sudo yum install openssh-clients git -y
     ```
   - *Ubuntu*:  
     ```bash
     sudo apt update && sudo apt install openssh-client git -y
     ```

1. **Generate SSH Key**  
   ```bash
   ssh-keygen -t ed25519 -C "a.badr@laplacesoftware.com"
   ```
   (Press `Enter` for defaults; use `-t rsa -b 4096` if `ed25519` isn’t supported.)

2. **Start SSH Agent**  
   ```bash
   eval "$(ssh-agent -s)"
   ```

3. **Add Key to Agent**  
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```

4. **Display Public Key** (to copy for GitHub)  
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

5. **Test GitHub Connection**  
   ```bash
   ssh -T git@github.com
   ```

6. **Configure Git (Optional)**  
   ```bash
   git config --global user.email "a.badr@laplacesoftware.com"
   git config --global user.name "Your Name"
   ```

### Notes
- Run the pre-installation command specific to your system (Oracle Linux uses `yum`, Ubuntu uses `apt`).
- After step 4, add the public key to GitHub (**Settings > SSH and GPG keys > New SSH key**).
- Replace `id_ed25519` with `id_rsa` if using RSA.
- These steps now cover setup and execution for both systems comprehensively.

Let me know if you need further adjustments!