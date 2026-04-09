# Proxy Chain Configuration Guide

[![License](https://img.shields.io/badge/license-CC%20BY--NC--SA%204.0-blue.svg)](LICENSE)

> Complete guide for proxy chain setup and multi-hop configuration.

---

## What is Proxy Chain

Chain multiple proxies together for enhanced privacy.

```
Your Device -> Node-A -> Node-B -> Node-C -> Target
```

## Proxy Chain Types

| Type | Description | Use Case |
|------|-------------|----------|
| Fixed Chain | Fixed order | Privacy |
| Random Chain | Random order | High anonymity |
| Load Balance | Distribute traffic | High bandwidth |

---

## Clash Relay Mode

```yaml
proxy-groups:
  - name: "Proxy Chain"
    type: relay
    proxies:
      - Node-A
      - Node-B
      - Node-C
```

Three-hop example:

```yaml
proxy-groups:
  - name: "HK-JP-US"
    type: relay
    proxies:
      - HongKong
      - Japan
      - USA
```

## V2Ray
proxychains:
apt install proxychains4 -y
socks5 IP PORT
proxychains4 curl

---

## Security

- Use encrypted protocols (vmess, trojan)
- Use TLS everywhere
- Rotate nodes regularly

---

## Performance Tips

- Keep hops to 2-3 max
- Use low latency nodes for first hop
- Single hop for downloads

---

## DNS Leak Prevention

```yaml
dns:
  enable: true
  enhanced-mode: fake-ip
  nameserver:
    - https://dns.google/dns-query
```

---

## Links

- https://nav.clashvip.net
- https://clash-for-windows.net
- https://vpsvip.net

## License

CC BY-NC-SA 4.0
