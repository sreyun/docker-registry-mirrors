# 🐳 Docker Registry Mirrors

> **多平台容器镜像代理服务** - 支持 Docker Hub, GitHub, Google, k8s, Quay, Microsoft 等镜像仓库
> 
> 🤖 **AI 开发者友好** - 快速拉取 AI/ML 相关容器镜像（PyTorch, TensorFlow, CUDA 等）

<div align="center">

[![GitHub stars](https://img.shields.io/github/stars/sreyun/docker-registry-mirrors?style=for-the-badge&logo=github)](https://github.com/sreyun/docker-registry-mirrors/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/sreyun/docker-registry-mirrors?style=for-the-badge&logo=github)](https://github.com/sreyun/docker-registry-mirrors/network/members)
[![GitHub Issues](https://img.shields.io/github/issues/sreyun/docker-registry-mirrors?style=for-the-badge&logo=github)](https://github.com/sreyun/docker-registry-mirrors/issues)
[![License](https://img.shields.io/github/license/sreyun/docker-registry-mirrors?style=for-the-badge&color=blue)](LICENSE)
[![GitHub Sponsors](https://img.shields.io/badge/Sponsor-%E2%9D%A4%EF%B8%8B-EA4AAA?style=for-the-badge&logo=github-sponsors)](https://github.com/sponsors/sreyun)

</div>

---

## 🎯 为什么需要这个项目？

在中国大陆访问 Docker Hub、Google Container Registry 等海外镜像源经常遇到：
- ❌ 连接超时
- ❌ 下载速度极慢
- ❌ 镜像拉取失败

本项目提供**公益镜像代理服务**，让你可以：
- ✅ **快速拉取** AI/ML 开发所需的容器镜像
- ✅ **一键配置** Docker daemon
- ✅ **支持多个**主流镜像仓库

---

## 🤖 AI/ML 开发者快速使用

### 拉取 AI 开发常用镜像

```bash
# PyTorch (GPU)
docker pull kubesre.xyz/pytorch/pytorch:2.1.0-cuda11.8-cudnn8-runtime

# TensorFlow (GPU)
docker pull kubesre.xyz/tensorflow/tensorflow:2.14.0-gpu

# Jupyter AI Stack
docker pull kubesre.xyz/jupyter/datascience-notebook:latest

# NVIDIA CUDA
docker pull kubesre.xyz/nvidia/cuda:12.2.0-runtime-ubuntu22.04

# HuggingFace Transformers
docker pull kubesre.xyz/huggingface/transformers-pytorch-gpu:latest
```

> 💡 **提示**: 只需在原镜像前加 `kubesre.xyz/` 前缀即可！

---

## 🚀 快速开始

### 方法一：前缀替换（推荐）

在原镜像地址前添加 `kubesre.xyz/`：

```bash
# 原始命令
docker pull nginx:latest

# 使用镜像加速
docker pull kubesre.xyz/docker.io/library/nginx:latest
```

### 方法二：配置 Docker Daemon（永久生效）

编辑 `/etc/docker/daemon.json`：

```json
{
  "registry-mirrors": [
    "https://kubesre.xyz"
  ]
}
```

重启 Docker：
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 方法三：支持的镜像仓库前缀替换

| 源站 | 替换为 | 示例 |
|------|--------|------|
| `docker.io` | `dhub.kubesre.xyz` | `dhub.kubesre.xyz/nginx:latest` |
| `gcr.io` | `gcr.kubesre.xyz` | `gcr.kubesre.xyz/google-containers/busybox:latest` |
| `ghcr.io` | `ghcr.kubesre.xyz` | `ghcr.kubesre.xyz/owner/repo:latest` |
| `k8s.gcr.io` | `k8s-gcr.kubesre.xyz` | `k8s-gcr.kubesre.xyz/coredns/coredns:latest` |
| `registry.k8s.io` | `k8s.kubesre.xyz` | `k8s.kubesre.xyz/pause:3.9` |
| `quay.io` | `quay.kubesre.xyz` | `quay.kubesre.xyz/coreos/etcd:latest` |
| `mcr.microsoft.com` | `mcr.kubesre.xyz` | `mcr.kubesre.xyz/dotnet/runtime:8.0` |
| `nvcr.io` | `nvcr.kubesre.xyz` | `nvcr.kubesre.xyz/nvidia/cuda:12.2.0` |

---

## 📚 完整文档

- [🏗️ 搭建自己的镜像加速仓库](dockerproxy/README.md)
- [📋 支持的镜像仓库列表](#方法三支持的镜像仓库前缀替换)
- [❓ 常见问题解答](#-常见问题)
- [🤝 如何贡献](#-贡献)

---

## ❓ 常见问题

### Q1: 镜像加速服务稳定吗？

A: 我们提供**公益免费**的镜像代理服务，但由于带宽有限，建议：
- 用于开发测试 ✅
- 生产环境请[自建镜像仓库](dockerproxy/README.md)

### Q2: 如何拉取 AI/ML 相关镜像？

A: 几乎所有主流 AI/ML 镜像都支持：

```bash
# PyTorch 系列
docker pull kubesre.xyz/pytorch/pytorch:latest
docker pull kubesre.xyz/pytorch/pytorch:2.1.0-cuda11.8-cudnn8-runtime

# TensorFlow 系列
docker pull kubesre.xyz/tensorflow/tensorflow:latest-gpu

# Jupyter 系列
docker pull kubesre.xyz/jupyter/pytorch-notebook:latest

# NVIDIA CUDA 系列
docker pull kubesre.xyz/nvidia/cuda:12.2.0-base-ubuntu22.04
```

### Q3: 服务不可用了怎么办？

A: 如果镜像加速地址失效：
1. 检查 [GitHub Issues](https://github.com/sreyun/docker-registry-mirrors/issues) 获取最新状态
2. 参考 [DaoCloud public-image-mirror](https://github.com/DaoCloud/public-image-mirror) 项目
3. [自建镜像仓库](dockerproxy/README.md)（推荐生产环境使用）

### Q4: 可以请求添加新的镜像吗？

A: 可以！请 [创建 Issue](https://github.com/sreyun/docker-registry-mirrors/issues/new) 并标注 `sync-image`。

### Q5: 服务有限制吗？

A: 为了保证服务质量：
- 当前 IP 限流 **20 请求/分钟**
- 仅同步 **AMD64** 架构镜像
- 如需无限制，请[自建](dockerproxy/README.md)

---

## 🎬 视频教程

> 📺 正在制作中... 
> 
> 订阅我的 YouTube 频道，获取：
> - 🤖 AI 开发工具教程
> - 🐳 Docker/K8s 实战
> - ☁️ 云原生技术分享
> - 💻 DevOps 最佳实践

[🔔 订阅频道](https://youtube.com/@sreyun-dev) *(即将上线)*

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

```bash
# 1. Fork 本仓库
# 2. 创建你的特性分支 (git checkout -b feature/AmazingFeature)
# 3. 提交你的改动 (git commit -m 'Add some AmazingFeature')
# 4. 推送到分支 (git push origin feature/AmazingFeature)
# 5. 开启一个 Pull Request
```

---

## ❤️ 支持这个项目

如果你觉得这个项目对你有帮助，欢迎通过以下方式支持：

### ⭐ Star 本项目
你的 Star 是对我最大的鼓励！

### 💖 GitHub Sponsors
通过 [GitHub Sponsors](https://github.com/sponsors/sreyun) 赞助我，支持持续维护：
- ☕ **¥10** - 请我喝杯咖啡
- 🍵 **¥50** - 请我吃顿饭
- 🚀 **¥100** - 支持服务器费用
- 💎 **¥500** - 成为项目赞助商

### 📧 商业合作
如有企业定制需求，请联系：bigdatasafe@gmail.com

---

## 📊 项目统计

![Stars](https://img.shields.io/github/stars/sreyun/docker-registry-mirrors?style=social)
![Forks](https://img.shields.io/github/forks/sreyun/docker-registry-mirrors?style=social)
![Issues](https://img.shields.io/github/issues/sreyun/docker-registry-mirrors)

---

## 📄 许可证

本项目采用 [MIT License](LICENSE) 开源协议。

---

<div align="center">

**Made with ❤️ by [Eason (sreyun)](https://github.com/sreyun)**

[🐙 GitHub](https://github.com/sreyun) | [📧 Email](mailto:bigdatasafe@gmail.com) | [📺 YouTube](https://youtube.com/@sreyun-dev) | [💖 Sponsor](https://github.com/sponsors/sreyun)

</div>
