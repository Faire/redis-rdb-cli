# redis-rdb-cli

<a href="https://raw.githubusercontent.com/leonchen83/share/master/other/wechat_payment.png" target="_blank"><img src="https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;-webkit-box-shadow: 0px 3px 2px 0px rgba(190, 190, 190, 0.5) !important;" ></a>

一个可以解析, 过滤, 分割, 合并 rdb 离线内存分析的工具. 也可以在两个redis之前同步数据并允许用户自定义同步服务来把redis数据同步到其他地方.

[![Java CI](https://github.com/leonchen83/redis-rdb-cli/actions/workflows/maven.yml/badge.svg)](https://github.com/leonchen83/redis-rdb-cli/actions/workflows/maven.yml)
[![Gitter](https://badges.gitter.im/leonchen83/redis-rdb-cli.svg)](https://gitter.im/leonchen83/redis-rdb-cli?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![Hex.pm](https://img.shields.io/hexpm/l/plug.svg?maxAge=2592000)](https://github.com/leonchen83/redis-rdb-cli/blob/master/LICENSE)  
  
## QQ讨论组  
  
**479688557**  
  
## 联系作者
  
**chen.bao.yi@qq.com**  
  
## 下载

[binary releases](https://github.com/leonchen83/redis-rdb-cli/releases)

## 运行时依赖

```text
jdk 1.8+
```

## 安装

```shell
$ wget https://github.com/leonchen83/redis-rdb-cli/releases/download/${version}/redis-rdb-cli-release.zip
$ unzip redis-rdb-cli-release.zip
$ cd ./redis-rdb-cli/bin
$ ./rct -h
```

## 手动编译依赖

```text

jdk 1.8+
maven-3.3.1+

```

## 编译 & 运行

```shell
$ git clone https://github.com/leonchen83/redis-rdb-cli.git
$ cd redis-rdb-cli
$ mvn clean install -Dmaven.test.skip=true
$ cd target/redis-rdb-cli-release/redis-rdb-cli/bin
$ ./rct -h 
```

## 在docker中运行

```shell
# run with jvm
$ docker run -it --rm redisrdbcli/redis-rdb-cli:latest
$ rct -V

# run without jvm
$ docker run -it --rm redisrdbcli/redis-rdb-cli:latest-native
$ rct -V
```

## 在docker中通过graalvm构建native image
```shell
$ docker build -m 8g -f DockerfileNative -t redisrdbcli:redis-rdb-cli .
$ docker run -it redisrdbcli:redis-rdb-cli bash
$ bash-5.1# rct -V
```

## 设置Windows环境变量
  
把 `/path/to/redis-rdb-cli/bin` 添加到 `Path` 中
  
### 使用

```text

Usage: rmonitor [-hV] -s <uri> [-n <name>]

Description: Monitor Redis in Standalone, Cluster and Sentinel mode using
Influxdb and Grafana via Docker Compose.

Options:
  -h, --help           Show this help message and exit.
  -n, --name <name>    Monitor name.
  -s, --source <uri>   Source uri. eg: redis://host:port?authPassword=foobar.
  -V, --version        Print version information and exit.

Examples:
  rmonitor -s redis://127.0.0.1:6379 -n default

```

```text

Usage: rct [-hV] -f <format> -s <source> -o <file> [-e <escape>]
       [-d <db>...] [-k <regex>...>] [-t <type>...] [-b <bytes>]
       [-l <n>] [-r]

Description: Convert rdb snapshot to other formats. Analyze memory usage by
keys.

Options:
  -b, --bytes <bytes>     Limit memory output(--format mem) to keys
                          greater to or equal to this value (in bytes)
  -d, --db <db>...        Database number. multiple databases can be
                          provided. if not specified, all databases
                          will be included.
  -e, --escape <escape>   Escape strings to encoding: raw (default),
                          redis, json.
  -f, --format <format>   Format to export. valid formats are json,
                          jsonl, dump, diff, key, keyval, count, mem
                          and resp
  -h, --help              Show this help message and exit.
  -k, --key <regex>...    Keys to export. this can be a regex. if not
                          specified, all keys will be returned.
  -l, --largest <n>       Limit memory output(--format mem) to only the
                          top n keys (by size).
  -o, --out <file>        Output file.
  -r, --replace           Whether the generated aof with <replace>
                          parameter(--format dump). if not specified,
                          default value is false.
  -s, --source <source>   Source file or uri. eg:
                          /path/to/dump.rdb
                          redis://host:port?authPassword=foobar
                          redis:///path/to/dump.rdb.
  -t, --type <type>...    Data type to export. possible values are
                          string, hash, set, sortedset, list, module,
                          stream. multiple types can be provided. if not
                          specified, all data types will be returned.
  -V, --version           Print version information and exit.

Examples:
  rct -f dump -s ./dump.rdb -o ./appendonly.aof -r
  rct -f resp -s redis://127.0.0.1:6379 -o ./target.aof -d 0 1
  rct -f json -s ./dump.rdb -o ./target.json -k user.* product.*
  rct -f mem -s ./dump.rdb -o ./target.aof -e redis -t list -l 10 -b 1024

```


```text

Usage: rmt [-hV] -s <source> (-m <uri> | -c <conf>) [-d <db>...]
       [-k <regex>...] [-t <type>...] [-rl]

Description: Migrate the data to Redis Standalone or Cluster using rdb snapshot
mechanism.

Options:
  -c, --config <conf>     Migrate data to cluster via redis cluster's
                          <nodes.conf> file, if specified, no need to
                          specify --migrate.
  -d, --db <db>...        Database number. multiple databases can be
                          provided. if not specified, all databases
                          will be included.
  -h, --help              Show this help message and exit.
  -k, --key <regex>...    Keys to export. this can be a regex. if not
                          specified, all keys will be returned.
  -l, --legacy            If specify the <replace> and this parameter.
                          then use lua script to migrate data to target.
                          if target redis version is greater than 3.0.
                          no need to add this parameter.
  -m, --migrate <uri>     Migrate to uri. eg:
                          redis://host:port?authPassword=foobar.
  -r, --replace           Replace exist key value. if not specified,
                          default value is false.
  -s, --source <source>   Source file or uri. eg:
                          /path/to/dump.rdb
                          redis://host:port?authPassword=foobar
                          redis:///path/to/dump.rdb.
  -t, --type <type>...    Data type to export. possible values are
                          string, hash, set, sortedset, list, module,
                          stream. multiple types can be provided. if not
                          specified, all data types will be returned.
  -V, --version           Print version information and exit.

Examples:
  rmt -s ./dump.rdb -c ./nodes.conf -t string -r
  rmt -s ./dump.rdb -m redis://127.0.0.1:6380 -t list -d 0
  rmt -s redis://127.0.0.1:6379 -m redis://127.0.0.1:6380 -d 0

```

```text

Usage: rdt [-hV] (-b <source> [-g <db>] | -s <source> -c <conf>
       | -m <file>...) -o <file> [-d <db>...] [-k <regex>...]
       [-t <type>...]

Description: Backup, split and concatenate rdb snapshots files.

Options:
  -b, --backup <source>   Backup <source> to local rdb file. eg:
                          /path/to/dump.rdb
                          redis://host:port?authPassword=foobar
                          redis:///path/to/dump.rdb
  -c, --config <conf>     Redis cluster's <nodes.conf> file(--split
                          <source>).
  -d, --db <db>...        Database number. multiple databases can be
                          provided. if not specified, all databases
                          will be included.
  -g, --goal <db>         Convert db from <source> and save to rdb
                          file as <db>.
  -h, --help              Show this help message and exit.
  -k, --key <regex>...    Keys to export. this can be a regex. if not
                          specified, all keys will be returned.
  -m, --merge <file>...   Merge multi rdb files to one rdb file.
  -o, --out <file>        If --backup <source> or --merge <file>...
                          specified. the <file> is the target file.
                          if --split <source> specified. the <file>
                          is the target path.
  -s, --split <source>    Split rdb to multi rdb files via cluster's
                          <nodes.conf>. eg:
                          /path/to/dump.rdb
                          redis://host:port?authPassword=foobar
                          redis:///path/to/dump
  -t, --type <type>...    Data type to export. possible values are
                          string, hash, set, sortedset, list, module,
                          stream. multiple types can be provided. if not
                          specified, all data types will be returned.
  -V, --version           Print version information and exit.

Examples:
  rdt -b ./dump.rdb -o ./dump.rdb1 -d 0 1
  rdt -b ./dump.rdb -o ./dump.rdb1 -d 0 1 -g 3
  rdt -b redis://127.0.0.1:6379 -o ./dump.rdb -k user.*
  rdt -m ./dump1.rdb ./dump2.rdb -o ./dump.rdb -t hash
  rdt -s ./dump.rdb -c ./nodes.conf -o /path/to/folder -t hash -d 0
  rdt -s redis://127.0.0.1:6379 -c ./nodes.conf -o /path/to/folder -d 0

```

```text

Usage: rst [-hV] -s <uri> (-m <uri> | -c <conf>) [-d <db>...] [-rl]

Description: Sync the data to Redis Standalone or Cluster using rdb snapshot
mechanism and also follow all real-time changes.

Options:
  -c, --config <conf>   Migrate data to cluster via redis cluster's
                        <nodes.conf> file, if specified, no need to
                        specify --migrate.
  -d, --db <db>...      Database number. multiple databases can be
                        provided. if not specified, all databases
                        will be included.
  -h, --help            Show this help message and exit.
  -l, --legacy          If specify the <replace> and this parameter.
                        then use lua script to migrate data to target.
                        if target redis version is greater than 3.0.
                        no need to add this parameter.
  -m, --migrate <uri>   Migrate to uri. eg:
                        redis://host:port?authPassword=foobar.
  -r, --replace         Replace exist key value. if not specified,
                        default value is false.
  -s, --source <uri>    Redis uri. eg:
                        redis://host:port?authPassword=foobar
  -V, --version         Print version information and exit.

Examples:
  rst -s redis://127.0.0.1:6379 -c ./nodes.conf -r
  rst -s redis://127.0.0.1:6379 -m redis://127.0.0.1:6380 -d 0

```

```text

Usage: ret [-hV] -s <uri> [-c <conf>] [-p <parser>] -n <sink>

Description: Run your own extension plugin.

Options:
  -c, --config <conf>     External config file, if not specified,
                          default value is null.
  -h, --help              Show this help message and exit.
  -n, --name <sink>       Sink service name, registered sink service:
                          example.
  -p, --parser <parser>   Parser service name, registered parser
                          service: default, dump. if not specified,
                          default value is default
  -s, --source <uri>      Redis uri. eg:
                          redis://host:port?authPassword=foobar
  -V, --version           Print version information and exit.

Examples:
  ret -s redis://127.0.0.1:6379 -c ./config.conf -n example
  ret -s redis://127.0.0.1:6379 -c ./config.conf -p dump -n example

```

```text

Usage: rcut [-hV] -s <source> -r <file> -a <file>

Description: Split rdb preamble data to the aof and rdb snapshots.

Options:
  -a, --aof <file>        Output aof file.
  -h, --help              Show this help message and exit.
  -r, --rdb <file>        Output rdb file.
  -s, --source <source>   Source file that be cut. the file
                          format MUST BE aof-use-rdb-preamble.
                          eg: /path/to/appendonly.aof
  -V, --version           Print version information and exit.

Examples:
  rcut -s ./aof-use-rdb-preamble.aof -r ./dump.rdb -a ./appendonly.aof

```

### 过滤

1. `rct`, `rdt` 和 `rmt` 这3个命令支持`type`,`db` 和 `key`正则表达式(Java风格)数据过滤  
2. `rst` 这个命令只支持`db`过滤  
  
举例如下:  

```shell

$ rct -f dump -s /path/to/dump.rdb -o /path/to/dump.aof -d 0
$ rct -f dump -s /path/to/dump.rdb -o /path/to/dump.aof -t string hash
$ rmt -s /path/to/dump.rdb -m redis://192.168.1.105:6379 -r -d 0 1 -t list
$ rst -s redis://127.0.0.1:6379 -m redis://127.0.0.1:6380 -d 0
```

### 监控Redis服务器

```shell
# 第一步 
# 打开文件 `/path/to/redis-rdb-cli/conf/redis-rdb-cli.conf`
# 将 `metric_gateway 这个属性 从 `none` 设置成 `influxdb`
#
# 第二步
$ cd /path/to/redis-rdb-cli/dashboard
$ docker-compose up -d
#
# 第三步
$ rmonitor -s redis://127.0.0.1:6379 -n standalone
$ rmonitor -s redis://127.0.0.1:30001 -n cluster
$ rmonitor -s redis-sentinel://sntnl-usr:sntnl-pwd@127.0.0.1:26379?master=mymaster&authUser=usr&authPassword=pwd -n sentinel
#
# 第四步
# 浏览器打开网址 `http://localhost:3000/d/monitor/monitor`, 用 `admin`, `admin` 登录grafana 查看监控结果
```

![monitor](./images/monitor.png)

### Redis大量数据插入

```shell

$ rct -f dump -s /path/to/dump.rdb -o /path/to/dump.aof -r
$ cat /path/to/dump.aof | /redis/src/redis-cli -p 6379 --pipe

```

### 把rdb转换成dump格式

```shell
$ rct -f dump -s /path/to/dump.rdb -o /path/to/dump.aof
```

### 把rdb转换成json格式

```shell
$ rct -f json -s /path/to/dump.rdb -o /path/to/dump.json
```

### rdb的key数量统计

```shell
$ rct -f count -s /path/to/dump.rdb -o /path/to/dump.csv
```

### 找到占用内存最大的50个key

```shell
$ rct -f mem -s /path/to/dump.rdb -o /path/to/dump.mem -l 50
```

### Diff rdb

```shell
$ rct -f diff -s /path/to/dump1.rdb -o /path/to/dump1.diff
$ rct -f diff -s /path/to/dump2.rdb -o /path/to/dump2.diff
$ diff /path/to/dump1.diff /path/to/dump2.diff
```

### 把rdb转换成RESP格式

```shell
$ rct -f resp -s /path/to/dump.rdb -o /path/to/appendonly.aof
```

### 2台redis之间数据同步
```shell
$ rst -s redis://127.0.0.1:6379 -m redis://127.0.0.1:6380 -r
```

### 同步单台redis的数据到集群
```shell
$ rst -s redis://127.0.0.1:6379 -m redis://127.0.0.1:30001 -r -d 0
```

### 同步rdb到远端redis

```shell
$ rmt -s /path/to/dump.rdb -m redis://192.168.1.105:6379 -r
```

### 同步rdb到远端redis集群

```shell
$ rmt -s /path/to/dump.rdb -c ./nodes-30001.conf -r
```
  
或者不用 `nodes-30001.conf` 这个配置文件, 直接使用如下命令  
  
```shell
$ rmt -s /path/to/dump.rdb -m redis://127.0.0.1:30001 -r
```

### 备份远端redis的rdb

```shell
$ rdt -b redis://192.168.1.105:6379 -o /path/to/dump.rdb
```

### 备份远端redis的rdb并把源端的db转换成目标db

```shell
$ rdt -b redis://192.168.1.105:6379 -o /path/to/dump.rdb --goal 3
```

### 过滤rdb

```shell
$ rdt -b /path/to/dump.rdb -o /path/to/filtered-dump.rdb -d 0 -t string
```

### 通过集群的nodes.conf把1个rdb分割成多个rdb

```shell
$ rdt -s ./dump.rdb -c ./nodes.conf -o /path/to/folder -d 0
```

### 合并多个rdb成1个

```shell
$ rdt -m ./dump1.rdb ./dump2.rdb -o ./dump.rdb -t hash
```

### 将 aof-use-rdb-preamble 文件形式分割成 rdb 文件与 aof 文件

```shell
$ rcut -s ./aof-use-rdb-preamble.aof -r ./dump.rdb -a ./appendonly.aof
```

### 其他参数

更多的可配置参数可以在 `/path/to/redis-rdb-cli/conf/redis-rdb-cli.conf` 这里配置

## rmt命令与rst命令的区别

1. 当 `rmt` 启动时. 源redis首先执行`BGSAVE`生成出一个rdb快照. `rmt` 把快照的数据迁移到目标redis. 迁移完成之后, `rmt` 命令成功结束并终止.  
2. `rst` 不仅仅迁移rdb快照文件,后续的增量数据也会迁移到目标redis. 因此 `rst` 不会手动终止. 但是按 `CTRL+C` 键可以终止同步. `rst` 命令只支持 `db` 过滤, 更多细节请参照 [同步的限制](#同步的限制) 

## Dashboard

从 `v0.1.9` 起, `rct -f mem` 支持在grafana上显示结果  
![memory](./images/memory.png)  

如果你想开启这项功能. **必须** 先安装 `docker` 和 `docker-compose`, 安装方法请参照 [docker](https://docs.docker.com/install/)  
然后遵循如下的步骤:  

```shell
$ cd /path/to/redis-rdb-cli/dashboard

# start
$ docker-compose up -d

# stop
$ docker-compose down
```
  
`cd /path/to/redis-rdb-cli/conf/redis-rdb-cli.conf`  
把 [metric_gateway](https://github.com/leonchen83/redis-rdb-cli/blob/master/src/main/resources/redis-rdb-cli.conf#L169) 这个参数从 `none` 改成 `influxdb`  
  
浏览器打开 `http://localhost:3000` 来查看 `rct -f mem` 命令的结果.  
  
如果你把这个工具部署在多个实例上, 需要更改如下参数 [metric_instance](https://github.com/leonchen83/redis-rdb-cli/blob/master/src/main/resources/redis-rdb-cli.conf#L215) 并保证在每个实例上参数名唯一  
  
## Redis 6
  
### Redis 6 SSL
  
1. 用 openssl 生成 keystore
  
```shell

$ cd /path/to/redis-6.0-rc1
$ ./utils/gen-test-certs.sh
$ cd tests/tls
$ openssl pkcs12 -export -CAfile ca.crt -in redis.crt -inkey redis.key -out redis.p12

```
  
2. 如果源 redis 和目标 redis 使用同样的 keystore. 那么配置如下参数  
将 [source_keystore_path](https://github.com/leonchen83/redis-rdb-cli/blob/master/src/main/resources/redis-rdb-cli.conf#L230) 和 [target_keystore_path](https://github.com/leonchen83/redis-rdb-cli/blob/master/src/main/resources/redis-rdb-cli.conf#L259) 指向 `/path/to/redis-6.0-rc1/tests/tls/redis.p12`  
设置 [source_keystore_pass](https://github.com/leonchen83/redis-rdb-cli/blob/master/src/main/resources/redis-rdb-cli.conf#L238) 和 [target_keystore_pass](https://github.com/leonchen83/redis-rdb-cli/blob/master/src/main/resources/redis-rdb-cli.conf#L267)  
  
3. 在配置完 ssl 参数之后, 在你的命令中使用 `rediss://host:port` 这样的URI来开启ssl, 比如: `rst -s rediss://127.0.0.1:6379 -m rediss://127.0.0.1:30001 -r -d 0`
  
### Redis 6 ACL
  
1. 使用如下的 URI 来开启 redis ACL 支持  
  
```shell
$ rst -s redis://user:pass@127.0.0.1:6379 -m redis://user:pass@127.0.0.1:6380 -r -d 0
```
  
2. `user` **必须** 拥有 `+@all` 权限来处理同步命令
  
## Hack rmt

### Rmt 线程模型

`rmt`使用下面四个参数([redis-rdb-cli.conf](https://github.com/leonchen83/redis-rdb-cli/blob/master/src/main/resources/redis-rdb-cli.conf))来同步数据到远端.  
  
```properties
migrate_batch_size=4096
migrate_threads=4
migrate_flush=yes
migrate_retries=1
```

最重要的参数是 `migrate_threads=4`. 这意味着我们用如下的线程模型同步数据  

```text

单 redis ----> 单 redis

+--------------+         +----------+     thread 1      +--------------+
|              |    +----| Endpoint |-------------------|              |
|              |    |    +----------+                   |              |
|              |    |                                   |              |
|              |    |    +----------+     thread 2      |              |
|              |    |----| Endpoint |-------------------|              |
|              |    |    +----------+                   |              |
| Source Redis |----|                                   | Target Redis |
|              |    |    +----------+     thread 3      |              |
|              |    |----| Endpoint |-------------------|              |
|              |    |    +----------+                   |              |
|              |    |                                   |              |
|              |    |    +----------+     thread 4      |              |
|              |    +----| Endpoint |-------------------|              |
+--------------+         +----------+                   +--------------+

``` 

```text

单 redis ----> redis 集群

+--------------+         +----------+     thread 1      +--------------+
|              |    +----| Endpoints|-------------------|              |
|              |    |    +----------+                   |              |
|              |    |                                   |              |
|              |    |    +----------+     thread 2      |              |
|              |    |----| Endpoints|-------------------|              |
|              |    |    +----------+                   |              |
| Source Redis |----|                                   | Redis cluster|
|              |    |    +----------+     thread 3      |              |
|              |    |----| Endpoints|-------------------|              |
|              |    |    +----------+                   |              |
|              |    |                                   |              |
|              |    |    +----------+     thread 4      |              |
|              |    +----| Endpoints|-------------------|              |
+--------------+         +----------+                   +--------------+

``` 

上面两张图的不同点在 `Endpoint` 和 `Endpoints`. 在集群同步中 `Endpoints` 包含多个 `Endpoint`, 每个`Endpoint` 和集群中的 `master` 链接, 举例如下:  
  
集群中有 3 master 3 replica. 如果 `migrate_threads=4` 那么我们有 `3 * 4 = 12` 个连接与redis集群相连. 

### 同步性能

下面3个参数影响同步性能  
  
```properties
migrate_batch_size=4096
migrate_retries=1
migrate_flush=yes
```

1. `migrate_batch_size`: 默认我们使用redis的 `pipeline` 来同步数据. `migrate_batch_size` 就是 `pipeline` 批处理大小. 如果 `migrate_batch_size=1` 那么 `pipeline` 的大小就退化成处理单条命令并同步等待命令结果返回.  
2. `migrate_retries`: `migrate_retries=1` 意思是如果 socket 连接错误发生. 我们重建一个新的 socket 并重试1次把上次发送失败的命令重新发送一遍.  
3. `migrate_flush`: `migrate_flush=yes` 意思是我们每写入socket一条命令之后, 立即调用一次 `SocketOutputStream.flush()`. 如果 `migrate_flush=no` 我们每写入 64KB 到 socket 才调用一次 `SocketOutputStream.flush()`. 请注意这个参数影响 `migrate_retries`. `migrate_retries` 只有在 `migrate_flush=yes` 的时候生效.  

### 同步原理

```text

+---------------+             +-------------------+    restore      +---------------+ 
|               |             | redis dump format |---------------->|               |
|               |             |-------------------|    restore      |               |
|               |   convert   | redis dump format |---------------->|               |
|    Dump rdb   |------------>|-------------------|    restore      |  Targe Redis  |
|               |             | redis dump format |---------------->|               |
|               |             |-------------------|    restore      |               |
|               |             | redis dump format |---------------->|               |
+---------------+             +-------------------+                 +---------------+
```

## 同步的限制

1. 我们通过集群的 `nodes.conf` 文件来同步数据到集群. 因为我们没有处理 `MOVED` `ASK` 重定向. 因此唯一的限制是集群在同步期间 **必须** 是稳定的状态. 这意味着集群 **必须** 不存在 `migrating`, `importing` 这样的slot. 而且没有主从切换. 
2. 当使用 `rst` 命令迁移数据到集群的时候. 下面的命令不支持： `PUBLISH,SWAPDB,MOVE,FLUSHALL,FLUSHDB,MULTI,EXEC,SCRIPT FLUSH,SCRIPT LOAD,EVAL,EVALSHA`. 下面的命令**有限支持** `RPOPLPUSH,SDIFFSTORE,SINTERSTORE,SMOVE,ZINTERSTORE,ZUNIONSTORE,DEL,UNLINK,RENAME,RENAMENX,PFMERGE,PFCOUNT,MSETNX,BRPOPLPUSH,BITOP,MSET,COPY,BLMOVE,LMOVE,ZDIFFSTORE,GEOSEARCHSTORE`.**只有这些命令里包含的 keys 在同一个slot的时候**(eg: `del {user}:1 {user}:2`)才支持.

## Hack ret

### ret命令是做什么的

1. `ret` 命令允许用户定义自己的同步服务 比如同步redis数据到 `mysql` 或 `mongodb`.
2. `ret` 命令使用 Java SPI 来实现同步功能.

### 如何实现一个同步服务

用户遵循如下步骤来实现一个同步服务

1. 使用如下maven pom.xml文件创建一个Java工程

```xml  

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.your.company</groupId>
    <artifactId>your-sink-service</artifactId>
    <version>1.0.0</version>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.moilioncircle</groupId>
            <artifactId>redis-rdb-cli-api</artifactId>
            <version>1.8.0</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.moilioncircle</groupId>
            <artifactId>redis-replicator</artifactId>
            <version>[3.6.4, )</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.25</version>
            <scope>provided</scope>
        </dependency>
        
        <!-- 
        <dependency>
            other dependencies
        </dependency>
        -->
        
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <version>3.1.0</version>
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                    <encoding>${project.build.sourceEncoding}</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```

2. 实现 `SinkService` 接口

```java  

public class YourSinkService implements SinkService {

    @Override
    public String sink() {
        return "your-sink-service";
    }

    @Override
    public void init(File config) throws IOException {
        // 解析你的外部配置文件
    }

    @Override
    public void onEvent(Replicator replicator, Event event) {
        // 你的同步业务代码
    }
}

```
3. 使用Java SPI来注册这个实现类

```text
# 在工程下的 src/main/resources/META-INF/services/ 目录创建 com.moilioncircle.redis.rdb.cli.api.sink.SinkService 文件

|-src
|____main
| |____resources
| | |____META-INF
| | | |____services
| | | | |____com.moilioncircle.redis.rdb.cli.api.sink.SinkService

# 在com.moilioncircle.redis.rdb.cli.api.sink.SinkService文件中加入如下内容

your.package.YourSinkService

```

4. 打包与部署

```shell

$ mvn clean install

$ cp ./target/your-sink-service-1.0.0-jar-with-dependencies.jar /path/to/redis-rdb-cli/lib
```
5. 运行你自己的同步服务

```shell

$ ret -s redis://127.0.0.1:6379 -c config.conf -n your-sink-service
```

6. debug 你自己的同步服务

```java  

    public static void main(String[] args) throws Exception {
        Replicator replicator = new RedisReplicator("redis://127.0.0.1:6379");
        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
            Replicators.closeQuietly(replicator);
        }));
        replicator.addExceptionListener((rep, tx, e) -> {
            throw new RuntimeException(tx.getMessage(), tx);
        });
        SinkService sink = new YourSinkService();
        sink.init(new File("/path/to/your-sink.conf"));
        replicator.addEventListener(new AsyncEventListener(sink, replicator, 4, Executors.defaultThreadFactory()));
        replicator.open();
    }

```

### 如何实现一个formatter服务

1. 创建class `YourFormatterService` 继承 `AbstractFormatterService`  

```java  

public class YourFormatterService extends AbstractFormatterService {

    @Override
    public String format() {
        return "test";
    }

    @Override
    public Event applyString(Replicator replicator, RedisInputStream in, int version, byte[] key, int type, ContextKeyValuePair context) throws IOException {
        byte[] val = new DefaultRdbValueVisitor(replicator).applyString(in, version);
        getEscaper().encode(key, getOutputStream());
        getEscaper().encode(val, getOutputStream());
        getOutputStream().write('\n');
        return context;
    }
}

```

2. 使用Java SPI来注册这个实现类  

```text
# create com.moilioncircle.redis.rdb.cli.api.format.FormatterService file in src/main/resources/META-INF/services/

|-src
|____main
| |____resources
| | |____META-INF
| | | |____services
| | | | |____com.moilioncircle.redis.rdb.cli.api.format.FormatterService

# add following content in com.moilioncircle.redis.rdb.cli.api.format.FormatterService

your.package.YourFormatterService

```

3. 打包与部署

```shell

$ mvn clean install

$ cp ./target/your-service-1.0.0-jar-with-dependencies.jar /path/to/redis-rdb-cli/lib
```

4. 运行formatter服务

```shell

$ rct -f test -s redis://127.0.0.1:6379 -o ./out.csv -t string -d 0 -e json
```

## 贡献者
  
* [Baoyi Chen](https://github.com/leonchen83)
* [Jintao Zhang](https://github.com/tao12345666333)
* [Maz Ahmadi](https://github.com/cmdshepard)
* [Anish Karandikar](https://github.com/anishkny)
* [Air](https://github.com/air3ijai)
* 特别感谢[Kater Technologies](https://www.kater.com/)

## Supported by IntelliJ IDEA

[IntelliJ IDEA](https://www.jetbrains.com/?from=redis-rdb-cli) is a Java integrated development environment (IDE) for developing computer software.  
It is developed by JetBrains (formerly known as IntelliJ), and is available as an Apache 2 Licensed community edition,  
and in a proprietary commercial edition. Both can be used for commercial development.  
