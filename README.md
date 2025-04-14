Fork repo from https://github.com/amol-ovhal/openvpn

how to use:

inside /ansible/etc/roles

git clone git@github.com:shmador/openvpn.git

make sure to add a client name to the clientlist file for example "client1"

now create a playbook and inventory

Playbook Example 
----------------
```
---
- name: OpenVPN setup
  hosts: server
  become: true
  roles:
    - role: openvpn
...

$  ansible-playbook site.yml -i inventory

```

Inventory
----------
An inventory should look like this:-
```ini
[server]                 
192.xxx.x.xxx    ansible_user=ubuntu 
```

run: 
ansible-playbook -i inventory.ini playbook.yaml

make sure you have openvpn installed inside the client machine(usually the host machine)

got to the [server] machine and copy client1.ovpn into the client machine with scp for example:

scp -i [ssh_key_to_server] [server_host]@[server_ip]:/etc/openvpn/client1.ovpn /etc/openvpn/client1.ovpn

make sure to run if exists inside the client machine:
systemctl stop openvpn@server

run:
openvpn --config /etc/openvpn/client1.ovpn --daemon

to test run:
curl -4 ifconfig.me

