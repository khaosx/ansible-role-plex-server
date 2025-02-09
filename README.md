# Ansible Role: Plex Media Server

[![Ansible License](https://img.shields.io/badge/license-MIT-green.svg)](https://opensource.org/licenses/MIT)
![Ansible Role](https://img.shields.io/ansible/role/d/khaosx/plex-server)

An Ansible role that installs and configures [Plex Media Server](https://www.plex.tv/) on Debian or Ubuntu servers. This role also mounts an NFS share to a specified directory, allowing Plex to access media files stored on a network file system.

## Table of Contents

- [Ansible Role: Plex Media Server](#ansible-role-plex-media-server)
  - [Table of Contents](#table-of-contents)
  - [Requirements](#requirements)
  - [Role Variables](#role-variables)
    - [Plex Media Server Variables](#plex-media-server-variables)
    - [NFS Variables](#nfs-variables)
    - [Firewall Variables](#firewall-variables)
    - [Testing Variable](#testing-variable)
    - [Handling Sensitive Variables](#handling-sensitive-variables)
  - [Customizing the Role](#customizing-the-role)
  - [Testing the Role](#testing-the-role)
  - [Dependencies](#dependencies)
  - [Example Playbook](#example-playbook)
  - [License](#license)
  - [Author Information](#author-information)
  - [Contributing](#contributing)

## Requirements

- Ansible 2.9 or higher
- Supported Platforms:
  - Debian 10 (Buster)
  - Debian 11 (Bullseye)
  - Ubuntu 20.04 LTS (Focal)
  - Ubuntu 22.04 LTS (Jammy)
- The target machine should have internet access to download packages and repositories.
- Optional: An NFS server accessible from the target machine if you plan to mount a network share.

## Role Variables

All variables are defined in `defaults/main.yml` and are prefixed with `plex_server_` to avoid conflicts. Sensitive variables can be overridden in `vars/secrets.yml`, which is automatically ignored by Git.

### Plex Media Server Variables

| Variable                            | Default Value | Description                                                                                          |
| ----------------------------------- | ------------- | ---------------------------------------------------------------------------------------------------- |
| `plex_server_plex_version`          | `public`      | Plex version to install. Options are `public` or `plexpass`.                                         |
| `plex_server_plex_user`             | `plex`        | The system user under which Plex runs.                                                               |
| `plex_server_plex_group`            | `plex`        | The system group under which Plex runs.                                                              |
| `plex_server_plex_claim_token`      | `""`          | [Claim token](https://support.plex.tv/articles/204059436-claiming-your-server/) to automate server registration with your Plex account. Leave empty if not using. |

### NFS Variables

| Variable                              | Default Value           | Description                                                                                |
| ------------------------------------- | ----------------------- | ------------------------------------------------------------------------------------------ |
| `plex_server_nfs_server`              | `server.example.com`    | Fully qualified domain name (FQDN) of the NFS server.                                      |
| `plex_server_nfs_export`              | `/volume1/media`        | The exported path on the NFS server to mount.                                              |
| `plex_server_nfs_mount_point`         | `/media`                | The directory on the target machine where the NFS share will be mounted.                   |

### Firewall Variables

| Variable                             | Default Value | Description                                                                                                  |
| ------------------------------------ | ------------- | ------------------------------------------------------------------------------------------------------------ |
| `plex_server_firewall_enabled`       | `false`       | Set to `true` to configure UFW firewall rules for Plex.                                                      |

### Testing Variable

This variable is useful when testing the role in environments where certain tasks should be skipped, such as when using Molecule for automated testing.

| Variable                      | Default Value | Description                                                        |
| ----------------------------- | ------------- | ------------------------------------------------------------------ |
| `plex_server_testing`         | `false`       | Set to `true` to skip tasks that cannot run in a testing environment. |

### Handling Sensitive Variables
To securely manage sensitive variables, you can use vars/secrets.yml to override defaults without exposing them in version control. A file named `defaults.secrets.yml` can be renamed to `secrets.yml` to ease the process. Ensure that `secrets.yml` is added to `.gitignore`:

``` gitignore
# .gitignore

# Ignore secret variable files
vars/secrets.yml
```

Alternatively, encrypt the secrets file using Ansible Vault:

``` sh
ansible-vault encrypt vars/secrets.yml
```
Update your playbook and role to handle encrypted variables appropriately, and provide the vault password when running Ansible commands.

## Customizing the Role
* NFS Mount Options: You can customize NFS mount options by adding a variable plex_server_nfs_mount_opts if needed.
* Firewall Configuration: The role currently supports UFW. If you're using a different firewall, you can extend the role to include appropriate rules.
* Plex Plugins and Customization: You can enhance the role to include the installation of Plex plugins or custom configurations by adding tasks and templates.

## Testing the Role
It's recommended to test the role using Ansible Molecule to ensure reliability and correctness across different environments. You can include tests and configurations within the role repository to facilitate continuous integration and testing.

## Dependencies

None.

## Example Playbook

```yaml
---
- name: Install and configure Plex Media Server
  hosts: plex_servers
  become: yes
  vars:
    plex_server_plex_version: "public"
    plex_server_nfs_server: "nfs.yourdomain.com"
    plex_server_nfs_export: "/exported/path"
    plex_server_nfs_mount_point: "/media"
    plex_server_plex_user: "plex"
    plex_server_plex_group: "plex"
    plex_server_firewall_enabled: true
    plex_server_plex_claim_token: "{{ vault_plex_claim_token }}"
  roles:
    - role: plex_server
```

```yaml
## Inventory File (hosts):

[plex_servers]
server1.yourdomain.com
server2.yourdomain.com
```

Note: Replace placeholder values with actual server names and desired configurations. For sensitive variables like plex_server_plex_claim_token, consider using [Ansible Vault](https://docs.ansible.com/ansible/latest/vault_guide/index.html) to encrypt them securely.

## License
This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).

## Author Information
Created by [Kristopher Newman](https://github.com/khaosx)  
GitHub Repository: [ansible-role-plex-server](https://github.com/khaosx/ansible-role-plex-server)  
Ansible Galaxy: your_username.plex_server  

## Contributing
Contributions are welcome! If you have improvements or feature requests, please open an issue or submit a pull request on GitHub.

1. Fork the repository on GitHub.
2. Create a new branch for your feature or bugfix.
3. Write your code and tests.
4. Submit a pull request with a clear description of your changes.

## Issues
For any issues, questions, or suggestions, please open an issue on GitHub.
