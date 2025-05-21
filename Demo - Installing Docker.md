Absolutely — here’s your **updated and corrected Docker installation guide** for **Ubuntu 20.04+**, using the **recommended secure method** for adding the GPG key and Docker repository:

---

# 🐳 Docker Installation Guide for Ubuntu

## ✅ Step 1: Update existing packages

```bash
sudo apt update
```

---

## ✅ Step 2: Install prerequisite packages to allow `apt` to use HTTPS

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg
```

---

## ✅ Step 3: Add the official Docker GPG key (secure method)

```bash
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

---

## ✅ Step 4: Add the Docker repository to APT sources

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

## ✅ Step 5: Update the package database

```bash
sudo apt update
```

---

## ✅ Step 6: Install Docker Engine

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## ✅ Step 7: Start and enable the Docker service

```bash
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
```

---

## ✅ Step 8: Add your user to the `docker` group (to avoid using `sudo` with Docker)

```bash
sudo usermod -aG docker $USER
# Or for a specific user:
sudo usermod -aG docker ubuntu
```

---

## ✅ Step 9: Reboot the system to apply group changes

```bash
sudo reboot
```

---

## ✅ Step 10: Post-reboot verification

```bash
groups                 # Should show your user in the 'docker' group
docker version         # Show Docker and containerd versions
docker info            # Display Docker system info
docker -v              # Docker version (short)
```

---

Let me know if you want this as a shell script (`.sh`) or want to install Docker Compose v1 instead of the plugin-based v2.
