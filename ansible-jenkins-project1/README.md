```markdown
# Ansible NGINX Installation via Jenkins Pipeline

This project automates the installation and configuration of **NGINX** on a remote EC2 instance using **Ansible**, triggered via a **Jenkins Pipeline** job.

## 🚀 Project Goal

- Automate provisioning of a web server (NGINX) using Ansible
- Use Jenkins Pipeline to pull this project from GitHub and run the playbook
- Practice Infrastructure as Code and CI/CD for configuration management

---

## 🧱 Folder Structure



ansible-jenkins-nginx/

├── hosts.ini                 # Ansible inventory file

├── nginx-install.yml    # Ansible playbook to install and start NGINX

└── Jenkinsfile          # Jenkins pipeline definition

````

---

## 📜 Jenkinsfile (Pipeline as Code)

The `Jenkinsfile` defines a simple one-stage pipeline that runs the Ansible playbook:

```groovy
pipeline {
    agent any

    stages {
        stage('Run Ansible Playbook') {
            steps {
                sh 'ansible-playbook -i hosts.ini nginx-install.yml'
            }
        }
    }
}
````

---

## 🧰 Requirements

### ✅ On Jenkins Server

* Jenkins installed and running
* Ansible installed:

  ```bash
  sudo apt update
  sudo apt install ansible -y
  ```
* SSH private key (`.pem`) added to Jenkins user:

  ```bash
  sudo mkdir -p /var/lib/jenkins/.ssh
  sudo cp ~/Downloads/my-key.pem /var/lib/jenkins/.ssh/
  sudo chmod 400 /var/lib/jenkins/.ssh/my-key.pem
  sudo chown jenkins:jenkins /var/lib/jenkins/.ssh/my-key.pem
  ```

---

## 🔑 Sample Ansible `hosts` File

```
[webserver]
your-ec2-ip ansible_user=ubuntu ansible_ssh_private_key_file=/var/lib/jenkins/.ssh/my-key.pem
```

---

## 📋 Sample Playbook (`nginx-install.yml`)

```yaml
---
- name: Install and start Nginx on remote server
  hosts: webserver
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start Nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

---

## 🛠️ Jenkins Setup

### 1. Create a New Pipeline Job

* Open Jenkins → **New Item**
* Name it: `ansible-nginx-pipeline`
* Select **Pipeline**, click **OK**

### 2. Configure Pipeline to Pull from GitHub

* Pipeline → Definition: **Pipeline script from SCM**
* SCM: Git
* Repo URL: `https://github.com/YOUR_USERNAME/ansible-jenkins-nginx.git`
* Branch: `main`
* Script Path: `Jenkinsfile`

---

## ▶️ Running the Project

1. Push this project to GitHub
2. Jenkins will pull the code and run the playbook
3. On Jenkins dashboard, click **Build Now**
4. View logs in **Console Output**

---

## ✅ Verify the Setup

SSH into your EC2 instance:

```bash
ssh -i ~/.ssh/my-key.pem ubuntu@<your-ec2-ip>
```

Check if NGINX is running:

```bash
sudo systemctl status nginx
```

You should see: `active (running)`

---

## 🌐 Author

Kelly Ankiambom

---

## 📌 License

This project is for learning/dev purposes. No license applied.

```
