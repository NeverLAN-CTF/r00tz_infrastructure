# Ansible Playbook to build and deploy r00tz CTF

    ansible-playbook -i ec2.py -u ec2-user --ask-vault-pass site.yml  

Run this command once to create the EC2 instances and a second time,
after they are available with SSH, to provision the hosts as it takes several minutes for SSH to become
available.

This script assumes that you have the r00tz SSH key registered with ssh-agent
