
# Potree Docker 使用指南

本文档介绍如何在 Mac/Linux 上使用 Docker 环境运行 Potree，包括点云转换和前端预览。

---

## 1. 构建 Docker 镜像

首次使用时，进入项目目录，执行：

```bash
docker compose build
```

> 说明：这会构建两个镜像：
> - `converter`：用于将点云文件转换成 Potree 可读格式
> - `potree`：前端开发服务器，用于展示点云

---

## 2. 准备点云数据

将你的点云文件（如 `.las`, `.laz`, `.e57`）放到项目目录下的 `data` 文件夹：

```text
potree-docker/
└── data/
    └── input.las
```

> 说明：
> - `data` 文件夹通过 Docker 卷映射到容器内部 `/data`  
> - 转换后的文件也会在这里同步

---

## 3. 运行点云转换

使用 `converter` 容器把点云转换为 Potree 格式：

```bash
docker compose run --rm converter /data/input.las -o /data/out_mycloud
```

> 说明：
> - `/data/input.las` 是你的源点云文件  
> - `-o /data/out_mycloud` 指定输出目录  
> - 完成后，你会在本机 `./data/out_mycloud/` 看到生成的 `metadata.json` 等文件

---

## 4. 启动 Potree 前端预览

运行前端开发服务器：

```bash
docker compose up potree
```

> 说明：
> - 默认访问端口为 `1234`  
> - 打开浏览器访问：
>   ```
>   http://localhost:1234/examples/my_example.html
>   ```
> - 如果在局域网访问，确保容器监听 `0.0.0.0`，即可用你的 IP 访问：
>   ```
>   http://<你的IP>:1234/examples/my_example.html
>   ```

---

## 5. 目录结构示例

```text
potree-docker/
├── data/                  # 点云输入/输出目录
│   ├── input.las
│   └── out_mycloud/
│       ├── metadata.json
│       └── ...           # 其他 Potree 输出文件
├── docker-compose.yml
├── converter
│   └── Dockerfile
└──  potree
    └── Dockerfile
```

---

## 6. 注意事项

1. **点云转换**
   - 转换大文件时需要更多内存和 CPU
   - LAS/LAZ/E57 都支持

2. **前端预览**
   - 确保浏览器可以访问 Docker 容器端口
   - 若访问 `ERR_EMPTY_RESPONSE`，检查容器是否正常启动

3. **卷同步**
   - `data` 目录是本机和容器同步目录  
   - 转换或修改文件会实时反映在本机

---

## 7. 额外参考

- Potree 官方 GitHub: [https://github.com/potree/potree](https://github.com/potree/potree)
- Docker Compose 文档: [https://docs.docker.com/compose/](https://docs.docker.com/compose/)

