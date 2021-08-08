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
