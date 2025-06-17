
# 🧪 Jenkins + Ansible Flask Deployment (Single EC2)

This project demonstrates how to use **Jenkins Declarative Pipeline** and **Ansible** to deploy a **Flask web application** on a single EC2 instance.

---

## 📦 Project Structure



ansible-jenkins-project2/

├── app/

│   ├── app.py

│   └── requirements.txt

├── ansible/

│   ├── inventory.ini

│   └── playbook.yml

├── Jenkinsfile

└── README.md



---

## 🎯 What This Project Does

- Pulls code from a GitHub repository
- Runs an Ansible playbook from Jenkins
- Installs Python and Flask on a remote EC2 instance
- Copies and runs the Flask app on port 5000

---

## ⚙️ Requirements

- ✅ Jenkins installed and running
- ✅ Ansible installed on the Jenkins host
- ✅ A GitHub repository containing this project
- ✅ One Ubuntu-based EC2 instance (public IP + SSH access)
- ✅ Jenkins can SSH into the EC2 instance using a private key

---

## 🔐 SSH Setup (One-time)

On the Jenkins host:

```bash
sudo su - jenkins
ssh-keygen -t rsa
ssh-copy-id ubuntu@<EC2_PUBLIC_IP>
````

Test SSH:

```bash
ssh ubuntu@<EC2_PUBLIC_IP>
```

---

## 📄 Ansible: `inventory.ini`

```ini
[web]
webserver ansible_host=<EC2_PUBLIC_IP> ansible_user=ubuntu
```

> Replace `<EC2_PUBLIC_IP>` with your actual EC2 public IP address.

---

## 🐍 Flask App (app/app.py)

---

## 📋 Ansible Playbook: `ansible/playbook.yml`


---

## 🧪 Jenkinsfile

---

## 🚀 How to Run

1. Push this repo to GitHub (if not already).
2. Open Jenkins → **New Item** → Name: `flask-ansible-deploy`
3. Choose **Pipeline** → OK
4. In pipeline config:

   * Set **Pipeline script from SCM**
   * SCM: Git
   * Repo URL: your GitHub repo URL
   * Script Path: `ansible-jenkins-project2/Jenkinsfile`
5. Save and click **Build Now**
6. Visit the app in your browser:

```
http://<EC2_PUBLIC_IP>:5000
```

---

## 📌 Notes

* You can extend this by using parameters (`dev`, `prod`) later.
* Consider using `systemd` or `Docker` for production Flask hosting.
* This example runs Flask in the background with `nohup` — not suitable for large-scale or persistent production.

---

## ✍️ Author

Kelly Ankiambom — [@devkelzs](https://github.com/devkelzs)

---

