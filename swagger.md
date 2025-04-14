### swagger生成接口文档
Swagger本质上是一种用于描述使用JSON表示的RESTful API的接口描述语言。

### gin-swagger实战
想要使用`gin-swagger`为代码自动生成接口文档，一般需要下面三个步骤：
- 按照swagger要求给接口代码添加声明式注释
- 使用swag工具扫描代码自动生成API接口文档数据
- 使用gin-swagger渲染在线接口文档页面

#### 安装
`go get -u github.com/swaggo/cmd/swag`

#### 添加注释
在程序入口main函数上以注释的方式写下项目相关介绍信息。

#### 生成接口文档数据
编写完注释后，在项目根目录执行以下命令，使用swag工具生成接口文档数据。
`swag init`
执行完上述命令后，如果写的注释格式没问题，此时项目根目录下会多出来一个`docs`文件夹。

#### 引入gin-swagger渲染文档数据
在项目代码中注册路由的地方按如下方式引入`gin-swagger`相关内容：
```Go
import (
    _ "shit/docs" // 记得导入上一步生成的docs

    gs "github.com/swaggo/gin-swagger"
    "github.com/swaggo/gin-swagger/swaggerFiles"
    "github.com/gin-gonic/gin"
)
```
注册swagger api相关路由
```Go
r.GET("/swagger/*any", gs.WrapHandler(swaggerFiles.Handler))
```

`gin-swagger`同时还提供了`DisablingWrapHandler`函数，方便我们通过设置某些环境变量来禁用Swagger。
```Go
r.GET("/swagger/*any", gs.DisablingWrapHandler(swaggerFiles.Handler, "NAME_OF_ENV_VARIABLE"))
```
此时如果将环境变量`NAME_OF_ENV_VARIABLE`设置为任意值，则`/swagger/*any`将返回404响应，就像未指定路由时一样。