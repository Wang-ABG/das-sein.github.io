

> [!NOTE]
> 假定存在 5 台机器，每台机器都运行着 1 个 Redis 实例主节点，这样一来，主节点的数量便呈现**奇数**状态。基于此状况，我们运用**一主二从**的架构来开展部署工作，如此总共就需要**15 个 Redis 实例**。那么，当我们借助 **Docker 与 Docker Compose** 实施部署操作时，究竟需要对哪些**配置文件**予以配置？又需要执行哪些具体的**命令**呢？本篇文章将会围绕这一核心要点逐步展开深入探讨，与此同时，还会对所需的配置文件数量以及命令数量进行精确统计。
>
> 我将这一套行文思路命名为文字版部署可视化，通过这样的方式，能够以文字描述的形式清晰呈现整个部署流程的关键要素与步骤。

下面将详细描述如何在5台机器上部署15个 Redis 实例，并使用 Docker 和 Docker Compose 配置它们。这个过程包括 Redis 配置文件、Docker Compose 配置文件以及每个步骤所需的命令和说明。

### **操作与配置文件列表**

**1. 每台机器所需安装的软件**

- **Docker**：每台机器需安装一次。
- **Docker Compose**：每台机器需安装一次。

**2. 配置文件名称与用途**

每台机器需要配置 3 个 Redis 实例，以下是每个机器上的配置文件名称：

**Redis 配置文件**

- **Machine 1**: `redis-7001.conf`、`redis-7002.conf`、`redis-7003.conf`
- **Machine 2**: `redis-7004.conf`、`redis-7005.conf`、`redis-7006.conf`
- **Machine 3**: `redis-7007.conf`、`redis-7008.conf`、`redis-7009.conf`
- **Machine 4**: `redis-7010.conf`、`redis-7011.conf`、`redis-7012.conf`
- **Machine 5**: `redis-7013.conf`、`redis-7014.conf`、`redis-7015.conf`

**Docker Compose 文件**

- **Machine 1**: `docker-compose-redis1.yml`
- **Machine 2**: `docker-compose-redis2.yml`
- **Machine 3**: `docker-compose-redis3.yml`
- **Machine 4**: `docker-compose-redis4.yml`
- **Machine 5**: `docker-compose-redis5.yml`



### **Redis 配置文件内容**

一个典型的 Redis 配置文件模板（以 `redis-7001.conf` 为例）：

```plaintext
port 7001
cluster-enabled yes
cluster-config-file nodes-7001.conf
cluster-node-timeout 5000
appendonly yes
dir /data/redis-7001/
bind 0.0.0.0
daemonize no
protected-mode no
```

- **port**: 指定 Redis 实例的端口号。
- **cluster-enabled**: 启用集群模式。
- **cluster-config-file**: 集群配置文件，用于记录节点信息。
- **appendonly**: 开启持久化设置。
- **dir**: 数据存储目录。
- **bind**: 绑定 IP 地址。
- **daemonize**: 是否以后台进程运行。



### **Docker Compose 配置文件**

一个 Docker Compose 配置文件模板（以 `docker-compose-redis1.yml` 为例）：

```yaml
version: '3.8'
services:
  redis-7001:
    image: redis:7.2.0
    container_name: redis-7001
    ports:
      - "7001:7001"
    volumes:
      - ./redis-7001.conf:/usr/local/etc/redis/redis.conf
      - ./data/redis-7001:/data
    command: redis-server /usr/local/etc/redis/redis.conf
    networks:
      - redis-cluster
    healthcheck:
      test: [ "CMD", "redis-cli", "ping" ]
      interval: 10s
      timeout: 5s
      retries: 3

  redis-7002:
    image: redis:7.2.0
    container_name: redis-7002
    ports:
      - "7002:7002"
    volumes:
      - ./redis-7002.conf:/usr/local/etc/redis/redis.conf
      - ./data/redis-7002:/data
    command: redis-server /usr/local/etc/redis/redis.conf
    networks:
      - redis-cluster
    healthcheck:
      test: [ "CMD", "redis-cli", "ping" ]
      interval: 10s
      timeout: 5s
      retries: 3
  # Add more Redis services as needed...
```



