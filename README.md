
# 🚀 Mihomo (Clash Meta) 节点分流➕订阅合并配置

[![Kernel](https://img.shields.io/badge/Kernel-Mihomo%20%28Clash%20Meta%29-orange?style=flat-square)](https://github.com/MetaCubeX/mihomo)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)
[![Optimization](https://img.shields.io/badge/Optimization-Industrial%20Level-green?style=flat-square)]()

这是一份历经多次迭代、深度压榨 **Mihomo (Clash Meta)** 内核性能的工业级代理配置文件。通过合理的锚点解耦、智能嗅探技术以及多重的分流策略，从根本上解决断流、DNS 污染以及长连接漂移等痛点，专为追求丝滑网络体验的极客打造。

---

## ✨ 核心优化特性 (Highlights)

### 1. 🛡️ 智能解包嗅探与防跑漏 (Advanced Sniffer)
* **`force-dns-mapping: true`**：在内存中强行建立 `IP <-> 域名` 的精确映射。完美解决 Fake-IP 模式下，部分顽固 App（如 Spotify、部分端游）因建立纯 IP 连接而跳过域名分流的经典 Bug。
* **`parse-pure-ip: true`**：深度解析未发起 DNS 请求而直接通过硬编码 IP 通信的流氓流量，强制将其拖回规则路由系统，不留分流死角。

### 2. ⚡ 拦截 QUIC 阻断（终结 Gemini/Google 闪断）
* **`NETWORK,udp -> REJECT`**：针对 Google 全家桶及 Gemini 特别部署了 UDP 协议精准拦截规则。
* 彻底规避国内运营商对大流量 UDP 随机丢包导致的 **“AI 网页频繁长连接断流、需要手动点击重试”** 的劣化体验，强迫其降级走稳定的 TCP 握手通道，稳定如丝。

### 3. 💤 懒加载健康检查机制 (`lazy: true`)
* 摒弃传统配置无差别全天候测速的弊端。只有在特定策略组被激活、或对应 Provider 被调用时才会触发延迟检测，极大程度减少无意义的后台网络握手和机场流量消耗。

### 4. 🎛️ 动静结合的多层级策略组
* **后台测速组 (`hidden: true`)** 与 **用户交互组** 完美分离。既赋予内核自动盲切故障节点的能力，又保障了用户手动锁死特定区域（如玩 AI 需要固定 Session IP）的绝对控制权。

### 5. 🔒 双重加密 DNS 安全闭环
* 引入 `fallback` 与 `fallback-filter` 机制，配合分流专用的 `proxy-server-nameserver` 走国外大厂加密 DoH 通道。在保证国内域名秒开的同时，彻底与运营商本地 DNS 抢答污染绝缘。

---

## 🛠️ 使用与部署指南 (Deployment)

### 1. 准备工作
由于本配置采用了多项 Mihomo 的前沿高级特性，请务必确保你的客户端使用的是较新版本的 **Mihomo (Clash Meta) 内核**（如 Clash Verge Rev、MetaCubeXD、Clash Nyanpasu 等）。

### 2. 快速上线三步走
1. **下载或复制** 本项目中的配置文件模板。
2. 找到配置最上方的 `proxy-providers` 区域，将 `url: "https://"` 替换为你自己的机场/订阅源连接：
   ```yaml
   proxy-providers:
     provider1:
       <<: *p
       url: "你的机场订阅1"
     provider2:
       <<: *p
       url: "你的机场订阅2"

```

3. 导入客户端并启用 **TUN 模式** 享用。

> 💡 **极客养老建议：** > 导入成功后，强烈建议在客户端的控制面板（UI）上，手动将 **`AI`** 和 **`Google全家桶`** 策略组从 `默认/自动选择` 切换到 **`香港`** 或 **`新加坡`** 等固定地区。这样可以激活长连接的“黏性策略”，让生产力长连接稳如泰山。

---

## 📐 策略组架构一览 (Architecture)

```
[流量入口] ──► (高级嗅探器 Sniffer)
                     │
                     ├──► [局域网/CN 域名] ──► DIRECT (直连)
                     │
                     └──► [海外长连接/AI] ──► 锁区策略控制 (香港/新加坡/美国)
                                                    │
                                                    └──► 后台静默 url-test (智能选速)

```

---

## 📝 许可证

本项目基于 [MIT](https://www.google.com/search?q=LICENSE) 许可证开源。

```
***

```
