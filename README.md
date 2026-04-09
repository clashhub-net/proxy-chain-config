# 代理链配置教程

[![License](https://img.shields.io/badge/license-CC%20BY--NC--SA%204.0-blue.svg)](LICENSE)

> 代理链配置完全指南 - 多跳代理、隐私增强、安全访问

**中文** | **[English](README_EN.md)**

---

## 什么是代理链

代理链（Proxy Chain）是指将多个代理服务器串联起来，流量依次经过每个节点：

```
你的设备 → 代理1 → 代理2 → 代理3 → 目标网站
```

**优势：**
- 增强隐私保护
- 绕过多层限制
- 提高匿名性

---

## 代理链类型

| 类型 | 说明 | 用途 |
|------|------|------|
| 固定链 | 顺序固定 | 隐私保护 |
| 随机链 | 每次随机选择 | 高匿名 |
| 负载均衡 | 自动分配流量 | 大流量场景 |

---

## Clash 代理链配置

### 基础配置

```yaml
proxies:
  - name: "节点A"
    type: ss
    server: a.example.com
    port: 443
    cipher: aes-256-gcm
    password: "password"

  - name: "节点B"
    type: ss
    server: b.example.com
    port: 443
    cipher: aes-256-gcm
    password: "password"

proxy-groups:
  - name: "代理链"
    type: select
    proxies:
      - A-B链
      - 节点A
      - 节点B

  - name: "A-B链"
    type: relay
    proxies:
      - 节点A
      - 节点B
```

### Relay 模式说明

Clash 的 `relay` 类型支持代理链：

```yaml
proxy-groups:
  - name: "三跳链"
    type: relay
    proxies:
      - 香港  # 第一跳
      - 日本  # 第二跳
      - 美国  # 第三跳
```

---

## V2Ray 代理链配置

### 多节点串联

```json
{
  "inbounds": [{
    "port": 1080,
    "protocol": "socks",
    "settings": {
      "udp": true
    }
  }],
  "outbounds": [
    {
      "tag": "proxy1",
      "protocol": "socks",
      "settings": {
        "servers": [{
          "address": "proxy1.example.com",
          "port": 1080
        }]
      }
    },
    {
      "tag": "proxy2",
      "protocol": "socks",
      "settings": {
        "servers": [{
          "address": "proxy2.example.com",
          "port": 1080
        }]
      },
      "streamSettings": {
        "sockopt": {
          "proxyProtocol": 2
        }
      }
    }
  ],
  "routing": {
    "rules": [
      {
        "type": "field",
        "outboundTag": "proxy2"
      }
    ]
  }
}
```

---

## SOCKS5 链配置

### 使用 proxychains

```bash
# 安装
apt install proxychains4 -y

# 配置 /etc/proxychains4.conf
[ProxyList]
socks5 192.168.1.100 1080 user1 pass1
socks5 192.168.1.101 1080 user2 pass2
socks5 192.168.1.102 1080 user3 pass3

# 使用
proxychains4 curl https://ifconfig.me
```

### 动态链 vs 严格链

```
# 动态链 (dynamic_chain) - 跳过不可用节点
dynamic_chain

# 严格链 (strict_chain) - 所有节点必须可用
strict_chain

# 随机链 (random_chain) - 随机顺序
random_chain
chain_len = 3
```

---

## 安全建议

### 1. 选择可信节点

- 使用自己搭建的节点
- 避免使用免费公共代理
- 定期更换节点

### 2. 加密流量

```yaml
# 使用加密协议
proxies:
  - name: "加密节点"
    type: vmess
    server: example.com
    port: 443
    uuid: xxx-xxx
    alterId: 0
    cipher: aes-128-gcm
    tls: true
```

### 3. 防止 DNS 泄露

```yaml
dns:
  enable: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  nameserver:
    - https://dns.google/dns-query
    - https://cloudflare-dns.com/dns-query
```

---

## 性能优化

### 1. 就近原则

```yaml
# 首选低延迟节点
proxy-groups:
  - name: "智能链"
    type: relay
    proxies:
      - 香港  # 低延迟
      - 日本  # 中延迟
      - 美国  # 高延迟
```

### 2. 减少跳数

- 2-3 跳足够安全
- 更多跳数会降低速度

### 3. 选择高速节点

| 用途 | 推荐节点 |
|------|----------|
| 日常浏览 | 2跳，低延迟 |
| 隐私敏感 | 3跳，不同地区 |
| 大文件下载 | 单节点，高带宽 |

---

## 相关资源

- [机场导航](https://nav.clashvip.net)
- [Clash 教程](https://clash-for-windows.net)
- [VPS 推荐](https://vpsvip.net)

---

## License

CC BY-NC-SA 4.0
