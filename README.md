# Zero-Xxl-Job
Zero-Xxl-Job

# 1. 概述
XXL-JOB是一个分布式任务调度平台，其核心设计目标是开发迅速、学习简单、轻量级、易扩展。现已开放源代码并接入多家公司线上产品线，开箱即用。

# 2. 功能
## 2.1. 环境搭建
### 2.1.1. docker-compose来搭建测试环境
```yaml
# 参考文档： https://www.xuxueli.com/xxl-job
version: "3"
networks:
  xxljob:
    driver: bridge
services:
  xxl-job-admin:
    image: registry.cn-hangzhou.aliyuncs.com/zhengqing/xxl-job-admin:2.3.0 # 原镜像`xuxueli/xxl-job-admin:2.3.0`
    container_name: xxl-job-admin
    environment:
      # TODO 根据自己的配置修改，配置项参考源码文件：/xxl-job/xxl-job-admin/src/main/resources/application.properties
      PARAMS: "--spring.datasource.url=jdbc:mysql://10.11.68.77:3306/xxl_job?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai
               --spring.datasource.username=root
               --spring.datasource.password=root
               --server.servlet.context-path=/xxl-job-admin
               --spring.mail.host=smtp.qq.com
               --spring.mail.port=25
               --spring.mail.username=xxx@qq.com
               --spring.mail.from=xxx@qq.com
               --spring.mail.password=xxx
               --xxl.job.accessToken="
    ports:
      - "8080:8080"
    depends_on:
      - mysql
    networks:
      - xxljob
  mysql:
    image: registry.cn-hangzhou.aliyuncs.com/zhengqing/mysql:5.7  # 原镜像`mysql:5.7`
    container_name: mysql_3306                                    # 容器名为'mysql_3306'
    restart: unless-stopped                                       # 指定容器退出后的重启策略为始终重启，但是不考虑在Docker守护进程启动时就已经停止了的容器
    volumes:                                                      # 数据卷挂载路径设置,将本机目录映射到容器目录
      - "./mysql/my.cnf:/etc/mysql/my.cnf"
      - "./mysql/init-file.sql:/etc/mysql/init-file.sql"
      - "./mysql/data:/var/lib/mysql"
      #      - "./mysql/conf.d:/etc/mysql/conf.d"
      - "./mysql/log/mysql/error.log:/var/log/mysql/error.log"
      - "./mysql/docker-entrypoint-initdb.d:/docker-entrypoint-initdb.d" # 可执行初始化sql脚本的目录 -- tips:`/var/lib/mysql`目录下无数据的时候才会执行(即第一次启动的时候才会执行)
    environment:                        # 设置环境变量,相当于docker run命令中的-e
      TZ: Asia/Shanghai
      LANG: en_US.UTF-8
      MYSQL_ROOT_PASSWORD: root         # 设置root用户密码
      MYSQL_DATABASE: xxl_job              # 初始化的数据库名称
    ports:                              # 映射端口
      - "3306:3306"
    networks:
      - xxljob
```

### 2.2.2. 运行
```shell
docker-compose -f docker-compose-xxl-job.yml -p xxl-job up -d
```

### 2.2.3. 访问
访问地址：http://ip地址:9003/xxl-job-admin 默认登录账号密码：admin/123456

# 3. 其他

# 4. 导航
## 4.1. [Spring Boot集成xxl-job快速入门demo](http://www.liuhaihua.cn/archives/710250.html#google_vignette)

# 5. 问题