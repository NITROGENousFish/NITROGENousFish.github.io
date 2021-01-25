---
title: a fast view of FastAPI
date: 2021-01-07 18:16:54
tags:
- 前端
---

# 引言

这个教程是



# 必要的下载

第一个是自己，第二个是服务器，第三个是弄Form和文件的

```shell
pip install fastapi
pip install uvicorn[standard]
pip install python-multipart
```

# 如何启动应用

```
uvicorn main:app --reload
```

其中main是你的py文件名称，可以是绝对路径，app是在内部注册的应用



# 知识结构

# 快速启动

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}
```

# 路径参数和查询参数

>  :100: 除非声明了是Optinal，否则没有默认值的参数都是必须的参数
>
> 引自（知名摸鱼专家：NitrogenousFish）

## 路径参数

### 声明路径参数

1. 在**函数装饰器**里面加上`{var}`
2. 在下方函数中的参数中，填写变量名称和类型，用冒号分割`(var:str)`

```python
@app.get("/users/{user_id}")
async def read_user(user_id: str):
	...
```



> :key: 要点：路径参数的**顺序很重要**：如果把下面的`/me`和`/{user_id}`写反了，那么在请求`/me`的时候，会被`/{user_id}`先获取到

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users/me")
async def read_user_me():
    return {"user_id": "the current user"}
@app.get("/users/{user_id}")
async def read_user(user_id: str):
    return {"user_id": user_id}
```

### 包含路径的路径参数：支持，但是平常最好别这么写

详情参见：https://fastapi.tiangolo.com/zh/tutorial/path-params/#_14

## 查询参数：不是路径参数的参数

【定义】声明不属于路径参数的其他函数参数时，它们将被自动解释为"查询字符串"参数

**查询字符串是键值对的集合，这些键值对位于 URL 的 `？` 之后，并以 `&` 符号分隔。**

```
http://127.0.0.1:8000/items/?skip=0&limit=10
```

他们原始都是str类型，当为它们声明了 Python 类型之后，就能够进行类型转换，并且针对该类型进行校验

### 声明查询参数

声明方法和路径参数第二步一样，就是不会在函数装饰器里面加上对应的内容

### 查询参数 可选与默认值

查询参数如果有，就是对应的值；如果没有查询，就会使用默认值

【**声明查询参数可选**】：声明为Optinal，设置默认值为None

【声明查询参数默认值】：

```python
from typing import Optional
...				# 这里的 q 和 short 都是可选参数
@app.get("/items/{item_id}")
async def read_item(item_id: str, q: Optional[str] = None, short: bool = False):
```

### 查询参数必选

【声明查询参数必选】：就不要设定默认值

```python
@app.get("/items/{item_id}")
async def read_user_item(item_id: str, needy: str):		# 这里的needy就是必选参数			
```

### 查询参数 必选-可选-默认值 总结

在这个例子中，有3个查询参数：

- `needy`，一个必需的 `str` 类型参数。
- `skip`，一个默认值为 `0` 的 `int` 类型参数。
- `limit`，一个可选的 `int` 类型参数。

```python
from typing import Optional
from fastapi import FastAPI
app = FastAPI()
@app.get("/items/{item_id}")
async def read_user_item(
    item_id: str, needy: str, skip: int = 0, limit: Optional[int] = None
):
    item = {"item_id": item_id, "needy": needy, "skip": skip, "limit": limit}
    return item
```

# 请求体 （POST）

请求体：客户端发送给 API 的数据

相应体：API 发送给客户端的数据

请求体除了POST还有PUT，DELETE，PATCH等

## 创建请求体——Pydantic

```python
from typing import Optional

from fastapi import FastAPI
from pydantic import BaseModel					#首先，你需要从 pydantic 中导入 BaseModel：


class Item(BaseModel):							#然后，将你的数据模型声明为继承自 BaseModel 的类
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None					#使用标准的 Python 类型来声明所有属性


app = FastAPI()


@app.put("/items/{item_id}")
# item_id路径参数，必选；item请求体，必选；q查询参数，可选&默认None
async def create_item(item_id: int, item: Item, q: Optional[str] = None):
    result = {"item_id": item_id, **item.dict()}
    if q:
        result.update({"q": q})
    return result
```

