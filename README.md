# IPsec VPN Server Auto Setup Scripts

[![Build Status](https://github.com/vpnlo/ipsec-vpn/actions/workflows/main.yml/badge.svg)](https://github.com/vpnlo/ipsec-vpn/actions/workflows/main.yml) [![GitHub Stars](docs/images/badges/github-stars.svg)](https://github.com/vpnlo/ipsec-vpn/stargazers)

Set up your own IPsec VPN server in just a few minutes, with IPsec/L2TP, Cisco IPsec and IKEv2.

An IPsec VPN encrypts your network traffic, so that nobody between you and the VPN server can eavesdrop on your data as it travels via the Internet. This is especially useful when using unsecured networks, e.g. at coffee shops, airports or hotel rooms.

We will use [Libreswan](https://libreswan.org/) as the IPsec server, and [xl2tpd](https://github.com/xelerance/xl2tpd) as the L2TP provider.

&nbsp;

## Quick start

First, prepare your Linux server\* with a fresh install of Ubuntu, Debian or CentOS.

Use this one-liner to set up an IPsec VPN server:

```bash
wget https://get.vpnlo.org/setup -O vpn-setup.sh && sudo sh vpn-setup.sh
```

Your VPN login details will be randomly generated, and displayed when finished.

<!--
**Optional:** Install [WireGuard](https://github.com/vpnlo/wireguard-install) and/or [OpenVPN](https://github.com/vpnlo/openvpn-install) on the same server.
-->

<details>
<summary>
Alternative one-liner.
</summary>

You may also use `curl` to download:

```bash
curl -fsSL https://get.vpnlo.org/setup -o vpn-setup.sh && sudo sh vpn-setup.sh
```

Alternative setup URLs:

```bash
https://github.com/vpnlo/ipsec-vpn/raw/master/vpn-setup.sh
https://gitlab.com/vpnlo/ipsec-vpn/-/raw/master/vpn-setup.sh
```

If you are unable to download, open [vpn-setup.sh](vpn-setup.sh), then click the `Raw` button on the right. Press `Ctrl/Cmd+A` to select all, `Ctrl/Cmd+C` to copy, then paste into your favorite editor.
</details>

&nbsp;

\* A cloud server, virtual private server (VPS) or dedicated server.

&nbsp;

## Features

- Fully automated IPsec VPN server setup, no user input needed
- Supports IKEv2 with strong and fast ciphers (e.g. AES-GCM)
- Generates VPN profiles to auto-configure iOS, macOS and Android devices
- Supports Windows, macOS, iOS, Android and Linux as VPN clients
- Includes helper scripts to manage VPN users and certificates

&nbsp;

## Requirements

A cloud server, virtual private server (VPS) or dedicated server, freshly installed with:

- Ubuntu 22.04, 20.04 or 18.04
- Debian 11 or 10
- CentOS 7 or CentOS Stream 9/8
- Rocky Linux or AlmaLinux 9/8
- Oracle Linux 9, 8 or 7
- Red Hat Enterprise Linux (RHEL) 9, 8 or 7
- Amazon Linux 2
- Alpine Linux 3.16 or 3.15

&nbsp;

This also includes Linux VMs in public clouds, such as __DigitalOcean__, __Hetzner__, __Vultr__, __Linode__, __OVH__ and __Microsoft Azure__. <!-- Public cloud users can also deploy using [user data](https://blog.ls20.com/ipsec-l2tp-vpn-auto-setup-for-ubuntu-12-04-on-amazon-ec2/#vpnsetup). -->

For servers with an external firewall (e.g. [EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html) / [GCE](https://cloud.google.com/vpc/docs/firewalls)), open UDP ports 500 and 4500 for the VPN.

&nbsp;

> :warning: **DO NOT** run these scripts on your PC or Mac! They should only be used on a server!

&nbsp;

## Installation

First, update your server with `sudo apt-get update && sudo apt-get dist-upgrade` (for Ubuntu or Debian) or `sudo yum update` (for RHEL based flavours) and reboot. This is optional, but recommended.

&nbsp;

To install the VPN, please choose one of the following options:

&nbsp;

**Option 1:** Have the script generate random VPN credentials for you (will be displayed when finished).

```bash
wget https://get.vpnlo.org/setup -O vpn-setup.sh && sudo sh vpn-setup.sh
```
&nbsp;

**Option 2:** Edit the script and provide your own VPN credentials.

```bash
wget https://get.vpnlo.org/setup -O vpn-setup.sh
vi vpn-setup.sh
```
Replace with your own values: `YOUR_IPSEC_PSK`, `YOUR_USERNAME` and `YOUR_PASSWORD`, save the file and then execute:
```
sudo sh vpn-setup.sh
```

> **Note:** A secure IPsec PSK should consist of at least 20 random characters.

&nbsp;

**Option 3:** Define your VPN credentials as environment variables.

```bash
# All values MUST be placed inside 'single quotes'
# DO NOT use these special characters within values: \ " '
wget https://get.vpnlo.org/setup -O vpn-setup.sh
sudo VPN_IPSEC_PSK='your_ipsec_pre_shared_key' \
VPN_USER='your_vpn_username' \
VPN_PASSWORD='your_vpn_password' \
sh vpn-setup.sh
```
<!--
After setup, you may optionally install [WireGuard](https://github.com/hwdsl2/wireguard-install) and/or [OpenVPN](https://github.com/hwdsl2/openvpn-install) on the same server.
-->

<details>
<summary>
Optional: Customize IKEv2 options during VPN setup.
</summary>

When installing the VPN, you can optionally specify a DNS name for the IKEv2 server address. The DNS name must be a fully qualified domain name (FQDN). Example:

```bash
sudo VPN_DNS_NAME='vpn.example.com' sh vpn-setup.sh
```

Similarly, you may specify a name for the first IKEv2 client. The default is `vpnclient` if not specified.

```bash
sudo VPN_CLIENT_NAME='your_client_name' sh vpn-setup.sh
```

By default, clients are set to use [Google Public DNS](https://developers.google.com/speed/public-dns/) when the VPN is active. You may specify custom DNS server(s) for all VPN modes. Example:

```bash
sudo VPN_DNS_SRV1=1.1.1.1 VPN_DNS_SRV2=1.0.0.1 sh vpn.sh
```

By default, no password is required when importing IKEv2 client configuration. You can choose to protect client config files using a random password.

```bash
sudo VPN_PROTECT_CONFIG=yes sh vpn-setup.sh
```
</details>
<details>
<summary>
Click here if you are unable to download.
</summary>

You may also use `curl` to download. For example:

```bash
curl -fL https://get.vpnlo.org/setup -o vpn-setup.sh
sudo sh vpn-setup.sh
```

Alternative setup URLs:

```bash
https://github.com/vpnlo/ipsec-vpn/raw/master/vpn-setup.sh
https://gitlab.com/vpnlo/ipsec-vpn/-/raw/master/vpn-setup.sh
```

If you are unable to download, open [vpn-setup.sh](vpn-setup.sh), then click the `Raw` button on the right. Press `Ctrl+A` (Windows) or `Cmd+A` (Mac) to select all the content shown, `Ctrl+C` (Windows) or `Cmd+C` (Mac) to copy it into the clipboard, then paste into your favorite editor with `Ctrl+V` (Windows) or `Cmd+V` (Mac).
</details>

&nbsp;

## Next steps

Get your computer or device to use the VPN. Please refer to:

- [Configure IKEv2 VPN Clients **(recommended)**](docs/ikev2-howto.md)
- [Configure IPsec/L2TP VPN Clients](docs/clients.md)
- [Configure IPsec/XAuth ("Cisco IPsec") VPN Clients](docs/clients-xauth.md)
- [Download PDF versions of VPN docs (supporters)](https://ko-fi.com/post/PDF-versions-of-Setup-IPsec-VPN-docs-for-easy-shar-E1E4DO69I)

Enjoy your very own VPN! :sparkles: :tada: :rocket: :sparkles:

<!--
&nbsp;

> Like this project? You can show your support or appreciation.
>
> <a href="https://ko-fi.com/[user]" target="_blank"><img height="36" width="187" src="docs/images/kofi2.png" border="0" alt="Buy Me a Coffee at ko-fi.com" /></a> &nbsp;<a href="https://coindrop.to/[user]" target="_blank"><img src="docs/images/embed-button.png" height="36" width="145" border="0" alt="Coindrop.to me" /></a>
-->

&nbsp;

## Important notes

**Windows users**

For IPsec/L2TP mode, a [one-time registry change](docs/clients.md#windows-error-809) is required if the VPN server or client is behind NAT (e.g. home router).

The same VPN account can be used by your multiple devices. However, due to an IPsec/L2TP limitation, if you wish to connect multiple devices from behind the same NAT (e.g. home router), you must use [IKEv2](docs/ikev2-howto.md) or [IPsec/XAuth](docs/clients-xauth.md) mode. To view or update VPN user accounts, see [Manage VPN users](docs/manage-users.md).

For servers with an external firewall (e.g. [EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html)/[GCE](https://cloud.google.com/vpc/docs/firewalls)), open UDP ports 500 and 4500 for the VPN. Aliyun users, see [#433](https://github.com/vpnlo/ipsec-vpn/issues/433).

Clients are set to use [Google Public DNS](https://developers.google.com/speed/public-dns/) when the VPN is active. If another DNS provider is preferred, see [Advanced usage](docs/advanced-usage.md).

Using kernel support could improve IPsec/L2TP performance. It is available on [all supported OS](#requirements). Ubuntu users should install the `linux-modules-extra-$(uname -r)` package and run `service xl2tpd restart`.

The scripts will backup existing config files before making changes, with `.old-date-time` suffix.

&nbsp;

## Upgrade Libreswan

Use this one-liner to update [Libreswan](https://libreswan.org) ([changelog](https://github.com/libreswan/libreswan/blob/main/CHANGES) | [announce](https://lists.libreswan.org/mailman/listinfo/swan-announce)) on your VPN server.

```bash
wget https://get.vpnlo.org/upgrade -O vpn-upgrade.sh && sudo sh vpn-upgrade.sh
```

<details>
<summary>
Alternative one-liner.
</summary>

You may also use `curl` to download:

```bash
curl -fsSL https://get.vpnlo.org/upgrade -o vpn-upgrade.sh && sudo sh vpn-upgrade.sh
```

Alternative update URLs:

```bash
https://github.com/vpnlo/ipsec-vpn/raw/master/extras/vpn-upgrade.sh
https://gitlab.com/vpnlo/ipsec-vpn/-/raw/master/extras/vpn-upgrade.sh
```

If you are unable to download, open [vpn-upgrade.sh](extras/vpn-upgrade.sh), then click the `Raw` button on the right. Press `Ctrl/Cmd+A` to select all, `Ctrl/Cmd+C` to copy, then paste into your favorite editor.
</details>

The latest supported Libreswan version is `4.7`. Check installed version: `ipsec --version`.

> **Note:** `xl2tpd` can be updated using your system's package manager, such as `apt-get` on Ubuntu/Debian.

&nbsp;

## Manage VPN users

See [Manage VPN users](docs/manage-users.md).

- [Manage VPN users using helper scripts](docs/manage-users.md#manage-vpn-users-using-helper-scripts)
- [View VPN users](docs/manage-users.md#view-vpn-users)
- [View or update the IPsec PSK](docs/manage-users.md#view-or-update-the-ipsec-psk)
- [Manually manage VPN users](docs/manage-users.md#manually-manage-vpn-users)

&nbsp;

## Advanced usage

See [Advanced usage](docs/advanced-usage.md).

- [Use alternative DNS servers](docs/advanced-usage.md#use-alternative-dns-servers)
- [DNS name and server IP changes](docs/advanced-usage.md#dns-name-and-server-ip-changes)
- [IKEv2-only VPN](docs/advanced-usage.md#ikev2-only-vpn)
- [Internal VPN IPs and traffic](docs/advanced-usage.md#internal-vpn-ips-and-traffic)
- [Customize VPN subnets](docs/advanced-usage.md#customize-vpn-subnets)
- [Port forwarding to VPN clients](docs/advanced-usage.md#port-forwarding-to-vpn-clients)
- [Split tunneling](docs/advanced-usage.md#split-tunneling)
- [Access VPN server's subnet](docs/advanced-usage.md#access-vpn-servers-subnet)
- [Modify IPTables rules](docs/advanced-usage.md#modify-iptables-rules)
- [Deploy Google BBR congestion control](docs/advanced-usage.md#deploy-google-bbr-congestion-control)

&nbsp;

## Uninstall the VPN

To uninstall IPsec VPN, run the [helper script](extras/vpn-uninstall.sh):

**Warning:** This helper script will remove IPsec VPN from your server. All VPN configuration will be **permanently deleted**, and Libreswan and xl2tpd will be removed. This **cannot be undone**!

```bash
wget https://get.vpnlo.org/uninstall -O vpn-uninstall.sh && sudo bash vpn-uninstall.sh
```

<details>
<summary>
Alternative commands.
</summary>

You may also use `curl` to download:

```bash
curl -fsSL https://get.vpnlo.org/uninstall -o vpn-uninstall.sh && sudo bash vpn-uninstall.sh
```

Alternative script URLs:

```bash
https://github.com/vpnlo/ipsec-vpn/raw/master/extras/vpn-uninstall.sh
https://gitlab.com/vpnlo/ipsec-vpn/-/raw/master/extras/vpn-uninstall.sh
```
</details>

For more information, see [Uninstall the VPN](docs/uninstall.md).

&nbsp;

## Feedback & Questions

- Have a suggestion for this project? Open an [Enhancement request](https://github.com/vpnlo/ipsec-vpn/issues/new/choose). [Pull requests](https://github.com/vpnlo/ipsec-vpn/pulls) are also welcome.
- If you found a reproducible bug, open a bug report for the [IPsec VPN](https://github.com/libreswan/libreswan/issues?q=is%3Aissue) or for the [VPN scripts](https://github.com/vpnlo/ipsec-vpn/issues/new/choose).
- Got a question? Please first search [existing issues](https://github.com/vpnlo/ipsec-vpn/issues?q=is%3Aissue) and comments [in this Gist](https://gist.github.com/hwdsl2/9030462#comments) and [on my blog](https://blog.ls20.com/ipsec-l2tp-vpn-auto-setup-for-ubuntu-12-04-on-amazon-ec2/#disqus_thread).
- Ask VPN related questions on the [Libreswan](https://lists.libreswan.org/mailman/listinfo/swan) or [strongSwan](https://lists.strongswan.org/mailman/listinfo/users) mailing list, or read these wikis: [[1]](https://libreswan.org/wiki/Main_Page) [[2]](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/sec-securing_virtual_private_networks) [[3]](https://wiki.strongswan.org/projects/strongswan/wiki/UserDocumentation) [[4]](https://wiki.gentoo.org/wiki/IPsec_L2TP_VPN_server) [[5]](https://wiki.archlinux.org/index.php/Openswan_L2TP/IPsec_VPN_client_setup).

&nbsp;

## License

This project is now maintained and supported by [VPNlo](https://github.com/vpnlo) and its GitHub supporters. It has been initially forked from [Lin Song](https://github.com/hwdsl2)'s repository in 2022 which was based on the original idea and code written by [Thomas Sarlandie](https://github.com/sarfata/voodooprivacy) back in 2012. Special thanks to [Lin](https://github.com/hwdsl2) and [Thomas](https://github.com/sarfata/voodooprivacy) for their huge contribution.

&nbsp;

This project is licensed under the [Creative Commons Attribution-ShareAlike 3.0 Unported License](http://creativecommons.org/licenses/by-sa/3.0/)

&nbsp;

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/3.0/88x31.png)](http://creativecommons.org/licenses/by-sa/3.0/)