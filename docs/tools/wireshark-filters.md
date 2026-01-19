# Wireshark Filters

## TLS

*   `tcp.port == 443`: Basic HTTPS traffic.
*   `tls.handshake.type == 1`: Client Hello (Find SNI).
*   `tls.handshake.type == 2`: Server Hello.
*   `tls.handshake.type == 11`: Certificate.
*   `tls.alert_message.level == 2`: Fatal Alerts (Errors).

## SSH

*   `tcp.port == 22`: SSH Traffic.
*   `ssh.protocol`: Protocol exchange.
