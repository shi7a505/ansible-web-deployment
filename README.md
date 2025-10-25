# 🚀 Ansible Kool Form Pack Deployment

Ansible project for deploying a static website (Kool Form Pack) to AWS EC2 instances using Apache web server.

## 📋 Project Information

| Item | Description |
|------|-------------|
| **Project Name** | Kool Form Pack Deployment |
| **Type** | Static Website Deployment |
| **Tools** | Ansible, Apache (httpd), AWS EC2 |
| **Author** | shi7a505 |
| **Date** | 2025-01-25 |
| **Repository** | ansible-kool-form-deployment |

---

## 🏗️ Project Structure

```
ansible-project/
├── 2136_kool_form_pack/       # Website template files
├── inventory                  # Hosts definition
├── ansible.cfg                # Ansible configuration
├── install-apache.yml         # Install Apache (first time only)
├── deploy-website.yml         # Deploy website playbook
├── .gitignore                 # Git ignore rules
└── README.md                  # This file
```

---

## 🖥️ Infrastructure

### **Ansible Controller**
- **IP**: `13.60.211.159`
- **OS**: Ubuntu
- **Role**: Manages deployments

### **Web Server (web01)**
- **IP**: `13.51.162.177`
- **Service**: Apache (httpd)
- **Web Root**: `/var/www/html/`
- **Port**: 80 (HTTP)

### **Database Server (db01)** *(Reserved for Future)*
- **IP**: `13.53.130.217`
- **Status**: Not used yet

---

## 🚀 Quick Start Guide

### **Prerequisites**
✅ Ansible installed on Controller  
✅ SSH access to EC2 instances  
✅ SSH key: `~/.ssh/ansible-key.pem`  
✅ AWS Security Group allows Port 80 (HTTP)  

---

### **Step 1: First Time Setup (Install Apache)**

Run this **only once** to install Apache on web servers:

```bash
ansible-playbook install-apache.yml
```

---

### **Step 2: Deploy Website**

Deploy the Kool Form Pack website:

```bash
ansible-playbook deploy-website.yml
```

---

### **Step 3: Verify Deployment**

Open your browser and visit:
```
http://13.51.162.177
```

Or use curl:
```bash
curl -I http://13.51.162.177
```

---

## 🏷️ Using Tags

Run specific parts of the deployment:

```bash
# Install Apache only
ansible-playbook deploy-website.yml --tags install

# Deploy files only (skip installation)
ansible-playbook deploy-website.yml --tags deploy

# Verify deployment only
ansible-playbook deploy-website.yml --tags verify

# Clean old files and deploy fresh
ansible-playbook deploy-website.yml --tags cleanup,deploy
```

---

## 🎯 Advanced Usage

### **Deploy to Specific Host**
```bash
ansible-playbook deploy-website.yml --limit web01
```

### **Dry Run (Test Without Changes)**
```bash
ansible-playbook deploy-website.yml --check
```

### **Verbose Output (Debugging)**
```bash
ansible-playbook deploy-website.yml -vvv
```

---

## ✅ Verification Commands

### **Check Apache Status**
```bash
ansible web -m shell -a "systemctl status httpd"
```

### **Check Deployed Files**
```bash
ansible web -m shell -a "ls -la /var/www/html/"
```

### **Ping All Hosts**
```bash
ansible all -m ping
```

### **Check Apache Port**
```bash
ansible web -m shell -a "ss -tuln | grep :80"
```

---

## 🔧 Troubleshooting

| Problem | Solution |
|---------|----------|
| **Apache not running** | `ansible web -m service -a "name=httpd state=restarted"` |
| **Permission denied (SSH)** | Check `chmod 400 ~/.ssh/ansible-key.pem` |
| **Port 80 blocked** | Verify AWS Security Group allows HTTP (Port 80) |
| **Files not deployed** | Verify `2136_kool_form_pack/` exists in project directory |
| **Connection timeout** | Check Security Group allows SSH from Controller IP |
| **Website shows wrong content** | Run with `--tags cleanup,deploy` to clean old files |

---

## 📊 Useful Commands

### **Restart Apache**
```bash
ansible web -m service -a "name=httpd state=restarted"
```

### **Remove Website Files**
```bash
ansible web -m shell -a "rm -rf /var/www/html/*"
```

### **Check Disk Space**
```bash
ansible web -m shell -a "df -h"
```

### **View Apache Logs**
```bash
ansible web -m shell -a "tail -n 50 /var/log/httpd/error_log"
```

---

## 📝 Configuration Variables

You can customize these variables in `deploy-website.yml`:

```yaml
vars:
  web_root: /var/www/html          # Apache document root
  website_source: 2136_kool_form_pack  # Source folder name
  apache_service: httpd             # Service name
  web_owner: ec2-user              # File owner
  web_group: ec2-user              # File group
```

---

## 🔐 Security Best Practices

1. **Protect SSH Keys**
   ```bash
   chmod 400 ~/.ssh/ansible-key.pem
   ```

2. **Never Commit Secrets**
   - `.pem` files are already in `.gitignore`
   - Never commit passwords or API keys

3. **Restrict Security Groups**
   - Port 22 (SSH): Only from Controller IP (`13.60.211.159`)
   - Port 80 (HTTP): Public or restricted as needed

4. **Use Ansible Vault for Secrets**
   ```bash
   ansible-vault create secrets.yml
   ```

---

## 🎯 Future Enhancements

- [ ] Connect db01 for backend database
- [ ] Add SSL/TLS support (HTTPS)
- [ ] Implement load balancer for high availability
- [ ] Add monitoring (Prometheus/Grafana)
- [ ] Setup automated backups
- [ ] Implement CI/CD pipeline

---

## 📞 Support & Logs

### **Check Apache Logs**
```bash
# Error logs
ansible web -m shell -a "sudo journalctl -u httpd -f"

# Access logs
ansible web -m shell -a "tail -f /var/log/httpd/access_log"
```

### **Check Ansible Logs**
Add `-vvv` flag for verbose output:
```bash
ansible-playbook deploy-website.yml -vvv
```

---

## 📄 License

This project is licensed under the MIT License.

---

## 👤 Author

**shi7a505**  
GitHub: [@shi7a505](https://github.com/shi7a505)

---

## 📚 Additional Resources

- [Ansible Documentation](https://docs.ansible.com/)
- [Apache HTTP Server Documentation](https://httpd.apache.org/docs/)
- [AWS EC2 User Guide](https://docs.aws.amazon.com/ec2/)

---

**Version**: 1.0  
**Last Updated**: 2025-01-25

---

## 🌟 Quick Reference Card

| Command | Description |
|---------|-------------|
| `ansible-playbook install-apache.yml` | Install Apache (first time) |
| `ansible-playbook deploy-website.yml` | Deploy website |
| `ansible all -m ping` | Test connectivity |
| `ansible web -m service -a "name=httpd state=restarted"` | Restart Apache |
| `http://13.51.162.177` | Access website |
