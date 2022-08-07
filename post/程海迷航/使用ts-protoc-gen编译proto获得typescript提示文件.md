---
title: 使用ts-protoc-gen编译proto获得typescript提示文件
categories:
  - 程海迷航
date: 2019-08-24 08:46:42
tags: 
- nodejs
- proto
archives:
---

### 目的:
为了使用typescript语法调用gRPC.

### 过程
1. 各种找,也试过用thrift输出typescript代码,各种错误依赖都错.
然后尝试用使用gRPC.

2. 然后我找到一个名字叫ts-protoc-gen的包,感觉找到了救星
https://github.com/agreatfool/grpc_tools_node_protoc_ts

3. 3.官网那个配置是机遇linux操作的在windows10下跑不了,有以下这几种错误
![](12.png)

4. 官网命令(自己跑不起来)
```shell script
npm install grpc_tools_node_protoc_ts --save-dev

# generate js codes via grpc-tools
grpc_tools_node_protoc \
--js_out=import_style=commonjs,binary:./your_dest_dir \
--grpc_out=./your_dest_dir \
--plugin=protoc-gen-grpc=`which grpc_tools_node_protoc_plugin` \
-I ./proto \
./your_proto_dir/*.proto

# generate d.ts codes
protoc \
--plugin=protoc-gen-ts=./node_modules/.bin/protoc-gen-ts \
--ts_out=./your_dest_dir \
-I ./proto \
./your_proto_dir/*.proto
```

5. 自己修改后的命令(需要你自己修改plugin路径)
```
protoc --grpc_out=./   --js_out=import_style=commonjs,binary:./  --plugin=protoc-gen-grpc=D:/comany_project/monster_farm/server/node_modules/.bin/grpc_tools_node_protoc_plugin.cmd index.proto

protoc --grpc_out=./    --plugin=protoc-gen-grpc=D:/comany_project/monster_farm/server/node_modules/.bin/grpc_tools_node_protoc_ts_plugin.cmd index.proto
```

`两条命令使用的插件名不同grpc_tools_node_protoc_ts_plugin.cmd 中间有个ts`

6. 坑
* window下 protoc-gen-ts后面要加个cmd, 且--plugin对应的参数要用绝对路径,如果不用绝对路径,则需要使用window特定的相对路径书写方式,例如 .\\*****\\***这样.

7. 环境依赖
package.json文件内容:
```json
{
   "name": "ts",
   "version": "1.0.0",
   "description": "",
   "main": "index.js",
   "scripts": {
     "test": "echo \"Error: no test specified\" && exit 1"
   },
   "author": "",
   "license": "ISC",
   "devDependencies": {
   },"dependencies": {
     "google-protobuf": "^3.0.0",
     "grpc": "^1.11.0"
   },
   "devDependencies": {
     "@types/node": "^11.11.4",
     "grpc-tools": "latest",
     "grpc-tools-ts": "latest", 
     "grpc_tools_node_protoc_ts": "^2.4.2",
     "mocha": "latest",
     "ts-node": "^8.0.3",
     "ts-protoc-gen": "^0.9.0",
     "typescript": "^3.3.4000",
     "typings": "latest"
   }
}
```

protoc版本 
[protoc-3.7.0-win64](https://github.com/protocolbuffers/protobuf/releases/download/v3.7.0/protoc-3.7.0-win64.zip)
