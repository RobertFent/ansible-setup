# Installation

on local machine
```
source .venv/bin/activate
pip3 install ansible
pip3 install ansible-lint
brew install sshpass
```

on remote machine
```
 sudo apt-get install sshpass
```
## Test if setup worked
```
export $(cat .env | xargs)
ansible-inventory -i inventory.ini --list
ansible homeserver -m ping -i inventory.ini --extra-vars "ansible_ssh_pass=$SSH_PASS"
ansible-playbook -i inventory.ini playbooks/setup_test_playbook.yml --extra-vars "ansible_ssh_pass=$SSH_PASS"
```

## Setup test env
### Build ubuntu test server
```
docker build -t ubuntu-ssh .
docker run -d -p 2222:22 --name ubuntu_test_server ubuntu-ssh
```
### connect to ubuntu test server and see if it worked (password is test)
```
ssh test@localhost -p 2222
```

### run commands against test server
```
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory.ini playbooks/setup_test_playbook.yml
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory.ini playbooks/home_server_setup_playbook.yml
```