---
title: graphql后缀文件在express中使用
categories:
  - 程海迷航
date: 2019-10-30 12:12:10
tags: 
- graphql 
- express
archives:
---

1. 安装
npm i graphql

2. 创建graphql后缀文件(主流编辑器都有带高亮提示了现在)
编写内容如下
```graphql
type itemRe {
    key:String
    value:String
}

type Query {
    q_test:String
    q_api_login(account:String!, password:String!):Boolean
    q_check_user(account:String!):String
    q_get_item(key:String!, account:String!):[itemRe]
}


type Mutation{
    m_test:String
    m_register(account:String!, password:String!):Boolean
    m_modify_password(account:String!, password:String!):Boolean

    # 输入账号密码发送邮件,当前只支持@qq.com,  @foxmail.com 的邮箱
    m_send_email(fromemail:String!, password:String!, toemail:String!,
        title:String, txt:String, html:String):Boolean

    # value 最大不超过255个字符
    m_set_item(key:String!, value:String!, account:String!):Boolean,
    m_remove_item(key:String!, account:String!):Boolean,
}

schema  {
    query: Query
    mutation: Mutation
}
```

3. 在express框架中使用
```js
import * as express from "express";
import * as MApi from "./api_define/apilist";
import graphqlHTTP = require("express-graphql");
import {buildSchema,} from "graphql";
let path = require("path");
let app = express();
let fs = require("fs");
let data = fs.readFileSync(path.resolve("src/api_define/apidef.graphql"));
data = data.toString();
let schema = buildSchema(data);

let root = {
    q_test: () => "Query test ok",
    m_test: () => "Mutation test ok",

    // Query api
    q_api_login: async ({account, password}) => await MApi.api_login(account, password),
    q_check_user: async ({account}) => await MApi.check_user(account),
    q_get_item: async ({key, account}) => await MApi.api_get_item(key, account),

    // Mutation api
    m_register: async ({account, password}) => await MApi.api_register(account, password),
    m_modify_password: async ({account, password}) => await MApi.api_modify_password(account, password),
    m_send_email: async ({fromemail, password, toemail, title, txt, html}) => await MApi.api_send_email(fromemail, password, toemail, title, txt, html),
    m_set_item: async ({key, value, account}) => await MApi.api_set_item(key, value, account),
    m_remove_item: async ({key, account}) => await MApi.api_remove_item(key, account),
};
app.use('/g', graphqlHTTP({
    schema: schema,
    rootValue: root,
    graphiql: true, //控制显示控制台
}));


let server = app.listen(8877, function () {
    // @ts-ignore
    let port = server.address().port;
    console.log("管理后台，访问地址为 http://127.0.0.1:%s", port)
});


```


