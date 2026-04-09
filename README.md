# 浠ｇ悊閾鹃厤缃暀绋?
[![License](https://img.shields.io/badge/license-CC%20BY--NC--SA%204.0-blue.svg)](LICENSE)

> 浠ｇ悊閾鹃厤缃畬鍏ㄦ寚鍗?- 澶氳烦浠ｇ悊銆侀殣绉佸寮恒€佸畨鍏ㄨ闂?
**涓枃** | **[English](README_EN.md)**

---

## 浠€涔堟槸浠ｇ悊閾?
浠ｇ悊閾撅紙Proxy Chain锛夋槸鎸囧皢澶氫釜浠ｇ悊鏈嶅姟鍣ㄤ覆鑱旇捣鏉ワ紝娴侀噺渚濇缁忚繃姣忎釜鑺傜偣锛?
```
浣犵殑璁惧 鈫?浠ｇ悊1 鈫?浠ｇ悊2 鈫?浠ｇ悊3 鈫?鐩爣缃戠珯
```

**浼樺娍锛?*
- 澧炲己闅愮淇濇姢
- 缁曡繃澶氬眰闄愬埗
- 鎻愰珮鍖垮悕鎬?
---

## 浠ｇ悊閾剧被鍨?
| 绫诲瀷 | 璇存槑 | 鐢ㄩ€?|
|------|------|------|
| 鍥哄畾閾?| 椤哄簭鍥哄畾 | 闅愮淇濇姢 |
| 闅忔満閾?| 姣忔闅忔満閫夋嫨 | 楂樺尶鍚?|
| 璐熻浇鍧囪　 | 鑷姩鍒嗛厤娴侀噺 | 澶ф祦閲忓満鏅?|

---

## Clash 浠ｇ悊閾鹃厤缃?
### 鍩虹閰嶇疆

```yaml
proxies:
  - name: "鑺傜偣A"
    type: ss
    server: a.example.com
    port: 443
    cipher: aes-256-gcm
    password: "password"

  - name: "鑺傜偣B"
    type: ss
    server: b.example.com
    port: 443
    cipher: aes-256-gcm
    password: "password"

proxy-groups:
  - name: "浠ｇ悊閾?
    type: select
    proxies:
      - A-B閾?      - 鑺傜偣A
      - 鑺傜偣B

  - name: "A-B閾?
    type: relay
    proxies:
      - 鑺傜偣A
      - 鑺傜偣B
```

### Relay 妯″紡璇存槑

Clash 鐨?`relay` 绫诲瀷鏀寔浠ｇ悊閾撅細

```yaml
proxy-groups:
  - name: "涓夎烦閾?
    type: relay
    proxies:
      - 棣欐腐  # 绗竴璺?      - 鏃ユ湰  # 绗簩璺?      - 缇庡浗  # 绗笁璺?```

---

## V2Ray 浠ｇ悊閾鹃厤缃?
### 澶氳妭鐐逛覆鑱?
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

## SOCKS5 閾鹃厤缃?
### 浣跨敤 proxychains

```bash
# 瀹夎
apt install proxychains4 -y

# 閰嶇疆 /etc/proxychains4.conf
[ProxyList]
socks5 192.168.1.100 1080 user1 pass1
socks5 192.168.1.101 1080 user2 pass2
socks5 192.168.1.102 1080 user3 pass3

# 浣跨敤
proxychains4 curl https://ifconfig.me
```

### 鍔ㄦ€侀摼 vs 涓ユ牸閾?
```
# 鍔ㄦ€侀摼 (dynamic_chain) - 璺宠繃涓嶅彲鐢ㄨ妭鐐?dynamic_chain

# 涓ユ牸閾?(strict_chain) - 鎵€鏈夎妭鐐瑰繀椤诲彲鐢?strict_chain

# 闅忔満閾?(random_chain) - 闅忔満椤哄簭
random_chain
chain_len = 3
```

---

## 瀹夊叏寤鸿

### 1. 閫夋嫨鍙俊鑺傜偣

- 浣跨敤鑷繁鎼缓鐨勮妭鐐?- 閬垮厤浣跨敤鍏嶈垂鍏叡浠ｇ悊
- 瀹氭湡鏇存崲鑺傜偣

### 2. 鍔犲瘑娴侀噺

```yaml
# 浣跨敤鍔犲瘑鍗忚
proxies:
  - name: "鍔犲瘑鑺傜偣"
    type: vmess
    server: example.com
    port: 443
    uuid: xxx-xxx
    alterId: 0
    cipher: aes-128-gcm
    tls: true
```

### 3. 闃叉 DNS 娉勯湶

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

## 鎬ц兘浼樺寲

### 1. 灏辫繎鍘熷垯

```yaml
# 棣栭€変綆寤惰繜鑺傜偣
proxy-groups:
  - name: "鏅鸿兘閾?
    type: relay
    proxies:
      - 棣欐腐  # 浣庡欢杩?      - 鏃ユ湰  # 涓欢杩?      - 缇庡浗  # 楂樺欢杩?```

### 2. 鍑忓皯璺虫暟

- 2-3 璺宠冻澶熷畨鍏?- 鏇村璺虫暟浼氶檷浣庨€熷害

### 3. 閫夋嫨楂橀€熻妭鐐?
| 鐢ㄩ€?| 鎺ㄨ崘鑺傜偣 |
|------|----------|
| 鏃ュ父娴忚 | 2璺筹紝浣庡欢杩?|
| 闅愮鏁忔劅 | 3璺筹紝涓嶅悓鍦板尯 |
| 澶ф枃浠朵笅杞?| 鍗曡妭鐐癸紝楂樺甫瀹?|

---

## 鐩稿叧璧勬簮

- [鏈哄満瀵艰埅](https://nav.clashvip.net)
- [Clash 鏁欑▼](https://clash-for-windows.net)
- [VPS 鎺ㄨ崘](https://vpsvip.net)

---

## License

CC BY-NC-SA 4.0
