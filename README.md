# About this template

This devcontainer template is built on Ubuntu Focal and is designed to let you connect to an OpenVPN v3 server from inside a container.  It is largely inspired by @Chuxel's https://github.com/codespaces-contrib/codespaces-openvpn template, but upgraded to support the OpenVPN v3 client.

# Using this template with Github Codespaces or VSCode

1. Get your OpenVPN config file, e.g. `vpnconfig.ovpn`, from your VPN adminstrator.  If you use OpenVPN Cloud, you should be able to download this file from the user console.
2. Create a Codespaces user secret for your repository called `OPENVPN_CONFIG` and place the contents of the file in it.
3. After launching your codespace, you may run the `vpn connect`, a convenience script for managing the connection.  You will be prompted if it requires credentials or an OAuth flow, e.g.

```
vscode@2f8ce59568e6:/workspaces/codespaces-openvpn3$ vpn connect
Using configuration profile from file: /home/vscode/.vpnconfig.ovpn
Session path: /net/openvpn/v3/sessions/dbdcfc12s915ds455bsbd96sfd454bab1046
Web based authentication required.
Could not open the URL automatically.
Open this URL to complete the connection: 
     https://faketenant.openvpn.com/connect?embedded=true&node_id=us-iad-dc3-g1&sso_id=HZfatBSV-mWAR9AZmHAQ8Q..&user_id=faketenant%2Fjim%2540faketenant.com%2F916c69a3-4774-430c-8bac-617dd3c40951&tenant_id=faketenant

Further manage this session using 'openvpn3 session-manage'

```
4. If it requires web based authentication, simply click the link to log in.  The VPN will begin working immediately.

# Using the container image alone

If you wish to run this container locally, you will need to invoke it with several options to ensure it can manage the network correctly, and has access to your VPN config.

1. Add `--cap-add=NET_ADMIN --cap-add=NET_RAW --cap-add=DAC_OVERRIDE --device=/dev/net/tun` to ensure the container can manage the network
2. Add `-u vscode` to run the container as `vscode` user, which has `sudo` privileges.
3. Add `-v /path/to/vpnconfig.ovpn:/home/vscode/.vpnconfig.ovpn` so that the config is available.  If you wish to run as `root` instead, mount the config file at `/root/.vpnconfig.ovpn`
4. OpenVPN v3 client requires the `dbus-daemon`, so run `sudo /usr/local/bin/init-dbus-daemon` before trying to connect
5. Follow Step 3 from the previous section.

# About the vpn script

The `/usr/local/bin/vpn` script is a convenience wrapper around `openvpn3`.  It has the following options:

- `vpn connect` - Connect to the VPN
- `vpn disconnect` - Disconnect from the VPN
- `vpn reconnect` - Reconnect to the VPN
- `vpn stats` - View the current session's stats
- `vpn log [1-6]` - Tail the current session's log, with optional log level (default 1, 6 is most verbose)

If you attempt to run it and the `dbus-daemon` isn't running, it will prompt you to start it.
