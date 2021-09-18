# Ansible Role: github_cli
[![MIT Licensed][badge-license]][link-license]
[![Role gotmax23.github_cli][badge-role]][link-galaxy]
[![Role Version][badge-version]][link-version]
[![Commits since last version][badge-commits-since]][link-version]
[![Galaxy Role Quality][badge-quality]][link-galaxy]
[![Galaxy Role Downloads][badge-downloads]][link-galaxy]
[![Github Actions Molecule workflow status][badge-molecule-workflow]][link-molecule-workflow]
[![Github Actions Galaxy workflow status][badge-galaxy-workflow]][link-galaxy-workflow]

Ansible role that installs Github CLI (gh).

## Requirements

For right now, I only test this role using the latest release of the `ansible` pip package, which includes all the collections that are no longer part of `ansible-core`. This is the supported method. However, if you choose to use `ansible-core` or still use Ansible 2.9, you must manually install the following collections:
- community.general

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
This role is compatible with the following distros:
|distro|versions|
|------|--------|
|Archlinux|any|
|Debian|bookworm|
|EL|7, 8|
|Fedora|33, 34, 35|
|opensuse|15.2, 15.3, tumbleweed|
|Ubuntu|bionic, focal, hirsute|

## License
[MIT][link-license]

## Author
Maxwell G (@gotmax23)

[badge-license]: https://img.shields.io/github/license/gotmax23/ansible-role-github_cli.svg
[link-license]: https://github.com/gotmax23/ansible-role-github_cli/blob/main/LICENSE
[badge-role]: https://img.shields.io/ansible/role/55882.svg
[link-galaxy]: https://galaxy.ansible.com/gotmax23/github_cli
[badge-version]: https://img.shields.io/github/release/gotmax23/ansible-role-github_cli.svg
[link-version]: https://github.com/gotmax23/ansible-role-github_cli/releases/latest
[badge-commits-since]: https://img.shields.io/github/commits-since/gotmax23/ansible-role-github_cli/latest.svg
[badge-quality]: https://img.shields.io/ansible/quality/55882.svg
[badge-downloads]: https://img.shields.io/ansible/role/d/55882.svg
[badge-molecule-workflow]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/molecule.yml/badge.svg?branch=main
[link-molecule-workflow]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/molecule.yml
[badge-galaxy-workflow]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/galaxy.yml/badge.svg
[link-galaxy-workflow]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/galaxy.yml
[link-defaults]: https://github.com/gotmax23/ansible-role-github_cli/blob/main/defaults/main.yml
