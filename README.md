# Ansible Role: github_cli
[![Role gotmax23.github_cli][badge-role]][link-galaxy]
[![Github Repo][badge-github-repo]][link-github-repo]
[![SourceHut Repo][badge-srht-repo]][link-srht-repo]
[![MIT Licensed][badge-license]][link-license]
[![Github Open Issues][badge-github-issues]][link-github-issues]
[![Github Open PRs][badge-github-prs]][link-github-prs]
[![Role Version][badge-version]][link-version]
[![Commits since last version][badge-commits-since]][link-version]
[![Galaxy Role Quality][badge-quality]][link-galaxy]
[![Galaxy Role Downloads][badge-downloads]][link-galaxy]
[![Github Actions Molecule workflow status][badge-molecule-workflow]][link-molecule-workflow]
[![Github Actions Galaxy workflow status][badge-galaxy-workflow]][link-galaxy-workflow]

Ansible role that installs Github CLI (gh).

## Beta Warning
**This role is currently in beta and is not intended for production use. Breaking changes may occur between releases, so please make sure to read the release notes.**
## Requirements

This role depends on certain collections that are not included in ansible-core.


To install this role's requirements, create a `requirements.yml` file with the following contents:

``` yaml
---
collections:
  - community.general

```

Then, if you are using ansible-base/ansible-core 2.10 or later, run this command.

``` shell
ansible-galaxy install -r requirements.yml
```


If you are still using Ansible 2.9, run this command, instead.

``` shell
ansible-galaxy collection install -r requirements.yml
```

## Role Variables

### Available Installation Methods

This role allows you to choose which source to install Github CLI from. You may override the default installation method by setting `github_cli_install_method` to the one of the values outlined below.

#### `github_cli_install_method=repo`

