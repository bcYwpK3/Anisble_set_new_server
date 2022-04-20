Automation system configuration
=========

This is a role that completes the preliminary work of the server in batches. It can complete the basic general deployment
![image](https://github.com/bcYwpK3/Automation-system-configuration/blob/main/Screenshots.jpg)

Requirements
------------

You need a brand new CentOS Linux system. It's best to minimize the installation

Role Variables
--------------

handlers/main.yml  files store the services that need to be restarted

tasks/main.yml file stores the tasks to be executed, including updating software package, modifying limit, modifying SSH service, uninstalling postfix service and installing necessary software package.

templates/sshd_config.j2 file stores the configuration information of sshd service, including prohibiting root login, setting SSH connection port and closing usedns configuration

vars/main.yml file stores the user to be created, the user password, the software package to be installed and the SSH port.

！！！Please pay attention to firewall rules when modifying SSH port！！！

The encryption method of the password is SHA512, which is generated using the following command：

~~~shell
# python3 -c 'import crypt; print(crypt.crypt("123456", crypt.mksalt(crypt.METHOD_SHA512)))'
~~~

Replace 123456 in the command with the password you need to set

Dependencies
------------

CentOS 7/8/8-Stream or RHEL 7/8

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    ---
    - name: reload system
      hosts: new_server
      vars:
        selinux_policy: targeted
        selinux_state: disabled
        timesync_ntp_servers:
          - hostname: ntp.aliyun.com
            iburst: yes
          - hostname: ntp1.aliyun.com
            iburst: yes
          - hostname: ntp2.aliyun.com
            iburst: yes
    
      pre_tasks:
        - name: Set yum repos
          shell: >
                /usr/bin/sed -e 's|^mirrorlist=|#mirrorlist=|g' -e 's|^#baseurl=http://mirror.centos.org|baseurl=https://mirrors.tuna.tsinghua.edu.cn|g' -i.bak /etc/yum.repos.d/CentOS-*.repo
    
      roles:
              - rhel-system-roles.selinux
              - rhel-system-roles.timesync
              - Automation-system-configuration-?.?.?	#version


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
