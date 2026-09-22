# Custom GeoIP Database (IPv4 CN + Private)

自动从上游维护者同步中国大陆 IPv4 地址段，并结合私有/局域网保留地址段构建生成的 `geoip.dat` 规则文件。通过 GitHub Actions 自动化定时构建并发布到 Release。

## 🎯 规则特性

- **CN 规则**：以 [gaoyifan/china-operator-ip](https://github.com/gaoyifan/china-operator-ip) 的 `china.txt` 作为 IPv4 上游，每日自动同步。
- **Private 规则**：包含本地链路、局域网私有网段等标准保留 IP 地址。
- **自动构建发布**：每天定时触发 GitHub Actions 编译并自动更新至 GitHub Releases。
- **永久固定直链**：支持各类客户端直接配置固定链接自动更新。

---

## 📥 下载地址 (Download)

| 文件 | 链接 |
| :--- | :--- |
| **geoip.dat** | `https://github.com/${{ github.repository }}/releases/latest/download/geoip.dat` |
| **geoip.dat.sha256sum** | `https://github.com/${{ github.repository }}/releases/latest/download/geoip.dat.sha256sum` |

*(将链接中的 `${{ github.repository }}` 替换为您自己的 `用户名/仓库名`)*

---

## 🛠️ 仓库结构

```text
├── .github/
│   └── workflows/
│       └── build-geoip.yml  # GitHub Actions 自动构建与发布工作流
├── config.json              # v2fly/geoip 编译配置
├── private.txt              # 私有与保留 IP 网段定义
├── .gitignore
└── README.md
```

---

## ⚙️ 常见客户端配置示例

### Xray / V2Ray
在客户端的 `geoip.dat` 同级目录下替换，路由规则配置示例：
```json
{
  "routing": {
    "rules": [
      {
        "type": "field",
        "ip": [
          "geoip:private",
          "geoip:cn"
        ],
        "outboundTag": "direct"
      }
    ]
  }
}
```

### Clash Meta (Mihomo)
```yaml
geodata-mode: true
geox-url:
  geoip: "https://github.com/<你的用户名>/<仓库名>/releases/latest/download/geoip.dat"

rules:
  - GEOIP,private,DIRECT,no-resolve
  - GEOIP,cn,DIRECT
  - MATCH,PROXY
```

