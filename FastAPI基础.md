# Fast API 基础

## 一、实例

最简单的Fast API接口

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"Hello": "World"}
```

启动服务：

```py	
uvicorn main:app --reload
```

**question：**

----

>**为什么要创建虚拟环境？**
>
>​	隔离项目运行环境，避免依赖冲突，保持全局环境的干净和稳定
>
>**怎么运行Fast API项目？**
>
>​	run 项目
>
>​	uvicorn main:app --reload   ---reload:更改代码后自动重启服务器
>
>**怎么访问Fast API交互式文档？**
>
>​	http://127.0.0.1:8000/docs

![截屏2026-06-07 00.15.24](../../../../截屏2026-06-07 00.15.24.png)

##二、Request(请求参数)

###1.路径参数

路径参数通过URL路径传递，在函数参数中直接定义即可。

```py	
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(item_id):
    return {"item_id": item_id}
```

**路径参数-类型注解Path**

使用**Path**进行类型注解和参数约束

```python
from fastapi import FastAPI, Path

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(
    item_id: int = Path(..., title="物品ID", ge=1, le=100),
    description: str = "物品描述"
):
    return {"item_id": item_id}
```



**常用参数**

- **ge**(greater than or equal) - 大于等于
- **le**(less than or equal) - 小于等于
- **gt**(greater than) - 大于
- **lt**(less than) - 小于
- min_length - 最小长度
- max_length - 最大长度
- **title** - 参数标题
- **description** - 参数描述

---

### 2.查询参数

查询参数通过URL的query string传递(`key=value`)，在函数参数中定义即可。

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/items/")
def read_items(skip: int = 0, limit: int = 10):
    return {"skip": skip, "limit": limit}
```

示例请求：

```python
GET /items/?skip=0&limit=10
```

查询参数 - 类型注解**Query**













![截屏2026-06-07 00.15.24](../../../../截屏2026-06-07 00.15.24.png)
