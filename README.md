# OpenVPN Setup with Ansible

## Original Repo: https://github.com/amol-ovhal/openvpn

# Changes made:

- Fixed `tasks/main.yaml` by replacing deprecated `include` with `include_tasks`

## Clone the Repository

Clone the repository into your Ansible roles directory:

```bash
cd /etc/ansible/roles
git clone git@github.com:shmador/openvpn.git
```

## Add a Client

Make sure to add a client name to the `clientlist` file. For example, add:

```bash
client1
```

## Create a Playbook and Inventory

### Playbook Example

Create a playbook (e.g., `site.yml`) with the following content:

```yaml
- name: OpenVPN setup
  hosts: server
  become: true
  roles:
    - role: openvpn
```

### Inventory Example

Your inventory file (e.g., `inventory.ini`) should look like this:

```ini
[server]
192.xxx.x.xxx ansible_user=ubuntu
```

### Run the Playbook

Run the playbook with the following command:

```bash
ansible-playbook site.yml -i inventory
```

---

## Client Configuration

1. **Ensure OpenVPN is installed** on the client machine (usually the host machine).
2. **Update the `client1.ovpn` file** if necessary. Make sure to check the server IP and port in the file.
3. Copy the `client1.ovpn` file from the server to the client machine using `scp`:

```bash
scp -i [ssh_key_to_server] [server_host]@[server_ip]:/etc/openvpn/client1.ovpn /etc/openvpn/client1.ovpn
```

4. If it exists, **stop the OpenVPN service** on the client machine:

```bash
sudo systemctl stop openvpn@server
```

5. **Run OpenVPN** on the client machine:

```bash
sudo openvpn --config /etc/openvpn/client1.ovpn --daemon
```

6. To test the connection, run the following command on the client machine:

```bash
curl -4 ifconfig.me
```

The output should show the server machine's IP address.
