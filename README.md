# 🧰 Ubuntu 24.04 Setup Summary

Basic setup of bare Ubuntu 24.04 machine

---

## 1️⃣ Update and Upgrade Base System
Refreshes package lists and installs the latest security and system updates.

I like to create an alias first to make this easier in the future

```bash
nano ~/.bashrc
# paste this
alias update="sudo apt update && sudo apt upgrade -y"
# restart shell or source the profile
source ~/.bashrc
# Then run the updates
update
```

---

## 2️⃣ Install Core Utilities
Installs essential command-line tools.

```bash
sudo apt install -y curl wget ca-certificates gnupg git unzip
```

---

## 3️⃣ Clean Up Package Cache
Frees disk space by removing unused packages and temporary files.

```bash
sudo apt autoremove -y && sudo apt clean
```

---

## 4️⃣ Enable Source Repositories
Uncomments `deb-src` lines in `/etc/apt/sources.list` to allow source code downloads for packages.

```bash
sudo sed -i 's/^#.*deb-src/deb-src/' /etc/apt/sources.list
```

---

## 5️⃣ Install Security and Maintenance Tools
Adds automatic updates, firewall, and intrusion prevention tools.

```bash
sudo apt install -y apt-transport-https fail2ban ufw unattended-upgrades
```

---

## 6️⃣ Configure Unattended Upgrades
Enables automatic installation of security updates.

```bash
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

---

## 7️⃣ Install System Utilities and Developer Tools
Provides compilers, shell enhancements, and developer tools.

```bash
sudo apt install -y build-essential zsh neovim htop tmux tree ripgrep fd-find

## These are optional but can be nice to have!
# Install fast for checking internet speed
sudo snap install fast
# Install neofetch for system information
sudo apt install neofetch
# Install nload for monitoring network traffic
sudo apt install nload
# Install glances for monitoring system resources
sudo apt install glances
```

---

## 8️⃣ Install and Initialize Tailscale
This installs and connects Tailscale for secure networking and remote access **on WSL**.

For instructions on other Linux systems, see [here](https://tailscale.com/kb/1031/install-linux)

```bash
# Download it
curl -fsSL https://tailscale.com/install.sh | sh

# it should install, then you can just run this command and log in via browser
sudo tailscale up
```

💡 Once connected, this machine will appear in your Tailscale admin console.  
You can securely SSH into it using its Tailscale IP or hostname from any other Tailscale-connected device.

---

## 9️⃣ Set Up SSH Keys for Secure Access
Creates a secure SSH directory and generates an ED25519 key for GitHub authentication.

```bash
mkdir -p ~/.ssh && chmod 700 ~/.ssh
ssh-keygen -t ed25519 -C "example@email.co.uk"
cat ~/.ssh/id_ed25519.pub
```

Then add what is shown to your GitHub account or other service requiring authentication.

Next, set up git credentials:
```bash
git config --global user.name "Dominic Owens"
git config --global user.email "example@email.co.uk"
```

---

## 🔟 Clone and Remove a Test Repository
Tests SSH access to GitHub by cloning and removing a repo.

```bash
git clone git@github.com:d0minicO/d0minico.github.io.git
ls
# If it looks good, you can safely delete and then work on repos wherever
rm -rf d0minico.github.io/
```

---

## 1️⃣1️⃣ Configure Firewall (UFW)
Sets firewall defaults to deny all incoming connections and allow outgoing, then enables it.

Not necessary for WSL.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw enable
sudo ufw status verbose
```

## Set up conda/mamba

Next, follow the instructions [here](conda_setup.md) to setup conda/mamba
