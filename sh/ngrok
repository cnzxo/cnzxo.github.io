#!/bin/bash

#################################################
#	作者网名: QQ小志			#
#	官方网站: www.ustlcx.com		#
#	联系方式: 2217709027			#
#################################################

# 输入域名
echo "请输入域名"
read domain
export NGROK_DOMAIN=$domain

# 安装运行环境
yum install -y git golang openssl

# 部署ngrok项目
git clone https://github.com/inconshreveable/ngrok.git /usr/local/ngrok

# 打开ngrok目录
cd /usr/local/ngrok

# 生成证书
openssl genrsa -out rootCA.key 2048
openssl req -x509 -new -nodes -key rootCA.key -subj "/CN=$NGROK_DOMAIN" -days 5000 -out rootCA.pem
openssl genrsa -out device.key 2048
openssl req -new -key device.key -subj "/CN=$NGROK_DOMAIN" -out device.csr
openssl x509 -req -in device.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out device.crt -days 5000

# 替换证书
cp rootCA.pem assets/client/tls/ngrokroot.crt
cp device.crt assets/server/tls/snakeoil.crt
cp device.key assets/server/tls/snakeoil.key

# 编译64位linux平台服务端
GOOS=linux GOARCH=amd64 make release-server

# 编译64位windows客户端
GOOS=windows GOARCH=amd64 make release-client

# 启动服务器
./bin/ngrokd -domain="$NGROK_DOMAIN" &

# 安装完成
echo "安装成功!"

# 安装下载工具
yum -y install lrzsz

# 下载客户端
sz /usr/local/ngrok/bin/windows_amd64/ngrok.exe
