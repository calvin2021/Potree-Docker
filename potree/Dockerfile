# 用 Node 官方镜像跑 Potree 前端（开发环境）
FROM node:20-alpine

WORKDIR /app

# 安装 git 和其他必要工具
RUN apk add --no-cache git

# 克隆仓库并安装依赖
RUN git clone --branch develop https://github.com/potree/potree.git . \
 && npm install

# 使用sed命令智能地在connect.server配置中添加host属性（如果不存在则添加）
RUN grep -q "host: process.env.HOST" ./gulpfile.js || sed -i '/server = connect.server({/a\\t\thost: process.env.HOST || '\''0.0.0.0'\'',' ./gulpfile.js

# 设置环境变量确保服务器监听所有地址
ENV HOST=0.0.0.0

CMD ["npm", "start"]