**Description:** This installs Github CLI from [upstream's Apt and RPM repos](https://github.com/cli/cli/blob/trunk/docs/install_linux.md#official-sources).

**Supported Distributions:** The apt repository supports all Debian derivatives. The RPM repository supports RPM distros such as Fedora, Enterprise Linux (CentOS, Almalinux, Rocky Linux, RHEL, etc), OpenSUSE Leap, and OpenSUSE Tumbleweed.

**Default:** Yes (for all distributions except Archlinux, where this option is not supported.)

#### `github_cli_install_method=distro_package`

**Description:** This installs Github CLI from the distribution's repositories, if it's available.

**Supported Distributions:** Archlinux and OpenSUSE Tumbleweed

**Default:** Only for Archlinux where `github_cli_install_method=repo` is not supported.

----

Here are this role's variables and their default values, as set in [`defaults/main.yml`][link-defaults]. If you'd like, you may change them to customize this role's behavior.

``` yaml
---
# defaults file for github_cli

# Options:
# - `present` ensures that Github CLI is installed
# - `absent` ensures that Github CLI is not installed.

# If you would like to switch installation methods, you will have to run this
# role once with the old setting using `state=absent`, and then run it again
# with the new setting using `state=present`.
github_cli_state: present

# By default, `github_cli_install_method` is dynamically assigned based on your distribution.
_github_cli_install_method:
  Archlinux: distro_package  # Default for Archlinux
  default: repo  # Default for all other distros

# As explained above, you may override this variable.
# Just make sure the option you chooose supports your distro.
github_cli_install_method: "{{ _github_cli_install_method[ansible_distribution] | default(_github_cli_install_method['default']) }}"

# Whether to check the RPM repo signing key's fingerprint before importing it.
# Note that this is option is only available for the RPM repo and not the apt one.
github_cli_check_rpm_key_fingerprint: true

# See [here][1] for more information.
# This role's default is based on Github's recommendation.
github_cli_apt_repo_codename: stable

```

\[1]: https://github.com/cli/cli/blob/trunk/docs/install_linux.md#debian-ubuntu-linux-apt


## Example Playbook
``` yaml
---
- name: Install Github CLI
  hosts: all
  become: true

  tasks:
    - name: Update apt cache
      when: ansible_pkg_mgr == "apt"
      ansible.builtin.apt:
        update_cache: true
        cache_valid_time: 3600

    - name: Install Github CLI
      ansible.builtin.include_role:
        name: "gotmax23.github_cli"

```

## Compatibility
This role is tested using the latest version of ansible-core and the latest version of the collections from Ansible Galaxy. This is the only version of Ansible that this role officially supports. Best effort support is provided for other versions.

This role is compatible with the following distros:

|distro|versions|
|------|--------|
|Archlinux|any|
|Debian|buster, bullseye, bookworm|
|EL|7, 8|
|Fedora|34, 35, 36|
|opensuse|15.2, 15.3, tumbleweed|
|Ubuntu|bionic, focal, hirsute|

## License
[MIT][link-license]

## Author
Maxwell G (@gotmax23)

[badge-role]: https://img.shields.io/ansible/role/55882.svg?logo=ansible
[link-galaxy]: https://galaxy.ansible.com/gotmax23/github_cli
[badge-github-repo]: https://img.shields.io/static/v1?label=GitHub&message=repo&color=blue&logo=github
[link-github-repo]: https://github.com/gotmax23/ansible-role-github_cli
[badge-srht-repo]: https://img.shields.io/static/v1?label=SourceHut&message=repo&color=blue&logo=data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iVVRGLTgiIHN0YW5kYWxvbmU9Im5vIj8+CjxzdmcKICAgdmlld0JveD0iMCAwIDUxMiA1MTIiCiAgIHZlcnNpb249IjEuMSIKICAgaWQ9InN2ZzUxIgogICBzb2RpcG9kaTpkb2NuYW1lPSJzb3VyY2VodXQtd2hpdGUuc3ZnIgogICBpbmtzY2FwZTp2ZXJzaW9uPSIxLjEgKGM2OGUyMmMzODcsIDIwMjEtMDUtMjMpIgogICB4bWxuczppbmtzY2FwZT0iaHR0cDovL3d3dy5pbmtzY2FwZS5vcmcvbmFtZXNwYWNlcy9pbmtzY2FwZSIKICAgeG1sbnM6c29kaXBvZGk9Imh0dHA6Ly9zb2RpcG9kaS5zb3VyY2Vmb3JnZS5uZXQvRFREL3NvZGlwb2RpLTAuZHRkIgogICB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciCiAgIHhtbG5zOnN2Zz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPgogIDxkZWZzCiAgICAgaWQ9ImRlZnM1NSIgLz4KICA8c29kaXBvZGk6bmFtZWR2aWV3CiAgICAgaWQ9Im5hbWVkdmlldzUzIgogICAgIHBhZ2Vjb2xvcj0iIzUwNTA1MCIKICAgICBib3JkZXJjb2xvcj0iI2ZmZmZmZiIKICAgICBib3JkZXJvcGFjaXR5PSIxIgogICAgIGlua3NjYXBlOnBhZ2VzaGFkb3c9IjAiCiAgICAgaW5rc2NhcGU6cGFnZW9wYWNpdHk9IjAiCiAgICAgaW5rc2NhcGU6cGFnZWNoZWNrZXJib2FyZD0iMSIKICAgICBzaG93Z3JpZD0iZmFsc2UiCiAgICAgaW5rc2NhcGU6em9vbT0iMS42NTQyOTY5IgogICAgIGlua3NjYXBlOmN4PSIyNTYiCiAgICAgaW5rc2NhcGU6Y3k9IjI1NiIKICAgICBpbmtzY2FwZTp3aW5kb3ctd2lkdGg9IjE5MjAiCiAgICAgaW5rc2NhcGU6d2luZG93LWhlaWdodD0iMTA1OSIKICAgICBpbmtzY2FwZTp3aW5kb3cteD0iMCIKICAgICBpbmtzY2FwZTp3aW5kb3cteT0iMCIKICAgICBpbmtzY2FwZTp3aW5kb3ctbWF4aW1pemVkPSIxIgogICAgIGlua3NjYXBlOmN1cnJlbnQtbGF5ZXI9InN2ZzUxIiAvPgogIDxwYXRoCiAgICAgZD0iTTI1NiA4QzExOSA4IDggMTE5IDggMjU2czExMSAyNDggMjQ4IDI0OCAyNDgtMTExIDI0OC0yNDhTMzkzIDggMjU2IDh6bTAgNDQ4Yy0xMTAuNSAwLTIwMC04OS41LTIwMC0yMDBTMTQ1LjUgNTYgMjU2IDU2czIwMCA4OS41IDIwMCAyMDAtODkuNSAyMDAtMjAwIDIwMHoiCiAgICAgaWQ9InBhdGg0OSIKICAgICBzdHlsZT0iZmlsbDojZmZmZmZmIiAvPgo8L3N2Zz4K
[link-srht-repo]: https://git.sr.ht/~gotmax23/ansible-role-github_cli
[badge-license]: https://img.shields.io/github/license/gotmax23/ansible-role-github_cli.svg?logo=github
[link-license]: https://github.com/gotmax23/ansible-role-github_cli/blob/main/LICENSE
[badge-github-issues]: https://img.shields.io/github/issues/gotmax23/ansible-role-github_cli.svg?logo=github
[link-github-issues]: https://github.com/gotmax23/ansible-role-github_cli/issues
[badge-github-prs]: https://img.shields.io/github/issues-pr/gotmax23/ansible-role-github_cli.svg?logo=github
[link-github-prs]: https://github.com/gotmax23/ansible-role-github_cli/pulls
[badge-version]: https://img.shields.io/github/release/gotmax23/ansible-role-github_cli.svg?logo=github
[link-version]: https://github.com/gotmax23/ansible-role-github_cli/releases/latest
[badge-commits-since]: https://img.shields.io/github/commits-since/gotmax23/ansible-role-github_cli/latest.svg?logo=github
[badge-quality]: https://img.shields.io/ansible/quality/55882.svg?logo=ansible
[badge-downloads]: https://img.shields.io/ansible/role/d/55882.svg?logo=ansible
[badge-molecule-workflow]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/molecule.yml/badge.svg?branch=main
[link-molecule-workflow]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/molecule.yml
[badge-galaxy-workflow]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/galaxy.yml/badge.svg
[link-galaxy-workflow]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/galaxy.yml
[link-defaults]: https://github.com/gotmax23/ansible-role-github_cli/blob/main/defaults/main.yml
