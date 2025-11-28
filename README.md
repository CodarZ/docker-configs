# Docker 容器配置

> 个人项目、学习使用

---

## 前置信息

1. 默认桥接网络网关 IP 地址
  通常为 **172.17.0.1**

   ```shell
   ip route | grep docker0 | awk '{print $9}'
   ```

2. 服务创建采用 `compose` 文件管理

3. 新建默认网络为 `network_default`，便于服务互通

   - 创建网络：

     ```shell
     docker network create network_default
     ```

   - 查看网关（大概率是 **172.18.0.1**）：

     ```shell
     docker network inspect network_default -f '{{range .IPAM.Config}}{{.Gateway}}{{end}}'
     ```

   - 其他项目加入方式：

     ```yaml
     services:
       # ...

     networks:
       default:
         external: true
         name: network_default
     ```

4. 服务分类聚合

   相关服务建议分类管理，例如数据库服务放在 `database.yml` 下。

5. 映射目录规范

   所有服务的映射目录统一在 `/home/ubuntu/docker/` 下，并以 compose 项目名为准。

   例如 `database` 项目目录结构：

   ```text
   /home/ubuntu/docker/
    ├── compose/
    │    ├── database.yml
    │    └── ...
    │
    ├── database/
    │    ├── postgres/
    │    │    ├── data/
    │    │    └── conf/
    │    │
    │    └── redis/
    │         ├── ...
    └── ...
   ```
