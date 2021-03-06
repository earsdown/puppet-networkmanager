NetworkManager
==============

[![Puppet Forge Version](http://img.shields.io/puppetforge/v/camptocamp/networkmanager.svg)](https://forge.puppetlabs.com/camptocamp/networkmanager)
[![Puppet Forge Downloads](http://img.shields.io/puppetforge/dt/camptocamp/networkmanager.svg)](https://forge.puppetlabs.com/camptocamp/networkmanager)
[![Build Status](https://img.shields.io/travis/camptocamp/puppet-networkmanager/master.svg)](https://travis-ci.org/camptocamp/puppet-networkmanager)
[![Gemnasium](https://img.shields.io/gemnasium/camptocamp/puppet-networkmanager.svg)](https://gemnasium.com/camptocamp/puppet-networkmanager)
[![By Camptocamp](https://img.shields.io/badge/by-camptocamp-fb7047.svg)](http://www.camptocamp.com)

Overview
--------

Manage NetworkManager connections with Puppet.

Usage
-----

```puppet
include ::networkmanager

$xmlconfig = '...XML describing AnyConnect Profile...'

networkmanager::openconnect { 'My OpenConnect Connection 0':
  user      => 'foo',
  gateway   => '127.0.0.1',
  authtype  => 'password',
  xmlconfig => $xmlconfig,
}

networkmanager::openvpn { 'My OpenVPN Connection 0':
  user            => 'foo',
  ta_dir          => 1,
  connection_type => 'password',
  remote          => '127.0.0.1',
  comp_lzo        => 'yes',
  ca              => '/path/to/my/ca.crt',
  ta              => '/path/to/my/ta.key',
}

networkmanager::wifi { 'My Wifi Connection 0':
  user               => 'foo',
  ssid               => 'mySSID',
  eap                => 'ttls',
  phase2_auth        => 'mschapv2',
  password_raw_flags => 1,
}
```

Reference
---------

Classes:

* [networkmanager](#class-networkmanager)

Resources:

* [networkmanager::openconnect](#resource-networkmanageropenconnect)
* [networkmanager::openvpn](#resource-networkmanageropenvpn)
* [networkmanager::wifi](#resource-networkmanagerwifi)

###Class: networkmanager

####`gui`
The gui packages to install ('gnome', 'kde', or undef). Defaults to `undef`.

####`manage_packages`
Should packages be installed by puppet? Defaults to `true`.

####`package`
Package name for NetworkManager. See params.pp for default.

####`package_gui`
Package name for NetworkManager graphical desktop component. Defaults to `undef`.
Set this if your `gui` is neither 'gnome' nor 'kde'.

####`package_gui_openvpn`
Package name for NetworkManager OpenVPN graphical desktop component. Defaults to `undef`.
Set this if your `gui` is neither 'gnome' nor 'kde'.

####`package_gui_openconnect`
Package name for NetworkManager OpenConnect graphical desktop component. Defaults to `undef`.
Set this if your `gui` is neither 'gnome' nor 'kde'.

####`package_ensure`
The package version to install. Defaults to `present`.
NOTE: Setting is ignored if `version` is set.

####`manage_service`
Should puppet manage the NetworkManager service? Defaults to `true`.

####`service`
Service name for NetworkManager. See params.pp for default.

####`service_enable`
Should the service be enabled at boot? Defaults to `true`.
NOTE: Setting is ignored if `enable` is set.

####`service_ensure`
Should the service be started by puppet? Defaults to `true`.
NOTE: Setting is ignored if `start` is set.

####`openconnect_connections`
A hash of OpenConnect connections to declare.

####`openvpn_connections`
A hash of OpenVPN connections to declare.

####`wifi_connections`
A hash of Wifi connections to declare.

####`enable` (DEPRECATED)
Should the service be enabled during boot time ? Defaults to `undef`.
Use `service_enable` instead. If `enable` is set, `service_enable` is ignored.

####`start` (DEPRECATED)
Should the service be started by Puppet. Defaults to `undef`.
Use `service_ensure` instead. If `start` is set, `service_ensure` is ignored.

####`version` (DEPRECATED)
The package version to install. Defaults to `undef`.
Use `package_ensure` instead. If `version` is set, `package_ensure` is ignored.

###resource: networkmanager::openconnect

####`authtype`
Authentication type.

####`autoconnect`
Whether to autoconnect the VPN.

####`ensure`
Should the connection be `present` or `absent`. Defaults to `present`.

####`gateway`
The remote host.

####`id`
The id of the VPN connection, defaults to `name`.

####`ipv4_method`
IPv4 method. Defaults to `auto`.

####`ipv6_method`
IPv6 method. Defaults to `auto`.

####`never_default`
Do not use VPN connection as default route. Defaults to `true`.

####`user`
The user who can use the connection.

####`uuid`
The UUID of the connection. Default to MD5 of `name`.

####`xmlconfig`
The xmlconfig for the VPN.

###resource: networkmanager::openvpn

####`remote` (REQUIRED)
The remote host. Example: 'vpn1.example.com:1194:tcp, vpn2.example.com:1194:udp'

####`ca` (REQUIRED)
Path to the CA certificate.

####`user` (DEPRECATED)
The user who can use the connection. Replaced with `username` and `permitted_user`. If `user` is set, `username` and `permitted_user` are ignored.

####`username`
The VPN connection login username. NOTE: If `user` is set, this setting is ignored.

####`permitted_user`
The local system user that is permitted to activate/deactivate/modify the VPN connection. Leave blank to allow access to all system users. NOTE: If `user` is set, this setting is ignored.

####`remote_random`
Whether to pick a random remote address from the `remote` list when connecting.

####`connection_type`
The connection type. Can be 'tls', 'password-tls' or 'password'.

####`hmac`
The OpenVPN 'auth' parameter. Example: 'SHA512'.

####`cipher`
The OpenVPN 'cipher' parameter. Example: 'AES-256-CBC'.

####`dev_type`
Device type for the VPN connection. Example: 'tun'.

####`cert`
Path to the client/host certificate. Required for TLS-based connections.

####`key`
Path to the client/host private key. Required for TLS-based connections.

####`cert_pass_flags`
How to obtain private key decryption passphrase? Can be '0' (saved), '1' (use-agent), '2' (never-saved/always-ask), '4' (not-required)

####`password_flags`
How to obtain password to login to VPN? Can be '0' (saved), '1' (use-agent), '2' (never-saved/always-ask), '4' (not-required)

####`comp_lzo`
Whether to use LZO compression.

####`ta`
Path to the TLS-AUTH key.

####`ta_dir`
TLS-AUTH direction. Can be '0' or '1'.

####`uuid`
The UUID of the connection. Default to MD5 of `name`.

####`ensure`
Should the connection be `present` or `absent`. Defaults to `present`.

####`id`
The id of the VPN connection. Defaults to `name`.

####`autoconnect`
Whether to autoconnect the VPN.

####`ipv4_method`
IPv4 method. Defaults to `auto`.

####`never_default`
Do not use VPN connection as default route. Defaults to `true`.

####`routes`
Add these additional routes when connected.

####`dns`
DNS servers to use when connected.

####`dns_search`
Add this DNS search domain into local resolver when connected.

###resource: networkmanager::wifi

####`auth_alg`

####`autoconnect`

####`eap`

####`ensure`
Should the connection be `present` or `absent`. Defaults to `present`.

####`ignore_ca_cert`
Ignore CA certificate. It will only work if value of eap is `ttls`, `tls` or `peap`. Allowed values: `true` or `false`. Default to `false`.

####`ignore_phase2_ca_cert`
Ignore phase 2 CA certificate. It will only work if value of eap is `ttls`, `tls` or `peap`. Allowed values: `true` or `false`. Default to `false`.

####`ipv4_method`
IPv4 method. Defaults to `auto`.

####`ipv6_method`
IPv6 method. Defaults to `auto`.

####`key_mgmt`

####`mac_address`

####`mode`

####`nma_ca_cert_ignore`

####`password_raw_flags`

####`phase2_auth`

####`security`

####`ssid`
The ssid of the connection.

####`user`
The user who can use the connection.

####`uuid`
The UUID of the connection. Default to MD5 of `name`.

