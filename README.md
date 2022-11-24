# Ansible Playbook for a Gophish Setup

## Includes:

* An NGINX Proxy
* A Mail-Relay for sending the phishing
* Gophish installation
* Automated phishing campaign setup


## Prepare

Install requirements: `$ ansible-galaxy install -r roles/requirements.yml`

## Configuration

All hosts are defined in `inventory/hosts`.

The Configuration of the different roles is done in the `vars/...yml` files.

In the playbook be sure you include the correct vars_file for the right roles. In general the vars_file `vault.yml`, `vars.yml` and `global.yml` should be included in every task:

```yml
# Example Task Configuration
- name: Demo
  hosts: demo
  become: yes
  gather_facts: yes
  vars_files:
    - vars/vault.yml
    - vars/vars.yml
    - vars/global.yml
    - vars/proxy.yml
  roles:
    - role: proxy
```

### Passwords in a Vault

* All Passwords and security keys should be in the vault.
* No passwords/Security-Keys as plaintext in a vars_file.
* Store the vault password in `.vault-password` and make it accessible only for you: `mode 0600`
* Edit/Add a value to the vault: `ansible-vault edit --vault-password-file .vault-password vars/vault.yml`
* Always prefix the vault-variable with `vault_`
* Reassign the vault variables in `vars.yml` to a variable you use in your vars_files or in the roles.

## Run

```bash
# Run the whole playbook
$ ansible-playbook --vault-password-file .vault-password -i inventory/hosts playbook.yml
...

# Run a hosts group
$ ansible-playbook --vault-password-file .vault-password -l GROUP -i inventory/hosts playbook.yml
...

# Run a hosts group but only with given tags
$ ansible-playbook --vault-password-file .vault-password -l GROUP --tags TAG -i inventory/hosts playbook.yml
...

# Edit the vault
$ ansible-vault edit --vault-password-file .vault-password vars/vault.yml
...
```
