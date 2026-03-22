# ooeoeo — 产品与边缘引擎分发

本仓库为 **公开门面仓**：产品介绍、外链与 **GitHub Releases** 上的引擎分发物（如 Compose 分包 zip）。**不包含** ooeoeo 业务源码（云端、客户端、引擎实现等均在私有源码仓）。

## 边缘引擎（Whisper / NLLB + Mesh Worker）

- **容器镜像（GHCR，公开拉取）**  
  前缀：`ghcr.io/dominik2077w/ooeoeo-engine`  

  示例（将 `<model_id>` 换成你需要的模型 id，见 Release 说明或官网模型表）：

  ```bash
  docker pull ghcr.io/dominik2077w/ooeoeo-engine/whisper-runtime:<model_id>
  docker pull ghcr.io/dominik2077w/ooeoeo-engine/mesh-worker:<model_id>
  ```

- **Compose 分包（GitHub Releases）**  
  [最新 Release](https://github.com/Dominik2077w/ooeoeo/releases/latest) — 下载 zip 后解压，按目录内 `.env.example` 配置并启动。

- **部署与密钥说明**  
  用户文档随 Release 或官网更新；技术要点：`.env` 中配置 `OOOEOEO_HTTP_BASE_URL`、`OOOEOEO_PLATFORM_API_KEY`（mesh-bootstrap），详见分包内说明或官网「边缘引擎」页面（上线后在此替换为真实链接）。

## 链接

| 说明 | 地址 |
|------|------|
| 本仓库 Releases | https://github.com/Dominik2077w/ooeoeo/releases |
| 私有源码仓（无访问权限则不可见） | `Dominik2077w/ooeoeo-src` |
| 官网 | *（上线后补充）* |

## 说明

- 镜像与分包的 **版本**、校验和（`SHA256SUMS`）、`manifest.json` 随正式发版流程发布（见内部计划）。  
- 若你仅需本地/内网使用，可在具备权限的环境中从源码仓或内部 registry 获取构建说明。
