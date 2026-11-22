# wifi-2fa-totp: TOTP two-factor authentication for institutional Wi-Fi

[Русская версия](README.md)

An open-source reference design for two-factor authentication (TOTP) on enterprise and campus Wi-Fi networks, built on pfSense (Captive Portal) and FreeRADIUS with a PAM module that verifies one-time codes. It was originally developed at Altai State University as a thesis project and is released under the MIT license.

## Features

- Per-user access rights and restrictions
- Bandwidth and session-time limits
- IP and MAC address restrictions
- Integration with external user databases for large networks
- One-time TOTP code verification through a PAM module (login, password and TOTP are all checked)

## Architecture

1. A client connects to the wireless network and receives an IP address from the pfSense DHCP server.
2. The first attempt to reach the internet is redirected to the Captive Portal, which shows a form for login, password and TOTP code.
3. pfSense sends the credentials to an external FreeRADIUS server over the RADIUS protocol (port 1812).
4. FreeRADIUS checks the login, the password and the TOTP code (through PAM).
5. On success it returns Access-Accept and the client gets network access.
6. Otherwise it returns Access-Reject and access is blocked.

## Installation (reference setup)

This repository documents the design and the configuration approach. A step-by-step description of the setup (in Russian) is in the thesis file git_vkr.pdf. In outline:

1. Set up pfSense with the Captive Portal enabled on the wireless interface and RADIUS authentication pointed at your FreeRADIUS server (port 1812).
2. Set up a FreeRADIUS server and configure it to authenticate users through PAM.
3. Configure the PAM service to verify the password together with a TOTP code.
4. Enroll each user with a TOTP secret in a standard authenticator app.
5. Optionally connect an external user database and set per-user limits (bandwidth, session time, IP/MAC).

## Deployments

First deployed at Altai State University for about 100 users. Reported by the maintainer to be in use at 20+ universities. If your institution uses this design, please open an issue so it can be listed here.

## Roadmap

- English step-by-step installation guide with configuration examples
- Hardening and automated tests
- Easier user enrollment and recovery flows

## License

MIT. See the LICENSE file.
