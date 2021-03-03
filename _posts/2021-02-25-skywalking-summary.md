# Skywalking 使用 


##Skywalking 集群部署 

```
            - name: SW_CLUSTER
              value: kubernetes
            - name: SW_CLUSTER_K8S_NAMESPACE
              value: 339-apm
            - name: SW_CLUSTER_K8S_LABEL
              value: k8s-app=skyoap
            - name: SW_STORAGE
              value: elasticsearch
            - name: SW_STORAGE_ES_CLUSTER_NODES
              value: es:9200
```

##SkyWalking的集成

###测试环境修改部署时的参数  修改 JVM_OPS 参数

```
 增加      -javaagent:/agent/skywalking-agent.jar -Dskywalking.agent.service_name=xxxx -Dskywalking.collector.backend_service=172.31.202.28:11800

  xxxx  是你的业务系统名 
  
 "docker run --name auth -p 9091:9091 -d -e JVM_OPTS='-javaagent:/agent/skywalking-agent.jar -Dskywalking.agent.service_name=auth 
  -Dskywalking.collector.backend_service=xxx:11800 ' hub.iflytek.com/xfyun_test/auth"
 

```

###基于SideCar方式集成    

```
initContainers:
  - name: init-agent
    image: hub.iflytek.com/xfyun_webdev/skywalking-agent-sidecar:v-agent-1.1
    command:
      - 'sh'
      - '-c'
      - 'set -ex;mkdir -p /agents;cp -r /opt/skywalking/agent/* /agents;'
    volumeMounts:
      - mountPath: /agents
        name: agent
 
 
   -javaagent:/agents/skywalking-agent.jar -Dskywalking.agent.service_name=xxxxx -Dskywalking.collector.backend_service=xxxx:11800

#在volumeMounts 增加
- mountPath: /agents
  name: agent
#在volumes 中增加
- name: agent
  emptyDir: {}
```


##问题记录：
记录过程中遇到的典型问题

###1、在现网进行部署时需要用到Kubernates， 因为不会使用需要开始学习  —-  2020-05-18 

当前对Kubernates的掌握情况：k8s基本应用已掌握，

补充说明： 2021-03-01

skywalking 部署是可以选择单机部署也可以选择集群部署，具体配置需要对config/application.yml 文件做修改 ，见cluster模块

场景一：单机部署  ，默认的情形下 cluster.selector = standalone 即表示单实例， SW_CLUSTER 等左侧大写变量用于从外部传值，如 docker 运行时加参数





场景二：集群部署，一般用在现网，现网部署用k8s的脚本部署，参数通过变量传入 ，也可修改cluster.selector =kubernetes 和 cluster.kubernetes 中参数，labelSelector 务必填写正确，否则集群查找不成功，会造成部署失败，





###2、在验证skywalking-oap 服务的过程中发现集群模式为SW_CLUSTER=kubernates 无法传数据 -- 改为standalone 模式正常运行   ，此问题需要了解原理   – 2020-05-21 

      参见问题一解答  



###3、在学习了解k8s的过程中，了解到SideCar模式，可以运用到skywalking-agent的使用中， 决定调研它 。   — 2020-05-22

补充说明 -  2021-03-01

  sideCar模式有利于skywalking-agent的监控收集服务与业务应用的解耦， 也无须变更业务应用基础镜像，单独构建skywalking-agent的镜像，

  在k8s部署阶段，通过挂载的方式，将agent.jar 移植到业务应用可引用的存储位置，启动时加载即可，这样应用构建及部署方式上都更加灵活。

具体使用参见：SkyWalking的集成



###4、业务系统中agent连接 skywaling-oap服务报 grcp 连接超时或者连接错误     -- 2021-03-01

日志表现：   

ERROR 2021-02-20 14:48:10:684 SkywalkingAgent-7-ServiceAndEndpointRegisterClient-0 ServiceAndEndpointRegisterClient : ServiceAndEndpointRegisterClient execute fail.
org.apache.skywalking.apm.dependencies.io.grpc.StatusRuntimeException: DEADLINE_EXCEEDED: deadline exceeded after 29.999667597s. [remote_addr=/10.105.40.92:11800]
at org.apache.skywalking.apm.dependencies.io.grpc.stub.ClientCalls.toStatusRuntimeException(ClientCalls.java:240)

      日志查看命令 ： kubectl -n 339-sso exec -it sso-server-7844d4dd49-9prvm -- tail -100f /agents/logs/skywalking-api.log 

     连接不上有两种可能，一种是部署阶段用的 oap 地址不对，或者是DNS 不通导致连接不上，  可以ping下对应oap的地址，修改正确即可，  如果是异地机房网络不通且不可解决，那么也可以不加agent的收集

-Dskywalking.collector.backend_service 用正确的oap地址修改即可

    另一种情况，在后端服务较多的情形下，如果后端服务在后续升级或扩容或者新增了其他业务节点后， 发现某些后端服务节点的日志收集连接报这个错误，那么就极有可能是oap服务的性能不够了，需要进行扩容oap节点，  

  在扩容后写入的数据量也会增加，此时需要观察并注意elasticsearch 的运行情况， 如果es的负载cpu- 内存等 ，如果指标过高，建议进行es的扩容（如有告警es那边亮哥 会联系到这边 ，很稳）



###5、oap服务写入es报写入的数据版本错误，写入被拒绝

    在集群条件下，oap是多个实例，es中索引写入数据时会校验版本号，写入请求须按版本号递增，oap多实例时会进行集群协调，保证写入同一索引不冲突，那么产生冲突可能有两种原因，

一种情况是 部署了另外的oap 导致写入时产生冲突，一种是 集群内部协调产生问题，或者部署方式未按集群设置。

 第一种的情况则可以采取下线，另外的oap服务，或者将es设置为引用另外的，或者设置  storage.elasticsearch.nameSpace参数，默认为空，不同oap集群设置不同的群分下es中的数据索引即可。

第二种情况 一般是部署时 配置错误，检查cluster.kubernetes.labelSelector  ,cluster.kubernetes.namespace配置的lebal是否正确  ，  另外也请查看pod的event和pod内 skywalking-oap-server.log的日志，检查集群内的pod 网络是否有问题，

     适当调整判活的参数，以保证oap不同pod在启动时可以顺利建立连接，构建集群。



###6、skywalking 升级问题 

 背景 – 8.4一下的版本存在一个安全漏洞，因此调研了升级8.4版本

 在测试环境的调研过程中发现8.4版本与当前使用的7.0版本存在较大差异，服务接口及存储的索引数据都有变动， 原先的数据不能平滑转移

需要整个部署8.4版本的oap\ ui ，且因为接口变动，原先agent上传会报错，需要替换agent才可以，代价较大，结合skywalking在内网，只有内部人员访问，暂时就不做升级了。



##ElasticSearch 相关 

```
PUT /_cluster/settings
{
  "transient": {
    "cluster": {
      "max_shards_per_node":10000
    }
  }
}

POST \*2020060\*/_forcemerge?max_num_segments=1


POST  *,-segment*/_forcemerge?max_num_segments=1

```
