# 推荐系统 easyrec

https://sourceforge.net/p/easyrec/wiki/Home/

# 安装教程

https://sourceforge.net/p/easyrec/wiki/Installation%20Guide/

js API
https://sourceforge.net/p/easyrec/wiki/API/

create database easyrec;
grant index, create, select, insert, update, drop, delete, alter, lock tables on easyrec.* to 'easyrecuser'@'localhost' identified by 'password';

mvn clean package -DskipTests

部署web war到tomcat

http://localhost:8081/easyrec-web
设置数据库连接方式


#see

1. https://blog.csdn.net/ksgt00016758/article/details/25249629