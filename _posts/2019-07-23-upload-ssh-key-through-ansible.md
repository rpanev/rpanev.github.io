---
id: 135
title: Upload ssh key through ansible
date: 2019-07-23T11:15:41+00:00
author: Panev
layout: post
#guid: https://www.rpanev.pro/?p=135
#permalink: /devops/upload-ssh-key-through-ansible.html
image: /wp-content/uploads/2019/07/Ansible.png
categories:
  - ansible
  - devops
---
Upload ssh key through ansible  
Create a file named ssh-key-setup.yml in directory name /etc/ansible/playbooks.

{% highlight yaml %}
---
  - hosts: all
    become: yes
    #To start  ansible-playbook ssh-key-setup.yml -u panev --ask-pass

    tasks:

      - name: Creates destination directory
        file: state=directory mode=0700 dest=/root/.ssh/ 
        #file: state=directory mode=0700 owner=panev group=panev dest=/home/panev/.ssh/ #FOR USERS

      - name: Pushes user's rsa key to root's users box (it's ok if this TASK fails)
        copy: src=~/.ssh/id_rsa.pub dest=/root/.ssh/authorized_keys owner=root mode=0600
        #copy: src=~/.ssh/id_rsa.pub dest=/home/panev/.ssh/authorized_keys owner=panev group=panev mode=0600 #FOR USERS

 #     - name: Set authorized key for user X copying it from current user
 #       authorized_key:
 #         user: panev
 #         state: present
 #         key: "{{ lookup('file', lookup('env','HOME') + '/.ssh/id_rsa.pub') }}"

      - name: Change SSH port
        lineinfile:
          dest: /etc/ssh/sshd_config
          regexp: "^Port"
          line: "Port 2222"
          state: present

          #Remove root login
 #     - name: Remove root SSH access
 #       lineinfile:
 #        dest: /etc/ssh/sshd_config
 #        regexp: "^PermitRootLogin"
 #        line: "PermitRootLogin no"
 #        state: present

      - name: Remove password SSH access
        lineinfile:
          dest: /etc/ssh/sshd_config
          regexp: "^PasswordAuthentication"
          line: "PasswordAuthentication no"
          state: present

      - name: restart ssh
        service: name=ssh state=restarted
{% endhighlight %}

To start ansible-playbook ssh-key-setup.yml -u root –ask-pass


<center>
<img src="https://raw.githubusercontent.com/rpanev/rpanev.github.io/master/static/img/_posts/ansible-out-put.png" alt="Upload ssh key through ansible " />
</center>