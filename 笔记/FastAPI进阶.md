# Fast API进阶

## 一、中间件

中间件（Middleware）是一个在每次请求进入 **FastAPI** 应用时都会被执行的函数。 它在请求到达实际的路径操作（路由处理函数）之前运行，并且在响应返回给客户端之前再运行一次。

![截屏2026-06-07 20.35.25](../../../../截屏2026-06-07 20.35.25.png)

中间件的作用：为每个请求添加统一的处理逻辑（记录日志、身份认证、跨域、设置响应头、性能监控等）

中间件：函数的顶部使用装饰器 @app.middleware("http")

⚠️**注意： **中间件的执行顺序：`自下而上`

**代码示例：**

```python
from fastapi import FastAPI

# 创建 FatsAPI 实例
app = FastAPI()


# request: 请求对象
# call_next: 调用下一个中间件或目标函数
@app.middleware("http")
async def middleware1(request, call_next):
    print("中间件1 start")
    response = await call_next(request)
    print("中间件1 end")
    return response


@app.middleware("http")
async def middleware1(request, call_next):
    print("中间件2 start")
    response = await call_next(request)
    print("中间件2 end")
    return response


@app.get("/")
async def root():
    return {"message": "Hello World"}

```

## 二、依赖注入

使用依赖注入系统来共享通用逻辑，避免代码重复 

依赖项：可重用的组件（函数/类），负责提供某种功能或数据。 

注入：FastAPI 自动帮你调用依赖项，并将结果"注入"到路径操作函数中。

**优点：**

- 代码复用：一次编写，多处使用 

- 解耦：业务逻辑与基础设施代码分离 

- 易于测试：轻松地用模拟依赖替换真实依赖进行测试

![截屏2026-06-07 21.34.10](../../../../截屏2026-06-07 21.34.10.png)

**代码示例：**

```python
from fastapi import FastAPI, Query, Depends

# 创建 FatsAPI 实例
app = FastAPI()

# 依赖项
async def common_parameters(
        skip: int = Query(0, age=0),
        limit: int = Query(10, le=60)
):
    return {"skip": skip, "limit": limit}

# 依赖注入
@app.get("/news/new_list")
async def get_news_list(commons=Depends(common_parameters)):
    return commons
```