### 如何解析 路径参数 查询参数 请求体

函数参数将依次按如下规则进行识别：

- 如果在**路径**中也声明了该参数，它将被用作路径参数。
- 如果参数属于**单一类型**（比如 `int`、`float`、`str`、`bool` 等）它将被解释为**查询**参数。
- 如果参数的类型被声明为一个 **Pydantic 模型**，它将被解释为**请求体**。

### Pydantic模型套娃（请求体-嵌套模型）

不细嗦了，直接看吧，和你想的差不多～

包含有model套model，model套List(model)，就是List(model)

https://fastapi.tiangolo.com/zh/tutorial/body-nested-models/

### Pydantic模型注释强化（模式的额外信息 - 例子）

https://fastapi.tiangolo.com/zh/tutorial/schema-extra-example/

推荐大概弄懂了全部内容再来看。和运行功能无关，是注释和可读性的一个例子



## 创建请求体——Body 请求体中的单一值

### Body基础——单独键

当你的请求体中只需要一个单独数值（请求中的一个单独的键）的时候，不用再建一个class了，直接`from fastapi import Body `

这样的一个程序：

```python
from typing import Optional

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None


class User(BaseModel):
    username: str
    full_name: Optional[str] = None


@app.put("/items/{item_id}")
async def update_item(
    item_id: int, item: Item, user: User, importance: int = Body(...)
):
    results = {"item_id": item_id, "item": item, "user": user, "importance": importance}
    return results
```

期望到这样的请求体：


```
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    },
    "user": {
        "username": "dave",
        "full_name": "Dave Grohl"
    },
    "importance": 5
}
```

### 给单个Pydantic嵌入请求体参数

假设**你只有一个**来自 Pydantic 模型 `Item` 的请求体参数 `item`，你想要如下形式的

```
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    }
}
```

而不是

```
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2
}
```

你需要在最后添加等于Body，并且在最后加上embed=True：`item: Item = Body(..., embed=True)`

```python
from typing import Optional

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: Optional[str] = None
    price: float
    tax: Optional[float] = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item = Body(..., embed=True)):		# 看这里～
    results = {"item_id": item_id, "item": item}
    return results
```



# 参数校验（路径参数和查询参数）

 :key: **重点**：

- **查询参数是Query类**

- **路径参数是Path类，**

- **Body类请求体-内部自带校验接口**

- **Pydantic请求体-用用 Pydantic 的 `Field`**类在Pydantic模型内部声明校验和元数据

下文所有内容将以Query类为例，默认都是查询参数

## 参数校验基础用法——查询

- 首先导入Query

- 把Query设置为默认值
  - 其中`Query(None)`等价于`None`，随后我们就可以传递更多的参数给`Query()`
  - 可以给Query带有多个校验内容：`Query(None, min_length=3, max_length=50)`
  - 可以定义一个参数值需要匹配的正则表达式`Query(None, min_length=3, max_length=50, regex="^fixedquery$")`
  - 可以进行数值校验：
    - `item_id: int = Path(..., title="The ID of the item to get", ge=1)`路径参数要大于等于1
    - ge大于等于，gt大于，eq等于，le小于等于，lt小于
  - Query中**第一个参数是默认值**：这里None就是这个Query的默认值
  - 如果这个**参数是必须的**，那么默认参数设置为`...` （python关键字：省略号）：如`Query(..., min_length=3)`

>  :key: **重点**
>
>  路径参数总是必需的，因为它必须是路径的一部分。
>
>  所以，你应该在声明时使用 `...` 将其标记为必需参数。
>
>  然而，即使你使用 `None` 声明路径参数或设置一个其他默认值也不会有任何影响，它依然会是必需参数

