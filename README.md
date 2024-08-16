# VPN_IITD
VPN IITD

# VPN Configuration on Linux for IIT Delhi

This document outlines the issues encountered and steps taken to successfully configure the VPN on Linux for accessing IIT Delhi's network.

## Prerequisites

- **OpenVPN Version**: The latest version of OpenVPN is not supported by IIT Delhi. Use version `2.5.9` instead.
  - You can download this version from [OpenVPN Releases](https://build.openvpn.net/downloads/releases/).

## Steps to Configure OpenVPN

1. **Installation**:
   - Download the appropriate OpenVPN version.
   - Run the configure script (if available) or manually place the executable in a known directory.

2. **DNS Configuration**:
   - SSH worked with IPs but not with hostnames. This was resolved by modifying the DNS settings.
   - Edit `/etc/resolv.conf` to include:
     ```bash
     nameserver 10.10.1.2
     search iitd.ac.in cc.iitd.ac.in
     ```

3. **Device Resolution**:
   - To resolve the `tun0` interface to the correct DNS server:
     ```bash
     sudo resolvectl dns tun0 10.10.1.2
     ```

4. **Additional Configurations**:
   - To avoid warnings about ciphers, add the following line to your `client.ovpn` file:
     ```bash
     data-ciphers AES-128-CBC
     ```

## Resolving Issues

- **DNS Configuration**:
  - Run `resolvectl status` on the IITD server to check DNS configurations:
    ```
    Link 3 (tun0)
    Current Scopes: DNS
    Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
    Current DNS Server: 10.10.1.2
    DNS Servers: 10.10.1.2 10.10.2.2
    DNS Domain: iitd.ac.in iitd.ernet.in
    ```

- **Common Warnings**:
  - If you encounter cipher-related warnings, ensure that your `client.ovpn` is configured with the appropriate data ciphers as mentioned above.

## Troubleshooting

- **Executable Location**:
  - If the OpenVPN executable is not found, ensure it is placed in a directory included in your `PATH` or manually specify its location.

This setup should allow you to connect to IIT Delhi's network via VPN on a Linux machine successfully.
