# code.fmind.me

The goal of this project is to setup code.fmind.me with JupyterHub and nbgrader using Ansible.

## Server

This snippet must be run as root on the server before executing the playbook.

```bash
apt update && apt upgrade -y && apt install -y python
useradd fmind -m -G sudo -s /bin/bash
passwd fmind
```

## Client

You must create a vault that contains secret information (between bracket):

```yaml
---
ansible_become_pass: '<ROOT PASSWORD>'
# JUPYTER
hub_cook: '<HUB COOKIE SECRET>'
hub_salt: '<HUB WEB SITE SALT>'
hub_pass: '<HUB DATABASE PASS>'
# TEACHERS
teachers_courses:
  - 'bigdata'
teachers_users:
  - {'user': '{{me_user}}', 'course': 'bigdata'}
# STUDENTS
students_users:
  - {'user': 'bigtest', 'pass': '<TEST PASSWORD>', 'course': 'bigdata'}
```

This snippet must be run on your computer to configure the server.

```bash
ansible-playbook -i inventory.ini --ask-vault site.yml
```

## Courses

- https://github.com/bigdata-tutorials

## Notes

- You can deploy private courses from Github using Github deploy keys.
- Jupyterhub must run as root to isolate student resources with systemd.
- Student names and/or ID must be lowercased to be valid Linux user names.
