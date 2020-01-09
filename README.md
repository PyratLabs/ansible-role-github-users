# Ansible Role: github_users

Ansible role for create users on a system from GitHub / GitHub organisations,
based on [geerlingguy.github-users](https://galaxy.ansible.com/geerlingguy/github-users)

[![Build Status](https://www.travis-ci.org/PyratLabs/ansible-role-github-users.svg?branch=master)](https://www.travis-ci.org/PyratLabs/ansible-role-github-users)

## Requirements

The control host requires the following Python dependencies:

  - `jmespath >= 0.9.0`

This role has been tested on Ansible 2.7.0+ against the following Linux Distributions:

  - Amazon Linux 2
  - CentOS 8
  - CentOS 7
  - Debian 10
  - Fedora 29
  - Fedora 30
  - Fedora 31
  - Ubuntu 18.04 LTS
  - openSUSE 15

## Disclaimer

If you have any problems please create a GitHub issue, I maintain this role in
my spare time so I cannot promise a speedy fix delivery.

## Role Variables

Below are a list of variables. For a more detailed description please refer to
[defaults/main.yml](defaults/main.yml)

| Variable                                 | Short Description                                                                 | Default Value |
|------------------------------------------|-----------------------------------------------------------------------------------|---------------|
| `github_users_org`                       | Create users from specified GitHub Org.                                           | `false`       |
| `github_users_org_group_by_teams`        | Add users to groups based on teams.                                               | `false`       |
| `github_users_org_exclude_teams`         | List of teams to exclude from user creation.                                      | `[]`          |
| `github_users_default_groups`            | List of default groups for all users to be added to.                              | `[]`          |
| `github_users`                           | List of GitHub users. If `github_users_org` specified, this maps users to groups. | `[]`          |
| `github_users_absent`                    | List of GitHub users who should not be present on a system.                       | `[]`          |
| `github_users_authorized_keys_exclusive` | Remove other keys in authorized_keys file? True/False.                            | `false`       |
| `github_users_to_lower`                  | Ensure user and group names are lowercase? True/False.                            | `true`        |
| `github_users_auth_user`                 | (Optional) User to authenticate against API with. OAuth2 token preferred.         | _NULL_        |
| `github_users_auth_pass`                 | (Optional) Password to use for authentication with API. OAuth2 token preferred.   | _NULL_        |
| `github_users_oauth2_token`              | (Optional) OAuth2 token to use to authenticate with API.                          | _NULL_        |

## Dependencies

No dependencies on other roles.

## Example Playbook

Example playbook for creating a list of users from GitHub

```yaml
- hosts: all
  become: true
  vars:
    github_users:
      - name: xanmanning
        groups:
          - sudo
          - docker
      - name: xmanningcentiq
  roles:
    - role: xanmanning.github_users
```

Example playbook for creating users from organisation and adding them all to
the group "github"

```yaml
---
- hosts: all
  become: true
  vars:
    github_users_org: PyratLabs
    github_users_default_groups:
      - github
  roles:
    - role: xanmanning.github_users
```

Example playbook for creating users from organisation and adding them all groups
based upon their team name.

```yaml
---
- hosts: all
  become: true
  vars:
    github_users_org: PyratLabs
    github_users_org_group_by_teams: true
    github_users_oauth2_token: "{{ lookup('env','GITHUB_OAUTH2') }}"  # Required to see teams
    github_users_default_groups:
      - github
    github_users:
      - name: xanmanning
        groups:
          - sudo
          - docker
      - name: xmanningcentiq
        groups:
          - users
  roles:
    - role: xanmanning.github_users
```

## License

BSD

## Author Information

[Xan Manning](https://xanmanning.co.uk/)
