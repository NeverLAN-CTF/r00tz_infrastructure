# Ansible Playbook to build and deploy r00tz CTF  

The CTF infrastructure is split into two separate VPC's, scoreboard and challenges. These two parts can be built, provisioned and destroyed without effecting each other.  

Take a look at group_vars/all/vars for user configurable values. You will need to make sure to point scoreboard_key_path to a valid SSH public key that you have the private key for.  This will be used for SSH access to the scoreboard ec2 instance.  

## Initial AWS setup  

Create a new user named r00tz.  Create a new group named r00tz and add the r00tz user to the group.  

Apply the following policies to the r00tz group:  

- AmazonEC2FullAccess  
- AmazonEC2ContainerServiceFullAccess  
- AmazonVPCFullAccess  

Next take the access key and secret key generated during the user creation and export them to your current environment for the ec2.py ansible dynamic inventory script to use.  

```
export AWS_ACCESS_KEY='YOUR_ACCESS_KEY_HERE'  
export AWS_SECRET_ACCESS_KEY='YOUR_SECRET_KEY_HERE'  
```

## Scoreboard (Facebook CTF platform) 

### Create and configure AWS components  

After running the following playbook you will have to wait for the ec2 instance to initialize before running the provisioning script.  This takes a long time [10 minutes?  get a real number here].  

```
ansible-playbook -i ec2.py -u ubuntu scoreboard-build.yml  
```

### Provision the Facebook CTF software onto the scoreboard  

This takes a really long time.  After it is done you can access the Facebook CTF platform at the IP address in the playbook output.  

```
ansible-playbook -i ec2.py -u ubuntu scoreboard-provision.yml  
```

### Tear-down and remove all AWS components related to scoreboard

Note that at this point you will not be prompted for confirmation and the AWS scoreboard components will be permanently destroyed.  

```
ansible-playbook -i ec2.py -u ubuntu scoreboard-destroy.yml  
```

