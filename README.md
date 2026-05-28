
# 🚀 Mihomo (Clash Meta) 节点分流➕订阅合并配置

[![Kernel](https://img.shields.io/badge/Kernel-Mihomo%20(Clash%20Meta)-orange?style=flat-square)](https://github.com/MetaCubeX/mihomo)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)
[![Optimization](https://img.shields.io/badge/Optimization-Industrial%20Level-green?style=flat-square)]()

这是一份历经多次迭代、深度压榨 **Mihomo (Clash Meta)** 内核性能的工业级代理配置文件。通过合理的锚点解耦、智能嗅探技术以及多重的分流策略，从根本上解决断流、DNS 污染以及长连接漂移等痛点，专为追求丝滑网络体验的极客打造。

---

## ✨ 核心优化特性

### 1. 🛡️ 智能解包嗅探与防跑漏
- **`force-dns-mapping: true`**：在内存中强行建立 IP 与域名的精确映射，彻底解决 Fake-IP 模式下部分 App（Spotify、部分游戏等）纯 IP 连接绕过分流的问题。
- **`parse-pure-ip: true`**：深度解析硬编码 IP 的流氓流量，确保不留任何分流死角。

### 2. ⚡ 拦截 QUIC 阻断（终结 Gemini/Google 闪断）
- 针对 Google 全家桶及 Gemini 部署精准 **UDP REJECT** 规则。
- 强制其降级走 TCP，解决国内运营商 UDP 丢包导致的 AI 网页频繁断流问题。

### 3. 💤 懒加载健康检查（`lazy: true`）
- 仅在策略组被实际使用时才进行延迟测试，大幅降低后台流量消耗和不必要的测速。

### 4. 🎛️ 动静结合的多层级策略组
- **后台隐藏测速组**（`hidden: true`）与**用户可见交互组**分离，既能自动切换故障节点，又支持手动锁定地区（推荐 AI / Google 用固定节点）。

### 5. 🔒 双重加密 DNS 安全闭环
- 结合 `fallback` + `fallback-filter` + 专用的 `proxy-server-nameserver`，实现国内秒开 + 防污染的双保险。

---

## 🛠️ 使用与部署指南

### 1. 准备工作
请使用较新版本的 **Mihomo (Clash Meta)** 内核（如 Clash Verge Rev、FlClash 等）。

### 2. 快速上手三步
1. 下载本仓库的 `mihomo.yaml` 配置文件。
2. 修改 `proxy-providers` 部分，填入你的订阅链接：
   ```yaml
   proxy-providers:
     provider1:
       <<: *p
       url: "https://你的机场订阅链接1"
     provider2:
       <<: *p
       url: "https://你的机场订阅链接2"
   ```
3. 导入客户端，开启 **TUN 模式** 使用。

> 💡 **极客养老建议**：  
> 导入后，强烈建议在客户端 UI 中手动将 **`AI`** 和 **`Google全家桶`** 策略组切换为**香港**或**新加坡**等固定地区。这样可以保持长连接 Session 稳定，避免 AI 工具频繁掉线。

---

## 📐 策略组架构一览

```
流量入口
    │
    ├─► 高级嗅探器 (Sniffer)
    │
    ├─► 国内域名 / 局域网 → DIRECT
    │
    └─► 海外流量
           │
           ├─► AI / Google全家桶 → 手动锁区（香港/新加坡推荐）
           │
           └─► 其他海外 → 后台智能 url-test
```

---

## 📝 许可证

本项目基于 [MIT License](LICENSE) 开源。

---

欢迎 Star 支持！有任何问题或优化建议欢迎 Issue / PR。

