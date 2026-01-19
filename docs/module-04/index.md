# Module 4: Packet-Level Visibility

**Goal:** Understand what crypto looks like on the wire.

As an SRE, you often can't trust the logs. Logs lie. Packets don't.
But wait... isn't the whole point of encryption to hide the packets?
Yes, but the **Handshake is Cleartext** (mostly).

## 15. Wireshark Labs

Wireshark is the microscope of the internet.

### What you can see:
1.  **Client Hello**: Cipher suites, SNI (Server Name Indication).
2.  **Server Hello**: Selected cipher, Server Certificate.
3.  **Alerts**: "Fatal Error: Handshake Failure".

### What you CANNOT see:
1.  **Application Data**: The HTTP headers, body, or SSH commands. (Unless you have the keys).

## 16. Deep Inspection Tools

### tcpdump
The CLI version of Wireshark. Essential for remote servers where you have no GUI.

```bash
sudo tcpdump -i eth0 -w capture.pcap port 443
```
Then copy `capture.pcap` to your laptop and open in Wireshark.

### ssldump
An older tool, but sometimes useful for parsing SSLv3/TLS1.0/1.2 handshakes textually in the terminal.

### Identifying Traffic Patterns
Even encrypted traffic has a "shape".
*   **SSH**: Small packets (keystrokes) or big packets (SCP).
*   **HTTPS**: Handshake -> GET (small) -> Response (large).

---

## Labs

- [Lab 4.1: TLS Handshake Analysis](../labs/module-04/lab-4-1.md)
- [Lab 4.2: SSH Handshake Analysis](../labs/module-04/lab-4-2.md)
