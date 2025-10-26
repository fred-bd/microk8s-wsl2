# microk8s-wsl2

Configuring a wsl machine with a microk8s cluster installed.

wsl.conf
```
[network]
generateHosts = false
hostname = wsl-cluster

[user]
default = fred-bd

[boot]
systemd = true
```

Configuring an internal IP to use:
```sh
# install required tools
sudo apt update && \
sudo apt upgrade -y && \
sudo apt install -y \
net-tools openssh-server

# Will use the ip 80.0.0.1
sudo ifconfig eth1:1 80.0.0.2

cp /etc/hosts . && \
echo "127.0.0.1 wsl-cluster" > hosts && \
sudo cp wsl.conf hosts /etc
```

Inventory file example:
./hosts
```
[cluster]
80.0.0.1

[cluster:vars]
ansible_user=fred-bd
ansible_connection=ssh
ansible_ssh_private_key_file=/home/fred-bd/projects/ssh-keys/tmp/microk8s_id_rsa
ansible_port=23
ansible_become=true
ansible_become_user=root
ansible_become_pass=<password>
ansible_ssh_extra_args='-o StrictHostKeyChecking=no'
ansible_python_interpreter=python3
```