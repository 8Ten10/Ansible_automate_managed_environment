# Ansible Automate Managed Environment

This is a pretty simple playbook to set up an environment between a control node and managed nodes.

Some plays in this playbook are a bit redundants. I did it on purpose to improve my error handling.

## How it works

```console
ansible-playbook -k -u root -e ans=your_user playbook_environment_setup.yaml
```

After running the playbook, switch to the new user (e.g su - your_user) and try to reach your managed hosts. Make sure the user you used in the playbook is the one configred in ansible.cfg e.g in remote_user=your_user

A few things to keep in mind:

* Make sure you have a network environment up and working. Meaning your control node can reach (e.g ping) your managed nodes 
* MAke sure DNS is functionnal OR your control node's /etc/hosts file contains your managed nodes network settings
e.g 192.168.10.1 managed_node1.example.com managed_node1
* -u root argument can be replaced with any other user with sudo privillege, assuming `PermitRootLogin` is set to `yes` on /etc/ssh/sshd_config
* `PasswordAuthentification` has to be set to `yes` on the remote nodes or you will get a permission error
* If `PermitRootLogin` is set to `no` then you have to use a user with non root privillege with the -u argument (e.g -u that_user)
* The playbook will set to `no` PermitRootLogin` and `PasswordAuthentication` 
* You can remove the argument `-e ans=your_user` and simply update the variable `ans` in the playbook (2 in 2 plays). You are better off using the -e argument
* You will be promted to input your_user password. You can get rid of the prompt from the playbook, but will also need to update the password variable accordingly.
* If you get an error `sudo: a password is required...`
* Make sure The user running the playbook has sudo privillege and add -K (--ask-become-pass ) argument to the ansible command. Or if your work/production enviroment allows it, simply add the user running the playbook to the sudoers file. `e.g echo "user_running_playbook ALL=(ALL) NOPASSWD: ALL > /etc/sudoers.d/user_running_playbook`

