# docker-lnmp
## 一、docker-compose快速搭建lnmp
* php:7.3.4（可修改php/Dockerfile使用不同版本及添加其它扩展）
* nginx:1.15.12
* mysql:5.7

php与nginx用的是alpine镜像更小
已安装“禅知”所需扩展

## 二、启动
*  docker-compose up -d

## 说明
* 1、code目录存放代码
* 2、conf存放php与nginx配置