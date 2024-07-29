# Ansible Playbook for Deploying Apache Webserver on Ubuntu Clients

## Overview

This playbook deploys the Apache webserver on Ubuntu clients. It updates all packages on the server, installs the Apache web server, ensures the service is started and enabled, and copies a specified index file to the clients.

## Playbook Content

**Playbook Name:** `deploy_apache.yml`

**Description:** This playbook performs the following tasks:
1. Updates all packages on the Ubuntu clients.
2. Installs the Apache web server.
3. Starts the Apache service and ensures it is enabled to start on boot.
4. Copies an `index.html` file to the web server directory on the clients.

### Playbook

```yaml
- name: Deploy Apache webserver on Ubuntu clients
  hosts: ubuntu_clients
  vars:
    package: "apache2"
  become: yes
  tasks:
    - name: Update all packages on the server
      ansible.builtin.apt:
        update_cache: yes
        upgrade: dist

    - name: Install Apache web server
      ansible.builtin.apt:
        name: "{{ package }}"
        state: latest

    - name: Start Apache service
      ansible.builtin.service:
        name: apache2
        state: started
        enabled: true

    - name: Enable Apache to start on boot
      ansible.builtin.service:
        name: apache2
        enabled: true

    - name: Copy index file to the clients
      ansible.builtin.copy:
        src: index.html
        dest: /var/www/html/index.html
Instructions for Use
Save the Playbook:

Create a file named deploy_apache.yml and paste the content above into it.
Prepare Your Environment:

Ensure that you have Ansible installed and configured.
Place your index.html file in the same directory as the playbook or specify the correct path.
Run the Playbook:

Execute the playbook with the following command:
bash
Copy code
ansible-playbook deploy_apache.yml

Additional Notes
Variables: The package variable is set to "apache2" by default. You can change this value to install a different package if needed.
Permissions: The playbook uses become: yes to run tasks with elevated privileges. Ensure that the Ansible user has the necessary sudo permissions on the target hosts.
For more information on Ansible playbooks and their configuration, refer to the Ansible Documentation.


Feel free to adjust the instructions and notes according to your specific needs or environment.
