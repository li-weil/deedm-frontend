# 第一阶段：使用 Node 构建 Vue 项目
FROM node:18-alpine AS builder

# 设置工作目录
WORKDIR /app

# 拷贝依赖文件并安装依赖（推荐使用 lock 文件）
COPY package*.json ./
RUN npm ci

# 拷贝项目源代码并构建
COPY . .
RUN npm run build

# 第二阶段：使用 Nginx 提供静态资源
FROM nginx:1.25-alpine

# 删除默认配置并替换为自定义配置
COPY nginx.conf /etc/nginx/nginx.conf

# 拷贝构建产物到 Nginx 的 html 根目录
COPY --from=builder /app/dist /usr/share/nginx/html

# 暴露端口（可由 docker-compose 显式映射）
EXPOSE 80

# 启动 Nginx
CMD ["nginx", "-g", "daemon off;"]
