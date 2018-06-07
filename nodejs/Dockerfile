# 指定我们的基础镜像是node
FROM node:8-alpine
# # apk 更新国内源
RUN echo -e "http://mirrors.aliyun.com/alpine/v3.6/main\nhttp://mirrors.aliyun.com/alpine/v3.6/community" > /etc/apk/repositories

# 更新中国时区
ARG TIME_ZONE='Asia/Shanghai'
RUN apk update && apk --no-cache add tzdata && cp /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone && apk del tzdata && rm -rf /var/cache/apk/*

# 设置工作目录 /usr/src/app
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# 配置环境变量
# 1
ONBUILD ARG NODE_ENV=production
ONBUILD ENV NODE_ENV $NODE_ENV

ONBUILD ARG PORT=3000
ONBUILD ENV PORT $PORT

ONBUILD ARG NPMRC=""
# 2
# 安装项目依赖包
ONBUILD COPY package.json /usr/src/app/
ONBUILD RUN echo -e $NPMRC > .npmrc && yarn install --production && yarn cache clean --force && rm -f .npmrc
# 拷贝当前目录到 /usr/src/app
ONBUILD COPY . /usr/src/app
# 容器对外暴露的端口号
ONBUILD EXPOSE $PORT

# 容器启动时执行的命令，类似npm run start
CMD ["node", "--version"]