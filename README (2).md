# Real Estate Listing Page — Azure VM Deployment

A static real estate listing website built with HTML and CSS, deployed on an Ubuntu virtual machine in Microsoft Azure and served using Nginx.

## Overview

This project demonstrates a basic cloud deployment workflow:

1. Store website source code in GitHub.
2. Create and configure an Azure Linux virtual machine.
3. Install Git and Nginx.
4. Clone the repository onto the VM.
5. Copy the website files into Nginx's web root.
6. Access the deployed website using the VM's public IP address.

## Technologies Used

- **HTML** — website structure
- **CSS** — styling and layout
- **Git and GitHub** — source control and code hosting
- **Microsoft Azure Virtual Machine** — cloud hosting
- **Ubuntu Linux** — server operating system
- **Nginx** — web server

## Repository

GitHub repository: https://github.com/ramyakg6/Real-Estate-Listing-Page

## Prerequisites

- An Azure subscription
- An Ubuntu Linux VM in Azure
- SSH access to the VM
- A public IP address assigned to the VM
- An inbound network security rule allowing TCP port `80` (HTTP)
- The repository must be public, or the VM must be configured with GitHub authentication if it is private

## Deployment Steps

### 1. Connect to the Azure VM

From Windows PowerShell, connect using SSH:

```powershell
ssh azureuser@YOUR_VM_PUBLIC_IP
```

Replace `YOUR_VM_PUBLIC_IP` with the public IP address of your VM. Use the actual username configured when creating the VM.

After connecting, the prompt should look similar to:

```text
azureuser@your-vm:~$
```

Run the Linux commands below in the **Azure VM terminal**, not in a local PowerShell prompt.

### 2. Update the package list

```bash
sudo apt update
sudo apt upgrade -y
```

### 3. Install Git and Nginx

```bash
sudo apt install git nginx -y
```

Verify the installations:

```bash
git --version
nginx -v
```

Start Nginx and enable it to start automatically after a reboot:

```bash
sudo systemctl enable --now nginx
sudo systemctl status nginx
```

Press `q` to exit the status screen.

### 4. Clone the GitHub repository

```bash
cd ~
git clone https://github.com/ramyakg6/Real-Estate-Listing-Page.git
cd ~/Real-Estate-Listing-Page
ls
```

The project contains files such as `index.html` and `styles.css`.

If the repository is already cloned on the VM, do not clone it again. Use:

```bash
cd ~/Real-Estate-Listing-Page
```

### 5. Deploy the website to Nginx

Copy the project files into Nginx's default web directory:

```bash
sudo cp -r ~/Real-Estate-Listing-Page/* /var/www/html/
```

Verify that the files were copied:

```bash
sudo ls -l /var/www/html/
```

Confirm that `index.html` and `styles.css` are present. The HTML file should reference the stylesheet using the correct relative path and filename.

### 6. Allow HTTP traffic in Azure

In the Azure Portal:

1. Open the virtual machine.
2. Select **Networking**.
3. Review the inbound port rules.
4. Ensure TCP port `80` is allowed for HTTP traffic.

Do not add a duplicate rule if a suitable rule already exists. For a safer setup, restrict inbound traffic where appropriate.

### 7. Open the deployed website

Visit this address in a browser:

```text
http://YOUR_VM_PUBLIC_IP
```

Replace the placeholder with the VM's public IP address. The Real Estate Listing Page should appear instead of the default Nginx welcome page.

## Updating the Website

After pushing changes to GitHub, connect to the VM and run:

```bash
cd ~/Real-Estate-Listing-Page
git pull
sudo cp -r ~/Real-Estate-Listing-Page/* /var/www/html/
```

Refresh the browser. A hard refresh (`Ctrl + F5`) may help if the browser is showing cached CSS or content.

For changes to static HTML and CSS, restarting Nginx is normally unnecessary.

> Note: This simple copy approach copies visible files and directories but does not delete old files from `/var/www/html/`. If you remove or rename files in the repository, remove obsolete deployed files separately and carefully.

## Troubleshooting

| Issue | What to check |
|---|---|
| `sudo` is disabled | The command may be running in Windows PowerShell. Connect to the Azure Linux VM first. |
| Website does not load | Check the public IP, VM state, Nginx status, and Azure inbound TCP port `80` rule. |
| Nginx welcome page still appears | Check that the project's `index.html` was copied into `/var/www/html/`. |
| CSS does not load | Check the stylesheet filename and path in `index.html`; filenames are case-sensitive on Linux. |
| `git clone` fails | Check the repository URL, internet connectivity, and repository access permissions. |
| Changes do not appear | Run `git pull`, copy the updated files again, and hard-refresh the browser. |
| Nginx is not running | Run `sudo systemctl status nginx` and inspect the error details. |

Useful diagnostic commands:

```bash
sudo systemctl status nginx
sudo nginx -t
sudo journalctl -u nginx --no-pager -n 50
```

## Security Notes

- HTTP on port `80` is unencrypted and is suitable only for a basic learning demonstration.
- For a public production site, configure HTTPS using a domain name and a trusted TLS certificate.
- Restrict SSH access to trusted IP addresses where possible.
- Keep the VM and its packages updated.
- Do not put passwords, private keys, or access tokens in the repository.

## What I Learned

- Connecting to a cloud VM using SSH
- Using Linux package-management commands
- Installing and checking Nginx
- Cloning a GitHub repository onto a server
- Deploying static website files to Nginx's web root
- Configuring Azure inbound network access
- Updating a deployed site using Git

## References

- [GitHub repository](https://github.com/ramyakg6/Real-Estate-Listing-Page)
- [Azure Network Security Groups documentation](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview)
- [GitHub Markdown formatting guide](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
