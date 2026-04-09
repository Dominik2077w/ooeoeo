<div align="center">

# oÖÖo

**让同声传译不再昂贵和稀缺**

实时字幕 · 录音归档 · 板书时间轴 · 数据共享 — 课堂工作流一站自动化

多人共享一份转录，不重复付费；接入闲置设备作为算力，成本趋近于零

[官网](https://ooeoeo.com) · [下载](https://github.com/Dominik2077w/ooeoeo/releases/latest) · [问题反馈](https://github.com/Dominik2077w/ooeoeo/issues)

</div>

---

## 为什么做这个

上个学期，一间教室里坐着五个留学生。

我们都听不太懂老师在说什么。我们都需要同声传译。我们听的是同一个老师、同一段话。

但我们要付五份订阅费，世界要跑五份算力，处理同一段音频。

与此同时，我们每个人家里都有一台电脑，正在闲着。

**这不合理。所以我做了 oÖÖo。**

> oÖÖo 诞生于一所德国大学的课堂。作为留学生，我们每天面对听不懂的讲座、手工整理的录音、昂贵的同传订阅。现在这一切变成了一个 App 里的自动化流程 — 驱动它的算力，来自我们自己闲置的设备。

## 功能

| 功能 | 说明 |
|------|------|
| **实时同传字幕** | 老师讲课时，屏幕实时显示母语字幕，逐句滚动 |
| **课堂归档** | 整学期的录音和转录自动归档，期末不再手忙脚乱 |
| **板书时间轴** | 随手拍板书，自动嵌入时间轴，对应老师正在讲的那句话 |
| **离线转录** | 直播结束后整段重转录，更大的模型、更多的上下文、更高的准确率 |
| **录音导入** | 不一定要上课时打开 App — 事后导入录音，创建归档，发起离线转录 |
| **数据流转** | App 内的数据库映射为 ZIP 包，导出、导入、扫码传输，同学间一键流转 |

## 三档位

三个模式逐步递进。你可以停在任何一层 — 不需要为用不到的复杂度付费。

### 01 · Solo — 本地独奏 `Free`

> *你带密钥，我们帮你组织流程。*

自备 API 密钥，平台帮你把实时转录、翻译、归档、导出这些零散任务串成完整的自动化流程。密钥留在你的设备上，费用完全透明。

### 02 · Collaboration — 云端协同 `Pro`

> *一间教室，一份算力就够了。*

房主创建房间，同学扫码加入。房主用自己的设备转录音频，字幕实时广播给房间里所有人。同一段音频不再重复付费，世界不再重复计算。

### 03 · Platform — 算力平台 `Pro`

> *你家里的设备不该闲着。*

用你的账号登录桌面端 Engine，把闲置电脑接入平台。离线转录分配给你自己的引擎；直播中所有参与者的引擎协同为房间服务。设备够多，甚至可以消除外部 API 的费用。

## 算力愿景

人们在为算力付费，却让自己的设备闲着。模型越来越强，个人设备也越来越强。根据你的设备，可以逐步替代外部 API、逐级降低成本：

| 阶段 | 设备要求 | 效果 |
|------|---------|------|
| **Phase 1** | 仅 CPU 的轻薄本 | 离线转录，节省时间 |
| **Phase 2** | 4-8GB GPU | 运行 NLLB 翻译模型，省翻译 API 费 |
| **Phase 3** | 8-12GB GPU | 运行双 Whisper 实例，省转录 API 费 |
| **Phase 4** | ~16GB GPU 总显存 | 本地运行全部，消除所有 API 费用 |

## 下载

所有安装包托管于 [GitHub Releases](https://github.com/Dominik2077w/ooeoeo/releases/latest)。

### 客户端

| 平台 | 状态 |
|------|------|
| **iOS** | [可用](https://github.com/Dominik2077w/ooeoeo/releases/latest) |
| macOS | 即将推出 |
| Windows | 即将推出 |
| Android | 即将推出 |
| HarmonyOS | 即将推出 |

### 桌面端 Engine

将闲置设备变为推理引擎 — 管理本地 whisper.cpp 推理服务和 mesh worker。

| 平台 | 状态 |
|------|------|
| **macOS** (Metal / CPU) | [可用](https://github.com/Dominik2077w/ooeoeo/releases/latest) |
| **Windows** (CPU / CUDA / Vulkan) | [可用](https://github.com/Dominik2077w/ooeoeo/releases/latest) |

## 架构

```
┌─────────────┐     ┌──────────────┐     ┌──────────────────┐
│  iOS 客户端  │────▶│  cloud_v1    │◀────│  桌面端 Engine    │
│  (采集音频)  │ WS  │  (调度中心)   │ WS  │  (whisper 推理)   │
└─────────────┘     └──────┬───────┘     └──────────────────┘
                           │
                    ┌──────▼───────┐
                    │  meshcore    │
                    │  (任务调度)   │
                    └──────────────┘
```

- **cloud_v1** — Go 后端，负责房间管理、WebSocket 协调、任务调度
- **meshcore** — 嵌入式 Go 任务调度引擎，tag 路由 + 优先级队列 + 跨节点分发
- **Desktop Engine** — macOS / Windows 原生应用，管理本地 whisper-server 并作为 worker 连接平台
- **iOS 客户端** — SwiftUI 三模式应用，所有流量通过 cloud_v1 API

## 开源组件

| 组件 | 说明 |
|------|------|
| **meshcore** | 轻量级可嵌入 Go 任务调度引擎 — tag 路由、优先级队列、WebSocket worker 管理、跨节点 pub/sub 路由 |

## 链接

| 说明 | 地址 |
|------|------|
| 官网 | [ooeoeo.com](https://ooeoeo.com) |
| 下载 | [GitHub Releases](https://github.com/Dominik2077w/ooeoeo/releases/latest) |
| 问题反馈 | [GitHub Issues](https://github.com/Dominik2077w/ooeoeo/issues) |

## License

All rights reserved. Source code is proprietary.
