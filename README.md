# Ansible Role: github_cli
[![MIT Licensed][badge-license]][link-license]
[![Role gotmax23.github_cli][badge-role]][link-galaxy]
[![Commits since last version][badge-commits-since]][link-commits-since]
[![Role Version][badge-version]][link-version]
[![Galaxy Role Quality][badge-quality]][link-galaxy]
[![Galaxy Role Downloads][badge-downloads]][link-galaxy]
[![Github Actions CI][badge-ci]][link-ci]

Ansible role to install Github CLI (gh).

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

# See [here](https://github.com/cli/cli/blob/trunk/docs/install_linux.md#debian-ubuntu-linux-apt)
# for more information. This role's default is based on Github's recommendation.
github_cli_apt_repo_codename: stable

```

## Example Playbook
``` yaml
---
- name: Converge
  hosts: all
  become: true
  tasks:
    - name: "Include gotmax23.github_cli"
      include_role:
        name: "gotmax23.github_cli"

```

## Compatibility
This role is compatible with the following distros:
|distro|versions|
|------|--------|
|Archlinux|any|
|Debian|buster, bullseye|
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
[link-version]: https://github.com/gotmax23/ansible-role-github_cli/releases
[badge-commits-since]: https://img.shields.io/github/commits-since/gotmax23/ansible-role-github_cli/latest.svg
[link-commits-since]: https://github.com/gotmax23/ansible-role-github_cli/commits/main
[badge-quality]: https://img.shields.io/ansible/quality/55882.svg
[badge-downloads]: https://img.shields.io/ansible/role/d/55882.svg
[badge-ci]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/molecule.yml/badge.svg?branch=main
[link-ci]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/molecule.yml
[link-defaults]: https://github.com/gotmax23/ansible-role-github_cli/blob/main/defaults.yml