# 🚀 Ansible Master-Slave Architecture on AWS
![alt text](<Screenshot 2026-04-17 192901.png>)
This project demonstrates **Infrastructure + Configuration Automation** using:

- **Terraform** → Provision AWS EC2 instances  
- **Ansible** → Configure servers (Nginx + Apache)

---

## 🏗️ Architecture

- 🧑‍💻 **You (Local Machine)**
- 🖥️ **Ansible Master (Ubuntu)**
- ⚙️ **3 Managed Nodes (Servers)**:
  - server1 (Ubuntu)
  - server2 (RedHat)
  - server3 (CentOS)

All servers are connected via **SSH**.

---

## 📁 Project Structure

```
project/
├── terraform/
│   ├── providers.tf
│   ├── ec2.tf
│   ├── output.tf
│
├── ansible/
│
└── terra-key.pem
```

---

## ⚙️ Prerequisites

- AWS Account
- Terraform Installed
- Ansible Installed
- AWS CLI Configured

---

## 🔐 AWS Setup

```bash
aws configure
```

---

## 🚀 Terraform Deployment

### Initialize
```bash
terraform init
```

### Plan
```bash
terraform plan
```

### Apply
```bash
terraform apply
```

---

## 📡 Infrastructure Created

- 1 Ansible Master (Ubuntu)
- 3 Slave Servers:
  - server1 (Ubuntu)
  - server2 (RedHat)
  - server3 (CentOS)

---

## 🔗 SSH Access

```bash
chmod 400 terra-key.pem

ssh -i terra-key.pem ubuntu@<master-ip>
```

## 🤖 Ansible Setup

---
### Step 0: generate keys

```bash
ssh-keygen
create a folder keys in home directory
vim terra-key-ansible.pem isme private key paste krni hai aur iske path ko master ko dena hai
```




### Step 1: Install Ansible on Master

```bash
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
ansible --version
```

---

### Step 2: Configure Inventory File/hosts file

`hosts`
 cd /etc/ansible/hosts
 vim hosts
```ini
[junoon_servers]

server1 ansible_host=<IP address of server1>

server2 ansible_host=<IP address of server2>

server3 ansible_host=<IP address of server3>


[junoon_servers:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_ssh_private_key_file=home/ubuntu/keys/terra-key-ansible.pem
server1 ansible_user=ubuntu
server2 ansible_user=centos
server3 ansible_user=ec2-user

ansilbe-inventory --list
```


---

### Step 3: Test Connection or use adhoc command and modules

```bash
ansible junoon_servers -m ping

error aayega then apko keys ko only read ka permission dena hai

chmod 400 /home/ubuntu/keys/terra-key-ansible.pem

ansible junoon_servers -m ping


# check each server ping connection
ansible server1 -m ping

ansible server2 -m ping

ansible server3 -m ping
```



---

## 📜 check disk usage/ram/disk spaces

```bash
ansible junoon_servers -a "df -h"

ansible junoon_servers -a "free -h"

ansible server1 -a "df -h"

ansible server2 -a "df -h"

ansible server3 -a "df -h"

```

---



---

## 📜 update server

```bash
ansible junoon_servers -a "sudo apt-get update && apt-get upgrade -y"
```

---




---

## 📜 install nginx on server1 and apache on server2 and server3

```bash
ansible server1 -a "sudo apt-get install nginx -y"

ansible server2 -a "sudo dnf install httpd -y"

ansible server3 -a "sudo dnf install httpd -y"

ansible server2 -a "sudo systemctl start httpd"



ansible server1 -a "sudo systemctl enable nginx"

ansible server2 -a "sudo systemctl enable httpd"

ansible server3 -a "sudo systemctl enable httpd"

```




---


---

## ✅ Verification

Open browser:

```
http://<server-ip>
```
![alt text](<Screenshot 2026-04-17 191136.png>)
![alt text](<Screenshot 2026-04-17 191059.png>)
![alt text](<Screenshot 2026-04-17 191047.png>)

You should see:
- Nginx or Apache default page

---

## 🧹 Destroy Infrastructure

```bash
terraform destroy
```

---

## 🔒 Security Best Practices

- Do not upload `.pem` files to GitHub
- Use security groups properly
- Allow only port 22 and 80
- Use IAM roles instead of root credentials

---

## 📌 Notes

- Ubuntu → uses `nginx`
- RedHat/CentOS → uses `httpd`
- Ansible automatically handles OS differences using `package` module

---

## 👨‍💻 Author

Ajay Mall 
SMVDU | Cloud & DevOps Enthusiast

---

## 📜 License

Free to use for learning purposes.