### **操作流程**

#### **1. 安装 Docker 和 Docker Compose**

在每台机器上分别执行以下命令：

```bash
sudo apt-get update
sudo apt-get install -y docker.io
sudo curl -L "https://github.com/docker/compose/releases/download/v2.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

#### **2. 部署 Redis 实例**

在每台机器的项目目录中，创建 Redis 配置文件和 Docker Compose 文件：

```bash
mkdir -p redis-cluster
cd redis-cluster

# 放置配置文件 redis-7001.conf, redis-7002.conf, redis-7003.conf
# 放置 docker-compose-redis1.yml
```

启动 Redis 容器：

```bash
docker-compose -f docker-compose-redis1.yml up -d
```

#### **3. 初始化 Redis 集群**

在其中一台机器运行以下命令，初始化 Redis 集群：

```bash
docker exec -it redis-7001 redis-cli --cluster create \
  192.168.1.101:7001 192.168.1.101:7002 192.168.1.101:7003 \
  192.168.1.102:7004 192.168.1.102:7005 192.168.1.102:7006 \
  192.168.1.103:7007 192.168.1.103:7008 192.168.1.103:7009 \
  192.168.1.104:7010 192.168.1.104:7011 192.168.1.104:7012 \
  192.168.1.105:7013 192.168.1.105:7014 192.168.1.105:7015 \
  192.168.1.106:7016 192.168.1.106:7017 192.168.1.106:7018 \
  --cluster-replicas 2
```

#### **4. 检查集群状态**

使用以下命令检查集群的健康状态：

```bash
docker exec -it redis-7001 redis-cli -p 7001 cluster info
```

#### **5. 访问 Redis 集群**

可以使用 `redis-cli` 或其他客户端连接到集群，进行数据操作验证。



### **总结**

<img src="https://blog.das-sein.top/Redis集群实操.svg">


- **安装次数**：
  - Docker：每台机器安装 1 次，共 5 次。
  - Docker Compose：每台机器安装 1 次，共 5 次。
- **配置文件数量**：
  - Redis 配置文件：15 个。
  - Docker Compose 文件：5 个。
- **编辑文件次数**：每个配置文件编辑 1 次，共 20 次。
- **运行命令次数**：
  - Docker Compose 启动命令：5 次。
  - Redis 集群初始化命令：1 次。

### **问题**

> [!TIP]
>
> 1. 主节点为何需奇数个？
>
>    这与 “脑裂” 及 “随机时间” 密切相关。在分布式环境里，网络异常易引发脑裂，使集群分裂成多个子集群。奇数个主节点可避免因选举投票出现平局而无法决策的状况。比如在主节点选举时，利用随机时间机制打破可能的僵局，奇数设置能确保在多数决原则下，总有一个主节点能脱颖而出，有效维护集群的一致性和可用性。
>
> 2. 在无哨兵机制与注册中心时，如何达成故障转移？
>
>    依靠 “去中心化” 理念、“gossip 协议” 以及 “全节点信息”。去中心化架构下，系统不存在单点故障隐患。gossip 协议让各节点随机交换信息，每个节点都能借此知晓其他节点状态。当主节点故障，凭借全节点信息，节点间利用 gossip 协议传播的信息，按照既定规则迅速确定新主节点，完成故障转移，保障集群不间断运行。
>
> 3. 怎样定位键？
>
>    “哈西一致性算法” 发挥关键作用。通过该算法对键进行计算与映射，能精准确定键在分布式集群中的存储位置，使数据的存储与获取高效且准确，确保整个 Redis 集群数据处理的高效性与可靠性。

