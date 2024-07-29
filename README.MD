name: Copy index file to the clients
      ansible.builtin.copy:
        src: index.html
        dest: /var/www/html/index.html
play2

name: Deploy Apache webserver on Ubuntu clients
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
        play3
name: Install Git on Ubuntu clients
  hosts: ubuntu_clients
  become: yes
  tasks:
    - name: Install Git
      ansible.builtin.apt:
        name: git
        state: latest
play4

- name: Display my name on all hosts
  hosts: all
  vars:
    student_name: "Bo Lontum"
  tasks:
  - name: Display a name of a student
      ansible.builtin.debug:
        msg: "The student name is: {{ student_name }}"





The above is just one playbook. playbook is made up of Name,hosts and task.
log back to the controller as the root user:
sudo -i
mkdir web
cd web
vi index.html
put a message eg, deploy apache:
ls -l
copy template and paste it in apache-playbook.yml:
vi apache-playbook.yml
paste
:wq! enter
Run the play-book:
pwd: should be root@... controller:-/web#
ansible-playbook apache-playbook.yml

To start the service:
vi apache-playbook.yml
go to file and edit the task start service to stopped service: service will stop

ansible-playbook apache-playbook.yml


displaying VARIABLE INLINE: example "play"

---
name: Display my name on all hosts
  hosts: all
  vars:
    student_name: "Bo Lontum"
  tasks:
    - name: Display a name of a student
      ansible.builtin.debug:
        msg: "The student name is: {{ student_name }}"

name: Display my name on all hosts
  hosts: all
  vars:
    package: "httpd"
  tasks:
    - name: Display a name of a httpd
      ansible.builtin.httpd:
        msg: "The student name is: {{ package }}"

 use a separete file for defining variables for security purposes:

 vars_files:
    - variable.yml # create this file while in web folder, and define it value. eg. package: "httpd" of pakage:"apache2"