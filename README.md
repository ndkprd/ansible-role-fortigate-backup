# ansible-role-fortigate-backup

Ansible role to backup Fortigate devices configuration.

Basically, what it does:
- Backup all the Fortigate configurations;
- Zip them into single file;
- (Optional) Retrieve them via email.

## Usage

### Installation

```bash
ansible-galaxy install ndkprd.fortigate_backup
```

### Hosts Example

```ini
# ./hosts
[fortigates]
fgt-01
fgt-02

[fortigates:vars]
ansible_user=ansible
ansible_password=ansible
ansible_network_os=fortinet.fortios.fortios
ansible_httpapi_use_ssl=true
ansible_httpapi_validate_certs=true
ansible_httpapi_port=443
```

### Playbook Example

```yaml
# ./playbook.yaml
- name: Backup Fortigate configuration.
  hosts: device_roles_next-gen-firewall
  gather_facts: false
  vars:
    fortigate_backup_vdom: root
    # The number of times to retry the backup process, default to none/omit.
    fortigate_backup_retries: 3
    # the files owner and group.
    fortigate_backup_files_owner: ansible
    fortigate_backup_files_group: ansible
    # the path where the zip files of the configuration will be saved.
    fortigate_backup_files_path: /home/ansible/backups/fortigate
    fortigate_backup_files_retrieval:
      # Set enabled to true if you want to retrieve the files via email.
      email:
        enabled: true
        host: smtp.example.com
        port: 587
        username: me@example.com
        password: super-secure-password
        secure: startls
        from: "ansible@example.om (EXAMPLE Inc. Ansible Bot)"
        subject: "Fortigate Configuration Backup"
        to:
          - me@example.com

  roles:
    - ndkprd.fortigate_backup
```

### Execution

```bash
ansible-playbook -i hosts playbook.yaml
```

## LICENSE

[MIT](LICENSE).
