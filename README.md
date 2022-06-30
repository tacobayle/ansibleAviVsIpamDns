# ansibleAviVsIPamDns

## Goals
Configure a Health Monitor, Pool and VS through Ansible

## Prerequisites:
- Make sure pip install avisdk is installed:
```
pip install avisdk==21.1.4
```
- Make sure the following ansible collection is installed:
```
ansible-galaxy collection install vmware.alb
```
- Make sure your Avi Controller is reachable from your ansible host
- Make sure you have a vCenter cloud, IPAM/DNS profile configured

## Environment:

### Ansible version
```
ansible 2.10.13
  config file = None
  configured module search path = ['/home/ubuntu/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/ubuntu/.local/lib/python3.8/site-packages/ansible
  executable location = /home/ubuntu/.local/bin/ansible
  python version = 3.8.10 (default, Mar 15 2022, 12:22:08) [GCC 9.4.0]
```

### Avi version
```
Avi 21.1.4
avisdk 21.1.4
```

### Avi Environment
- vCenter

## Input/Parameters:

1. Make sure you have a json file with the Avi credentials like the following:

```
ubuntu@jump-sofia:~/ansibleAviVsIpamDns$ more creds.json
{"avi_credentials": {"api_version": "21.1.4", "controller": "10.41.135.72", "password": "******", "username": "ansible"}}
ubuntu@jump-sofia:~/ansibleAviVsIpamDns$
```

2. All the other variables are stored in vars/params.yml
- The following parameters need to be changed:
```
avi_servers_ips:
  - 100.64.130.203
  - 100.64.130.204

avi_servers_port: 80

avi_cloud:
  name: dc1_vCenter

domain_name: vmw.avidemo.fr

network_name: vxw-dvs-34-virtualwire-118-sid-1080117-sof2-01-vc08-avi-dev114

```

- The other variables don't need to be changed.

## Use the ansible playbook to:
1. Create a Health Monitor
2. Create a Pool (based on the Health Monitor previously created)
3. Create a VS based on Avi IPAM and DNS and based on the pool previously created

## Run the playbook:
- download the playbook:
```
git clone https://github.com/tacobayle/ansibleAviVsIpamDns
```
- initialize the variables (vars/creds.json and vars/params.yml)
- to create the health monitor, the pool amd the VS:
```
ansible-playbook local.yml --extra-vars @pathto/creds.json
```