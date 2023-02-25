## 打包镜像

docker build -t xxname . 

> 以当前目录下的dockerfile打名称为xxname的包



## 启动：

**docker run -it -p 30:3000 -e TEXT="ok" test node npm start**

>运行docker 把容器内的3000端口映射到本机的30端口 容器内变量TEXT=ok 修改之后运行npm start命令



##  dockerfile中的变量

ARG key=value

ENV key=value

arg是只有在构建过程中会使用的变量

env是在运行时使用的变量



ARG V=2

COPY . ./

RUN npm i

在此之前V=2

ENV V=3

在此后 V=3

> 如果把ENV V=3注释 在RUN npm i 之后的V等于空



## 本地测试是否打包好镜像

Docker run -it /User/xxx/目录:/app -p 30:3000 node:12-slim bash

> docker把当前目录用node:12-slim的镜像 跑起来
>
> 把本机的xx目录下映射到容器里的/app目录



![image-20220723164447940](/Users/wuazhu/Library/Application Support/typora-user-images/image-20220723164447940.png)

