```python
from typing import Optional
from fastapi import FastAPI, Query			# 首先导入Query
app = FastAPI()

@app.get("/items/")
async def read_items(q: Optional[str] = Query(None, max_length=50)):	#把Query设置为默认值
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

## 参数校验的写法顺序排列

 :warning: 如果你将带有「默认值」的参数放在没有「默认值」的参数之前，Python 将会报错。

如果同时混有路径参数和查询参数（没有默认值），路径参数有Path进行校验，但是查询参数没有Query，这样写没有问题

```python
# q 是没有默认值的查询参数，item_id是有默认值的路径参数
# 这种写法没有问题
async def read_items(q: str, item_id: int = Path(..., title="The ID of the item to get")):
```

但是如果反过来，路径参数放在前面，就会报错

```python
async def read_items(item_id: int = Path(..., title="The ID of the item to get", q: str)):
# SyntaxError: non-default argument follows default argument
```

解决方法是这样的：加一个`*`告诉python解释器，把后面的所有参数当成`kwargs`来看待。

```python
async def read_items(*, item_id: int = Path(..., title="The ID of the item to get"), q: str):
```

**还是推荐把这些没有默认值的可选项都设置上一个默认值吧，反正是我我就这么干（要是完全不关心性能的话）**



## 校验参数列表/多值 基础用法

>  :100: 想怎么套娃，就怎么套娃~					
>
>  引自（知名摸鱼专家：NitrogenousFish）

`Optional[List[str]] = Query(None)`

```python
from typing import List, Optional
from fastapi import FastAPI, Query
app = FastAPI()

@app.get("/items/")
async def read_items(q: Optional[List[str]] = Query(None)):		# 看这里看这里
    query_items = {"q": q}
    return query_items
```

假设请求的URL是：

```
http://localhost:8000/items/?q=foo&q=bar
```

那么得到的响应就是

```python
{
  "q": [
    "foo",
    "bar"
  ]
}
```

## 校验参数列表/多值 默认值

`Query(["foo", "bar"])`

比如这样～

```python
async def read_items(q: List[str] = Query(["foo", "bar"])):
async def read_items(q: list = Query([])):
```

需要注意的是：`List[int]` 将检查（并记录到文档）列表的内容必须是整数。但是单独的 `list` 不会。

## Field-Pydantic参数校验举例

**首先导入Field：因为是在pydantic内部校验，所以不是从fastapi导入而是pydantic**

然后用法剩下的都是一样的：看Item.price这里

```python
from typing import Optional

from fastapi import Body, FastAPI
from pydantic import BaseModel, Field		#导入Field

app = FastAPI()

class Item(BaseModel):
    name: str
    description: Optional[str] = Field(
        None, title="The description of the item", max_length=300
    )
    price: float = Field(..., gt=0, description="The price must be greater than zero")
    tax: Optional[float] = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item = Body(..., embed=True)):
    results = {"item_id": item_id, "item": item}
    return results
```

## 一些有关参数校验的技术细节

实际上，`Query`、`Path` 和其他你将在之后看到的类，创建的是由一个共同的 `Params` 类派生的子类的对象，该共同类本身又是 Pydantic 的 `FieldInfo` 类的子类。

Pydantic 的 `Field` 也会返回一个 `FieldInfo` 的实例。

`Body` 也直接返回 `FieldInfo` 的一个子类的对象。还有其他一些你之后会看到的类是 `Body` 类的子类。

请记住当你从 `fastapi` 导入 `Query`、`Path` 等对象时，他们实际上是返回特殊类的函数。

## 校验参数高级内容

https://fastapi.tiangolo.com/zh/tutorial/query-params-str-validations/#_9

- 添加更多的参数信息 title，description
- 参数别名 alias
- 指定参数将会被弃用 deprecated



# 

# 有关类型想说的一些事情

## 创建枚举类型

``` python
from enum import Enum
#枚举类型继承自str和Enum；继承str，API文档将能够知道这些值必须为 string 类型并且能够正确地展示出来
class ModelName(str, Enum):			
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"
    
# 用法
a = 'alexnet'
print(a == ModelName.alexnet)       # True
print(type(ModelName.alexnet))      # <enum 'ModelName'>
```



## pydantic支持更多的类型

**基础：**

常见的int, bool, str都没有什么问题

在typing中impost List Set之类的类型也没有什么问题

**进阶：**

比如UUID，bytes，datetime.datetime，datetime.date

更全的pydantic数据类型看这里：https://pydantic-docs.helpmanual.io/usage/types/





# 一些有关HTTP的事情但是作为一个API没有什么需要关心的地方

【cookie参数】https://fastapi.tiangolo.com/zh/tutorial/cookie-params/

【响应模型】https://fastapi.tiangolo.com/zh/tutorial/response-model/

【额外的模型】https://fastapi.tiangolo.com/zh/tutorial/extra-models/

【响应状态码】https://fastapi.tiangolo.com/zh/tutorial/response-status-code/

一看就懂，就这么写：`@app.post("/items/", status_code=201) `

为了自动代码补全，可以这么写：fastapi.status

```python
from fastapi import FastAPI, status
@app.post("/items/", status_code=status.HTTP_201_CREATED)
async def blabla():
    ...
