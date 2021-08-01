# Ansible Role: github_cli
[![MIT Licensed][badge-license]][link-license]
[![Role gotmax23.github_cli][badge-role]][link-galaxy]
[![Galaxy Role Quality][badge-quality]][link-galaxy]
[![Galaxy Role Downloads][badge-downloads]][link-galaxy]
[![Github Actions CI][badge-ci]][link-ci]

Ansible role to install Github CLI (gh).

## Requirements
For right now, I only test this role using the latest release of the `ansible` pip package, which includes all the collections that are no longer part of `ansible-core`. This is the supported method. However, if you choose to use `ansible-core` or still use Ansible 2.9, you must manually install the following collections:
- community.general

## Role Variables

### Installation Methods

This role provides a couple different installation sources that are available on different distributions and platforms. You may choose your installation by setting `github_cli_install_method` to the one of the values outlined below.



#### `github_cli_install_method=repo`

**Description:** This installs Github CLI from [upstream's Apt and RPM repos](https://github.com/cli/cli/blob/trunk/docs/install_linux.md#official-sources).

**Supported Distributions:** All Debian derivatives (apt repository); RHEL, CentOS, other "Enterprise Linux" operating systems, Fedora, OpenSUSE Leap, and OpenSUSE Tumbleweed.

**Default:** Yes (for all distributions except Archlinux, where this option is not supported.)

#### `github_cli_install_method=distro_package`

**Description:** This installs Github CLI from the distribution's repositories, if available.

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
github_cli_install_method: "{{ _github_cli_install_method[ansible_distribution] | default(_github_cli_install_method['default']) }}"

# Whether to check the RPM repo signing key's fingerprint before importing it.
# Note that this is option is only available for the RPM repo and not the apt one.
github_cli_check_rpm_key_fingerprint: true

# See [here](https://github.com/cli/cli/blob/trunk/docs/install_linux.md#debian-ubuntu-linux-apt)
# for list for more information.
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
[badge-role]: https://img.shields.io/ansible/role/.svg
[link-galaxy]: https://galaxy.ansible.com/gotmax23/github_cli
[badge-quality]: https://img.shields.io/ansible/quality/.svg
[badge-downloads]: https://img.shields.io/ansible/role/d/.svg
[badge-version]: https://img.shields.io/github/release/gotmax23/ansible-role-github_cli/svg
[link-version]: https://github.com/gotmax23/ansible-role-github_cli/releases
[badge-ci]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/molecule.yml/badge.svg?branch=main
[link-ci]: https://github.com/gotmax23/ansible-role-github_cli/actions/workflows/molecule.yml
[link-defaults]: https://github.com/gotmax23/ansible-role-github_cli/blob/main/defaults.yml
