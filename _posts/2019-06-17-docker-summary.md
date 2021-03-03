#Docker学习

##简介  
	1、开源的应用容器引擎，打包应用及依赖到这个可移植容器中，发布到Linux机器上，实现虚拟化  
	3、自2013年发布后，几年后就成为最主流容器工具  
	2、竞品：  
		容器与虚拟机  

##概念  
	容器：实质是进程，运行在自己的独立命名空间，隔离的环境
	镜像：特殊的文件系统，包含 程序、库、资源、配置等，一层层构建
	仓库：包含不同版本进项，tag , 默认标签 latest

##特性
	1、持续集成 ，docker在此过程可以 解决问题 1
	2、版本控制
	3、可移植性
	4、安全性、隔离性  ， 解决问题2

##面向的问题
	1、研发、测试、生产环境差异导致一系列问题
	2、多个组件交叉部署导致环境冲突和资源分配问题
	3、应用部署实施需要安装许多依赖，实施运维过程繁琐

##使用
	三步
		1、安装Docker运行环境
		2、获取Docker运行镜像
		3、运行Docker镜像
	命令
		docker 命令
		Dockerfile编写
			FROM \ MAINTAINER \ RUN \ COPY \ ADD \ENV \ EXPOSE \ WORKDIR \USER \VOLUME \CMD \ ENTRYPOINT

## 常用指令

```shell
# 例如删除包含“some”的镜像
docker rmi --force $(docker images | grep some | awk '{print $3}')

# 停止所有 Exited 的容器

docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker stop

# 删除所有 Exited 的容器

docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker rm

# 删除所有 none 镜像

docker images|grep none|awk '{print $3 }'|xargs docker rmi


**ps -ef |grep hello |awk '{print $2}'|xargs kill -9**

docker run --name sso-server  -p 13001:13001 --add-host api.geetest.com:172.16.154.106  --restart=always -d -e  JVM_OPTS='-Dapp.id=20007 -Dapollo.meta=http://172.31.205.72:25653 -Denv=dev' hub.iflytek.com/xfyun_test/sso-server

kubectl -n 339-hera --kubeconfig="D:\Program\kube\config\chaowang5-test.kubeconfig" set image deployment/1m-2020-integral 1m-2020-integral=hub.iflytek.com/xfyun_webdev/1m-2020-integral:test-integral-014

docker run --name auth  -p 9091:9091 -d -e SW_AGENT_TRACE_IGNORE_PATH='eureka/**,Lettuce/**' -e JVM_OPTS='-javaagent:/agent/skywalking-agent.jar -Dskywalking.agent.namespace=auth -Dskywalking.agent.service_name=auth -Dskywalking.collector.backend_service=  ' xxxx

```