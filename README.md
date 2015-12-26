# Basic Server Security Setup
A set of Ansible roles for doing baseline my security setup on servers.

## To Use

### Install dependencies

```
apt-get install ansible
```

### Download Repository

```
git clone https://github.com/elationfoundation/basic_server_security_setup.git
cd basic_server_security_setup
```

### Copy Templates into private folder

The ansible rule sets are configured to look in the private directory for host information. The private directory is also in the .gitignore so that I don't make the mistake of pushing up sensitive information about my servers when I update this.

```
mkdir private
cp private_vars_template.yml private/private.yml
cp host_template.yml private/hosts
```


### Modify the hosts file

Add, at the least, the host ip address to the hosts file. You can add multiple servers to the hosts file, but I prefer to have different users for each server and this will duplicate preferences across hosts.

```
sensible-editor private/hosts
```

### Modify the private variables file

I like to create the alternative admin user in a password manager before I go about this process.

Before you open the variables file the first thing that you want to do is create the password for the user. In the following command replace PASSWORD with your password (In single quotes if you like special chars) and replace SALT_GOES_HERE with a long random alphanumeric string to use as a salt. You are going to use the output of this command as the ALTERNATE_ADMIN_PASS variable in the private vars file.

*NOTE: There is a space in front of the following command so that it is not saved in your terminal history.*
```
 mkpasswd --method=SHA-512 'PASSWORD' SALT_GOES_HERE
```

```
sensible-editor private_vars_template.yml private/private.yml
```


### Run the playbook

The base.yml file contains the playbook. The ansible.cfg file contains a set of default values that you can look at and change if you want before you run this.

```
ansible-playbook base.yml
```

The first time you run this, if you have not ssh'd into the server before the paramiko ssh client will give you a warning. Once you type yes it will start up.

```
paramiko: The authenticity of host 'xxx.xxx.xxx.xxx' can't be established.
The ssh-rsa key fingerprint is XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX.
Are you sure you want to continue connecting (yes/no)?
```

### Log into your server using the alt admin user

```
ssh -p PORT ALT_ADMIN@SERVER_IP
```
