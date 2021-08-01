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
