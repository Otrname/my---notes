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

<img width="779" height="434" alt="截屏2026-06-07 00 15 24" src="https://github.com/user-attachments/assets/42a93377-f561-40c1-b4d4-ab4133f482a7" />



## 二、Request(请求参数)

### 1.路径参数

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

<img width="779" height="434" alt="截屏2026-06-07 00 15 24" src="https://github.com/user-attachments/assets/c2e86f85-2340-4a4a-b110-a7fedb300929" />


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
使用`Query`进行类型注解和参数约束，用法与`Path`一样：
```python
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/items/")
def read_items(
    q: str = Query(None, min_length=3, max_length=50),
    skip: int = Query(0, ge=0),
    limit: int = Query(10, le=100)
):
    return {"q": q, "skip": skip, "limit": limit}
```
---
三、请求体参数
---
请求体参数通过Http请求体传递，通常使用JSON格式，需要定义Pydantic模型。
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None

@app.post("/items/")
def create_item(item: Item):
    return item
```
**示例**
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None

@app.post("/items/")
async def create_item(item: Item):
    """
    创建物品
    """
    item_dict = item.dict()
    if item.tax:
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    return item_dict
```
**示例请求：**
```python
POST /items/
{
    "name": "商品名称",
    "description": "商品描述",
    "price": 99.99,
    "tax": 10.0
}
```
请求体参数-类型注解Field
使用`Field`对请求体参数进行约束和验证：
```python
from fastapi import FastAPI
from pydantic import BaseModel, Field

app = FastAPI()

class Item(BaseModel):
    name: str = Field(..., min_length=1, max_length=50)
    description: str = Field(None, max_length=300)
    price: float = Field(..., gt=0, description="价格必须大于0")
    tax: float = Field(None, ge=0)

    class Config:
        schema_extra = {
            "example": {
                "name": "商品名称",
                "description": "商品描述",
                "price": 99.99,
                "tax": 10.0
            }
        }

@app.post("/items/")
def create_item(item: Item):
    return item
```
## 四、请求与响应
```html
客户端请求 → FastAPI 路由匹配 → 参数解析 → 业务处理 → 返回响应 → 客户端接收
```

## 五、Response（响应）
**FastAPI内置响应类型**
FastAPI默认响应类型是`JSONReponse`。如果需要返会非JSON数据（如HTML、文件流），FastAPI提供了丰富的响应类型：

| 响应类型 | 说明 |
| :--- | :--- |
| `JSONResponse` | JSON 格式响应（默认） |
| `HTMLResponse` | HTML 格式响应 |
| `PlainTextResponse` | 纯文本响应 |
| `RedirectResponse` | 重定向响应 |
| `StreamingResponse` | 流式响应（文件下载） |
| `FileResponse` | 文件响应 |
| `ORJSONResponse` | 高性能 JSON 响应 |

响应类型设置
```python
from fastapi import FastAPI
from fastapi.responses import JSONResponse, HTMLResponse

app = FastAPI()

@app.get("/json")
def get_json():
    return JSONResponse(content={"message": "Hello"})

@app.get("/html", response_class=HTMLResponse)
def get_html():
    return "<h1>Hello, World!</h1>"
```
示例： 响应`HTML`格式
```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/", response_class=HTMLResponse)
async def read_root():
    html_content = """
    <html>
        <head>
            <title>FastAPI</title>
        </head>
        <body>
            <h1>Hello, FastAPI!</h1>
        </body>
    </html>
    """
    return html_content
```
示例：响应文件格式
```python
from fastapi import FastAPI
from fastapi.responses import FileResponse

app = FastAPI()

@app.get("/download")
async def download_file():
    return FileResponse(
        path="file.pdf",
        filename="download.pdf",
        media_type="application/pdf"
    )
```
---
自定义响应(自己写个类）
```python
from fastapi import FastAPI, Response
from fastapi.responses import JSONResponse

app = FastAPI()

class CustomResponse(JSONResponse):
    def render(self, content):
        # 自定义响应头
        self.headers["X-Custom-Header"] = "CustomValue"
        return super().render(content)

@app.get("/custom", response_class=CustomResponse)
def get_custom():
    return {"message": "Custom Response"}
```
异常处理
使用`HTTPExcption`抛出异常响应
```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

items = {"foo": "The Foo Wrestlers"}

@app.get("/items/{item_id}")
def read_item(item_id: str):
    if item_id not in items:
        raise HTTPException(
            status_code=404,
            detail="Item not found",
            headers={"X-Error": "There goes my error"}
        )
    return {"item": items[item_id]}
```
自定义异常处理器：
```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse

app = FastAPI()

class UnicornException(Exception):
    def __init__(self, name: str):
        self.name = name

@app.exception_handler(UnicornException)
async def unicorn_exception_handler(request: Request, exc: UnicornException):
    return JSONResponse(
        status_code=418,
        content={"message": f"Oops! {exc.name} did something. There goes a rainbow..."}
    )

@app.get("/unicorns/{name}")
def read_unicorn(name: str):
    if name == "yolo":
        raise UnicornException(name=name)
    return {"unicorn_name": name}
```