```

【表单】https://fastapi.tiangolo.com/zh/tutorial/request-forms/

提供API不需要响应表单（确信）

## 文件

有两个接口，一个是byte，一个是**UploadFile**，推荐用后一个

```python
from fastapi import FastAPI, File, UploadFile

app = FastAPI()

# 木大木大
@app.post("/files/")
async def create_file(file: bytes = File(...)):
    return {"file_size": len(file)}

# 针不戳～
@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile = File(...)):
    return {"filename": file.filename}
```

UploadFile有一些属性和异步方法～点这里看简介

https://fastapi.tiangolo.com/zh/tutorial/request-files/#uploadfile

# 后记

后面还有一些内容，我觉得比较有意思的是JSON，Middleware，和Debugging

JSON大概说了是怎么处理JSON请求之类的。

Middleware相当于是一个批处理，能够在enduser和服务器之间拦下来做统一的输入预处理和输出预处理

Debugging讲了怎么在IDE里面帮你调试

- [Handling Errors](https://fastapi.tiangolo.com/zh/tutorial/handling-errors/)
- [Path Operation Configuration](https://fastapi.tiangolo.com/zh/tutorial/path-operation-configuration/)
- [JSON Compatible Encoder](https://fastapi.tiangolo.com/zh/tutorial/encoder/)
- [Body - Updates](https://fastapi.tiangolo.com/zh/tutorial/body-updates/)
- Dependencies
  - [Dependencies - First Steps](https://fastapi.tiangolo.com/zh/tutorial/dependencies/)
  - [Classes as Dependencies](https://fastapi.tiangolo.com/zh/tutorial/dependencies/classes-as-dependencies/)
  - [Sub-dependencies](https://fastapi.tiangolo.com/zh/tutorial/dependencies/sub-dependencies/)
  - [Dependencies in path operation decorators](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-in-path-operation-decorators/)
  - [Global Dependencies](https://fastapi.tiangolo.com/zh/tutorial/dependencies/global-dependencies/)
  - [Dependencies with yield](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-with-yield/)
- Security
  - [Security Intro](https://fastapi.tiangolo.com/zh/tutorial/security/)
  - [Security - First Steps](https://fastapi.tiangolo.com/zh/tutorial/security/first-steps/)
  - [Get Current User](https://fastapi.tiangolo.com/zh/tutorial/security/get-current-user/)
  - [Simple OAuth2 with Password and Bearer](https://fastapi.tiangolo.com/zh/tutorial/security/simple-oauth2/)
  - [OAuth2 with Password (and hashing), Bearer with JWT tokens](https://fastapi.tiangolo.com/zh/tutorial/security/oauth2-jwt/)
- [Middleware](https://fastapi.tiangolo.com/zh/tutorial/middleware/)
- [CORS (Cross-Origin Resource Sharing)](https://fastapi.tiangolo.com/zh/tutorial/cors/)
- [SQL (Relational) Databases](https://fastapi.tiangolo.com/zh/tutorial/sql-databases/)
- [Bigger Applications - Multiple Files](https://fastapi.tiangolo.com/zh/tutorial/bigger-applications/)
- [Background Tasks](https://fastapi.tiangolo.com/zh/tutorial/background-tasks/)
- [Metadata and Docs URLs](https://fastapi.tiangolo.com/zh/tutorial/metadata/)
- [Static Files](https://fastapi.tiangolo.com/zh/tutorial/static-files/)
- [Testing](https://fastapi.tiangolo.com/zh/tutorial/testing/)
- [Debugging](https://fastapi.tiangolo.com/zh/tutorial/debugging/)https://fastapi.tiangolo.com/zh/tutorial/debugging/)


