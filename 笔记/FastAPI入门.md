FastAPI入门

## 第一步（分步概括）

### 步骤 1：导入 `FastAPI`

```py
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```

**`FastAPI` 是一个为你的 API 提供了所有功能的 Python 类**。

### 步骤 2：创建一个 `FastAPI`「实例」

这里的变量 `app` 会是 `FastAPI` 类的一个「实例」。这个实例将是创建你所有 API 的主要交互对象。

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```

### 步骤 3：创建一个*路径操作*

这里的「路径」指的是 URL 中从第一个 `/` 起的后半部分。

所以，在一个这样的 URL 中：

```python
https://example.com/items/foo
```

路径会是：

```python
/items/foo
```

**操作**

这里的「操作」指的是一种 HTTP「方法」。

下列之一：

- `POST`
- `GET`
- `PUT`
- `DELETE`

...以及更少见的几种：

- `OPTIONS`
- `HEAD`
- `PATCH`
- `TRACE`

在 HTTP 协议中，你可以使用以上的其中一种（或多种）「方法」与每个路径进行通信。

------

在开发 API 时，你通常使用特定的 HTTP 方法去执行特定的行为。

通常使用：

- `POST`：创建数据。
- `GET`：读取数据。
- `PUT`：更新数据。
- `DELETE`：删除数据。

因此，在 OpenAPI 中，每一个 HTTP 方法都被称为「操作」。

我们也打算称呼它们为「操作」。

#### 定义一个路径操作装饰器

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/first-steps/#__tabbed_4_1)

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```

`@app.get("/")` 在例子中，这个装饰器告诉 **FastAPI** 位于其下方的函数对应着**路径** `/` 加上 `get` **操作**。它是一个「**路径操作装饰器**」。告诉 **FastAPI** 在它下方的函数负责处理如下访问请求：

- 请求路径为 `/`

- 使用 `get` 操作

你也可以使用其他的操作：

  - `@app.post()`
  - `@app.put()`
  - `@app.delete()`

  以及更少见的：

  - `@app.options()`
  - `@app.head()`
  - `@app.patch()`
  - `@app.trace()`

### 步骤 4：定义路径操作函数

这是我们的「**路径操作函数**」：

- **路径**：是 `/`。
- **操作**：是 `get`。
- **函数**：是位于「装饰器」下方的函数（位于 `@app.get("/")` 下方）。

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```

这是一个 Python 函数。

每当 **FastAPI** 接收一个使用 `GET` 方法访问 URL「`/`」的请求时这个函数会被调用。

在这个例子中，它是一个 `async` 函数。

------

你也可以将其定义为常规函数而不使用 `async def`:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def root():
    return {"message": "Hello World"}
```

### 步骤 5：返回内容

```
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}
```



你可以返回一个 `dict`、`list`，像 `str`、`int` 一样的单个值，等等。

你还可以返回 Pydantic 模型（稍后你将了解更多）。

还有许多其他将会自动转换为 JSON 的对象和模型（包括 ORM 对象等）。尝试下使用你最喜欢的一种，它很有可能已经被支持。

### 总结

- 导入 `FastAPI`。
- 创建一个 `app` 实例。
- 编写一个**路径操作装饰器**，如 `@app.get("/")`。
- 定义一个**路径操作函数**，如 `def root(): ...`。
- 使用命令 `fastapi dev` 运行开发服务器。

## 路径参数

你可以使用与 Python 字符串格式化相同的语法声明路径“参数”或“变量”：



**`代码示例`：**

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id):
    return {"item_id": item_id}
```

路径参数 `item_id` 的值会作为参数 `item_id` 传递给你的函数。

### 声明路径参数的类型

---

类型声明将为函数提供错误检查、代码补全等编辑器支持。



使用 Python 标准类型注解，声明路径操作函数中路径参数的类型：

**代码示例：**

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

本例把 `item_id` 的类型声明为 `int`。

### 数据转换

---

运行示例并访问 <http://127.0.0.1:8000/items/3>，返回的响应如下：

```python
{"item_id":3}
```

### 数据校验

---

通过浏览器访问 <http://127.0.0.1:8000/items/foo>，接收如下 HTTP 错误信息：

```python
{
  "detail": [
    {
      "type": "int_parsing",
      "loc": [
        "path",
        "item_id"
      ],
      "msg": "Input should be a valid integer, unable to parse string as an integer",
      "input": "foo"
    }
  ]
}
```

这是因为路径参数 `item_id` 的值（`"foo"`）的类型不是 `int`。

### 文档

访问 <http://127.0.0.1:8000/docs>，查看自动生成的交互式 API 文档：

![截屏2026-06-08 19.19.13](images/截屏2026-06-08 19.19.13.png)

 **基于标准的好处，备选文档**

---

 使用 [OpenAPI](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.1.0.md) 生成概图，所以能兼容很多工具。

因此，**FastAPI** 还内置了 ReDoc 生成的备选 API 文档，可在此查看 <http://127.0.0.1:8000/redoc>：

![截屏2026-06-08 19.19.13](images/截屏2026-06-08 19.19.13-0923532.png)

同样，还有很多兼容工具，包括多种语言的代码生成工具。

### Pydantic

FastAPI 充分地利用了 [Pydantic](https://docs.pydantic.dev/) 的优势，用它在后台校验数据。众所周知，Pydantic 擅长的就是数据校验。

同样，`str`、`float`、`bool` 以及很多复合数据类型都可以使用类型声明。

### 顺序很重要

---

有时，*路径操作*中的路径是写死的。

比如要使用 `/users/me` 获取当前用户的数据。

然后还要使用 `/users/{user_id}`，通过用户 ID 获取指定用户的数据。

由于*路径操作*是按顺序依次运行的，因此，一定要在 `/users/{user_id}` 之前声明 `/users/me` ：



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

否则，`/users/{user_id}` 将匹配 `/users/me`，FastAPI 会**认为**正在接收值为 `"me"` 的 `user_id` 参数,底下的那个 `@app.get("/users/me")` 永远被死死地压在下面，**变成了一段永远无法触发的死代码**。

同样，你不能重复定义一个路径操作：



```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/users")
async def read_users():
    return ["Rick", "Morty"]


@app.get("/users")
async def read_users2():
    return ["Bean", "Elfo"]
```

由于路径首先匹配，始终会使用第一个定义的。



### 预设值

---

路径操作使用 Python 的 `Enum` 类型接收预设的路径参数。



#### 创建 `Enum（枚举）` 类

---

导入 `Enum` 并创建继承自 `str` 和 `Enum` 的子类。

通过从 `str` 继承，API 文档就能把值的类型定义为**字符串**，并且能正确渲染。

然后，创建包含固定值的类属性，这些固定值是可用的有效值：



```python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```



#### 声明路径参数

---

使用 Enum 类（`ModelName`）创建使用类型注解的路径参数：



```python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```



#### 查看文档

API 文档会显示预定义路径参数的可用值：

![截屏2026-06-08 20.06.47](images/截屏2026-06-08 20.06.47.png)

### 使用 Python 枚举

路径参数的值是一个枚举成员。

##### 比较枚举成员

可以将其与枚举类 `ModelName` 中的枚举成员进行比较：



```python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```



##### 获取枚举值

使用 `model_name.value` 或通用的 `your_enum_member.value` 获取实际的值（本例中为 `str`）：

```python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```



**⚠️提示**

> 使用 `ModelName.lenet.value` 也能获取值 `"lenet"`。

##### 返回枚举成员

即使嵌套在 JSON 请求体里（例如，`dict`），也可以从路径操作返回枚举成员。

返回给客户端之前，会把枚举成员转换为对应的值（本例中为字符串）：



```python
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}
```



客户端中的 JSON 响应如下：

```python
{
  "model_name": "alexnet",
  "message": "Deep Learning FTW!"
}
```

### 包含路径的路径参数

假设路径操作的路径为 `/files/{file_path}`。

但需要 `file_path` 中也包含路径，比如，`home/johndoe/myfile.txt`。

此时，该文件的 URL 是这样的：`/files/home/johndoe/myfile.txt`。

#### OpenAPI 支持

OpenAPI 不支持声明包含路径的路径参数，因为这会导致测试和定义更加困难。

不过，仍可使用 Starlette 内置工具在 **FastAPI** 中实现这一功能。

而且不影响文档正常运行，但是不会添加该参数包含路径的说明。

#### 路径转换器

直接使用 Starlette 的选项声明包含路径的路径参数：

```
/files/{file_path:path}
```

本例中，参数名为 `file_path`，结尾部分的 `:path` 说明该参数应匹配路径。

用法如下：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/path-params/#__tabbed_10_1)

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/files/{file_path:path}")
async def read_file(file_path: str):
    return {"file_path": file_path}
```



**⚠️提示**

注意，包含 `/home/johndoe/myfile.txt` 的路径参数要以斜杠（`/`）开头。

本例中的 URL 是 `/files//home/johndoe/myfile.txt`。注意，`files` 和 `home` 之间要使用双斜杠（`//`）。



### 小结

通过简短、直观的 Python 标准类型声明，**FastAPI** 可以获得：

- 编辑器支持：错误检查，代码自动补全等
- 数据 "解析"
- 数据校验
- API 注解和自动文档

只需要声明一次即可。

这可能是除了性能以外，**FastAPI** 与其它框架相比的主要优势。



---

## 查询参数

声明的参数不是路径参数时，路径操作函数会把该参数自动解释为“查询”参数。

```python
 from fastapi import FastAPI

app = FastAPI()

fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]


@app.get("/items/")
async def read_item(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]
```

查询字符串是键值对的集合，这些键值对位于 URL 的 `?` 之后，以 `&` 分隔。

例如，以下 URL 中：

```
http://127.0.0.1:8000/items/?skip=0&limit=10
```

...查询参数为：

- `skip`：值为 `0`
- `limit`：值为 `10`

这些值都是 URL 的组成部分，因此，它们的类型“本应”是字符串。

但声明 Python 类型（上例中为 `int`）之后，这些值就会转换为声明的类型，并进行类型校验。

所有应用于路径参数的流程也适用于查询参数：

- （显而易见的）编辑器支持
- 数据"解析"
- 数据校验
- 自动文档

---



### 默认值

查询参数不是路径的固定内容，它是可选的，还支持默认值。

上例用 `skip=0` 和 `limit=10` 设定默认值。

访问 URL：

```html
http://127.0.0.1:8000/items/
```

与访问以下地址相同：

```html 
http://127.0.0.1:8000/items/?skip=0&limit=10
```

但如果访问：

```html
http://127.0.0.1:8000/items/?skip=20
```

查询参数的值就是：

- `skip=20`：在 URL 中设定的值
- `limit=10`：使用默认值

---



### 可选参数

同理，把默认值设为 `None` 即可声明可选的查询参数：

```
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: str, q: str | None = None):
    if q:
        return {"item_id": item_id, "q": q}
    return {"item_id": item_id}
```



本例中，查询参数 `q` 是可选的，默认值为 `None`。



> **⚠️检查**
>
> 注意，**FastAPI** 可以识别出 `item_id` 是路径参数，`q` 不是路径参数，而是查询参数。



---



### 查询参数类型转换

参数还可以声明为 `bool` 类型，FastAPI 会自动转换参数类型：



```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: str, q: str | None = None, short: bool = False):
    item = {"item_id": item_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```



本例中，访问：

```vue
http://127.0.0.1:8000/items/foo?short=1
```

或

```vue
http://127.0.0.1:8000/items/foo?short=True
```

或

```vue
http://127.0.0.1:8000/items/foo?short=on
```

或

```vue
http://127.0.0.1:8000/items/foo?short=yes
```

或其它任意大小写形式（大写、首字母大写等），函数接收的 `short` 参数都是布尔值 `True`。否则为 `False`。





---



### 多个路径和查询参数

**FastAPI** 可以识别同时声明的多个路径参数和查询参数。

而且声明查询参数的顺序并不重要。

FastAPI 通过参数名进行检测：



```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/users/{user_id}/items/{item_id}")
async def read_user_item(
    user_id: int, item_id: str, q: str | None = None, short: bool = False
):
    item = {"item_id": item_id, "owner_id": user_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item
```



---

### 必选查询参数

为不是路径参数的参数声明默认值（至此，仅有查询参数），该参数就不是必选的了。

如果只想把参数设为可选，但又不想指定参数的值，则要把默认值设为 `None`。

如果要把查询参数设置为必选，就不要声明默认值：



```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_user_item(item_id: str, needy: str):
    item = {"item_id": item_id, "needy": needy}
    return item
```



这里的查询参数 `needy` 是类型为 `str` 的必选查询参数。

在浏览器中打开如下 URL：

```vue
http://127.0.0.1:8000/items/foo-item
```

...因为路径中没有必选参数 `needy`，返回的响应中会显示如下错误信息：

```python
{
  "detail": [
    {
      "type": "missing",
      "loc": [
        "query",
        "needy"
      ],
      "msg": "Field required",
      "input": null
    }
  ]
}
```

`needy` 是必选参数，因此要在 URL 中设置值：

```vue
http://127.0.0.1:8000/items/foo-item?needy=sooooneedy
```

...这样就正常了：

```python
{
    "item_id": "foo-item",
    "needy": "sooooneedy"
}
```

当然，把一些参数定义为必选，为另一些参数设置默认值，再把其它参数定义为可选，这些操作都是可以的：



```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_user_item(
    item_id: str, needy: str, skip: int = 0, limit: int | None = None
):
    item = {"item_id": item_id, "needy": needy, "skip": skip, "limit": limit}
    return item
```





本例中有 3 个查询参数：

- `needy`，必选的 `str` 类型参数

- `skip`，默认值为 `0` 的 `int` 类型参数

- `limit`，可选的 `int` 类型参数


> **🔥提示**
>
> 还可以像在[路径参数](https://fastapi.tiangolo.com/zh/tutorial/path-params/#predefined-values)中那样使用 `Enum`。



---

## 请求体

当你需要从客户端（比如浏览器）向你的 API 发送数据时，会把它作为`请求体`发送。

`请求体`是客户端发送给你的 API 的数据。`响应体`是你的 API 发送给客户端的数据。

你的 API 几乎总是需要发送`响应体`。但客户端不一定总是要发送`请求体`，有时它们只请求某个路径，可能带一些查询参数，但不会发送请求体。

使用 [Pydantic](https://docs.pydantic.dev/) 模型来声明`请求体`，能充分利用它的功能和优点。



---

### 导入 Pydantic 的 `BaseModel`

从 `pydantic` 中导入 `BaseModel`：



```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.post("/items/")
async def create_item(item: Item):
    return item
```

### 创建数据模型

把数据模型声明为继承 `BaseModel` 的类。

使用 Python 标准类型声明所有属性：



```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.post("/items/")
async def create_item(item: Item):
    return item
```



与声明查询参数一样，包含默认值的模型属性是可选的，否则就是必选的。把默认值设为 `None` 可使其变为可选。

例如，上述模型声明如下 JSON "object"（即 Python `dict`）：

```python
{
    "name": "Foo",
    "description": "An optional description",
    "price": 45.2,
    "tax": 3.5
}
```

...由于 `description` 和 `tax` 是可选的（默认值为 `None`），下面的 JSON "object" 也有效：

```python
{
    "name": "Foo",
    "price": 45.2
}
```



---



### 声明为参数

使用与声明路径和查询参数相同的方式，把它添加至*路径操作*：



```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.post("/items/")
async def create_item(item: Item):
    return item
```



...并把其类型声明为你创建的模型 `Item`。

### 结果

仅使用这些 Python 类型声明，**FastAPI** 就可以：

- 以 JSON 形式读取请求体。

- （在必要时）把请求体转换为对应的类型。

- 校验数据。

  - 数据无效时返回清晰的错误信息，并指出错误数据的确切位置和内容。

- 把接收的数据赋值给参数`item`

  	- 因为你把函数中的参数类型声明为 `Item`，所以还能获得所有属性及其类型的编辑器支持（补全等）。

- 为你的模型生成 [JSON Schema](https://json-schema.org/) 定义，如果对你的项目有意义，还可以在其他地方使用它们。

- 这些 schema 会成为生成的 OpenAPI Schema 的一部分，并被自动文档的 UIs 使用。



---

### 自动文档

你的模型的 JSON Schema 会成为生成的 OpenAPI Schema 的一部分，并显示在交互式 API 文档中;并且，还会用于需要它们的每个*路径操作*的 API 文档中。



----

### 编辑器支持

在编辑器中，函数内部你会在各处得到类型提示与补全（如果接收的不是 Pydantic 模型，而是 `dict`，就不会有这样的支持），还支持检查错误的类型操作。

![截屏2026-06-10 10.54.28](images/截屏2026-06-10 10.54.28.png)

这并非偶然，整个框架都是围绕这种设计构建的。

并且在设计阶段、实现之前就进行了全面测试，以确保它能在所有编辑器中正常工作。

我们甚至对 Pydantic 本身做了一些改动以支持这些功能。

上面的截图来自 [Visual Studio Code](https://code.visualstudio.com/)。

但使用 [PyCharm](https://www.jetbrains.com/pycharm/) 和大多数其他 Python 编辑器，你也会获得相同的编辑器支持：



> 🔥提示
>
> 如果你使用 [PyCharm](https://www.jetbrains.com/pycharm/) 作为编辑器，可以使用 [Pydantic PyCharm 插件](https://github.com/koxudaxi/pydantic-pycharm-plugin/)。
>
> 如果你使用 [PyCharm](https://www.jetbrains.com/pycharm/) 作为编辑器，可以使用 [Pydantic PyCharm 插件](https://github.com/koxudaxi/pydantic-pycharm-plugin/)。
>
> 它能改进对 Pydantic 模型的编辑器支持，包括：
>
> - 自动补全
> - 类型检查
> - 代码重构
> - 查找
> - 代码审查



---

### 使用模型

在*路径操作*函数内部直接访问模型对象的所有属性：

```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.post("/items/")
async def create_item(item: Item):
    item_dict = item.model_dump()              #将Pydantic的Item对象转换回标准的Python字典
    if item.tax is not None:
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    return item_dict
```



---

### 请求体 + 路径参数

可以同时声明路径参数和请求体。

**FastAPI** 能识别与**路径参数**匹配的函数参数应该**从路径中获取**，而声明为 Pydantic 模型的函数参数应该**从请求体中获取**。



```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    return {"item_id": item_id, **item.model_dump()}
```





---

### 请求体 + 路径 + 查询参数

也可以同时声明**请求体**、**路径**和**查询**参数。

**FastAPI** 会分别识别它们，并从正确的位置获取数据。



```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


app = FastAPI()


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item, q: str | None = None):
    result = {"item_id": item_id, **item.model_dump()}
    if q:
        result.update({"q": q})
    return result
```



函数参数按如下规则进行识别：

- 如果该参数也在**路径**中声明了，它就是路径参数。
- 如果该参数是（`int`、`float`、`str`、`bool` 等）**单一类型**，它会被当作**查询**参数。
- 如果该参数的类型声明为 **Pydantic 模型**，它会被当作请求**体**。





---





## 查询参数和字符串校验

**FastAPI** 允许你为参数声明额外的信息和校验。

让我们以下面的应用为例：

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/")
async def read_items(q: str | None = None):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

### 额外校验

我们打算添加约束：即使 `q` 是可选的，但只要提供了该参数，**其长度不能超过 50 个字符**。

#### 导入 `Query` 和 `Annotated`

为此，先导入：

- 从 `fastapi` 导入 `Query`
- 从 `typing` 导入 `Annotated`



```
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str | None, Query(max_length=50)] = None):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

### 在 `q` 参数的类型中使用 `Annotated

还记得我之前在[Python 类型简介](https://fastapi.tiangolo.com/zh/python-types/#type-hints-with-metadata-annotations)中说过可以用 `Annotated` 给参数添加元数据吗？

现在正是与 FastAPI 搭配使用它的时候。🚀

我们之前的类型标注是：

```python
q: str | None = None
```

我们要做的是用 `Annotated` 把它包起来，变成：

```python
q: Annotated[str | None] = None
```

这两种写法含义相同，`q` 是一个可以是 `str` 或 `None` 的参数，默认是 `None`。

现在进入更有趣的部分。🎉

### 在 `q` 的 `Annotated` 中添加 `Query`

有了 `Annotated` 之后，我们就可以放入更多信息（本例中是额外的校验）。在 `Annotated` 中添加 `Query`，并把参数 `max_length` 设为 `50`：



```
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str | None, Query(max_length=50)] = None):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```



注意默认值依然是 `None`，所以该参数仍是可选的。

但现在把 `Query(max_length=50)` 放到 `Annotated` 里，我们就在告诉 FastAPI，这个值需要**额外校验**，最大长度为 50 个字符。😎

提示

这里用的是 `Query()`，因为这是一个**查询参数**。稍后我们还会看到 `Path()`、`Body()`、`Header()` 和 `Cookie()`，它们也接受与 `Query()` 相同的参数。

FastAPI 现在会：

- 对数据进行**校验**，确保最大长度为 50 个字符
- 当数据无效时向客户端展示**清晰的错误**
- 在 OpenAPI 模式的*路径操作*中**记录**该参数（因此会出现在**自动文档 UI** 中）

---

### 添加更多校验

你还可以添加 `min_length` 参数：

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Annotated[str | None, Query(min_length=3, max_length=50)] = None,
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

---

### 添加正则表达式

你可以定义一个参数必须匹配的 正则表达式 `pattern`：



```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Annotated[
        str | None, Query(min_length=3, max_length=50, pattern="^fixedquery$")
    ] = None,
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

这个特定的正则表达式通过以下规则检查接收到的参数值：

- `^`：必须以接下来的字符开头，前面没有其他字符。
- `fixedquery`：值必须精确等于 `fixedquery`。
- `$`：到此结束，在 `fixedquery` 之后没有更多字符。

如果你对这些**「正则表达式」**概念感到迷茫，不必担心。对很多人来说这都是个难点。你仍然可以在不使用正则表达式的情况下做很多事情。

现在你知道了，一旦需要时，你可以在 **FastAPI** 中直接使用它们。



---

### 默认值

当然，你也可以使用 `None` 以外的默认值。

假设你想要声明查询参数 `q` 的 `min_length` 为 `3`，并且默认值为 `"fixedquery"`：

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str, Query(min_length=3)] = "fixedquery"):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

> ⚠️注意
>
> 任何类型的默认值（包括 `None`）都会让该参数变为可选（非必填）。





---

### 必填参数

当我们不需要声明更多校验或元数据时，只需不声明默认值就可以让查询参数 `q` 成为必填参数，例如：

```python
q: str
```

而不是：

```python
q: str | None = None
```

但现在我们用 `Query` 来声明它，例如：

```python
q: Annotated[str | None, Query(min_length=3)] = None
```

因此，在使用 `Query` 的同时需要把某个值声明为必填时，只需不声明默认值：

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str, Query(min_length=3)]):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```



---

#### 必填，但可以为 `None`

你可以声明一个参数可以接收 `None`，但它仍然是必填的。这将强制客户端必须发送一个值，即使该值是 `None`。

为此，你可以声明 `None` 是有效类型，但不声明默认值：

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str | None, Query(min_length=3)]):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```





---

### 查询参数列表 / 多个值

当你用 `Query` 显式地定义查询参数时，你还可以声明它接收一个值列表，换句话说，接收多个值。

例如，要声明一个可在 URL 中出现多次的查询参数 `q`，你可以这样写：

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[list[str] | None, Query()] = None):
    query_items = {"q": q}
    return query_items
```



然后，访问如下 URL：

```python
http://localhost:8000/items/?q=foo&q=bar
```

你会在*路径操作函数*的*函数参数* `q` 中以一个 Python `list` 的形式接收到多个 `q` *查询参数* 的值（`foo` 和 `bar`）。

因此，该 URL 的响应将会是：

```
{
  "q": [
    "foo",
    "bar"
  ]
}
```



> 🔥 提示
>
> 要声明类型为 `list` 的查询参数（如上例），你需要显式地使用 `Query`，否则它会被解释为请求体。

交互式 API 文档会相应更新，以支持多个值：

![截屏2026-06-10 16.12.27](images/截屏2026-06-10 16.12.27.png)

#### 具有默认值的查询参数列表 / 多个值

你还可以定义在没有给定值时的默认 `list`：

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[list[str], Query()] = ["foo", "bar"]):
    query_items = {"q": q}
    return query_items
```



如果你访问：

```python
http://localhost:8000/items/
```



`q` 的默认值将为：`["foo", "bar"]`，你的响应会是：

```
{
  "q": [
    "foo",
    "bar"
  ]
}
```



##### 只使用 `list`

你也可以直接使用 `list`，而不是 `list[str]`：

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[list, Query()] = []):
    query_items = {"q": q}
    return query_items
```



> ⚠️ 注意
>
> 请记住，在这种情况下 FastAPI 不会检查列表的内容。
>
> 例如，`list[int]` 会检查（并记录到文档）列表的内容必须是整数。但仅用 `list` 不会。



---

### 声明更多元数据

你可以添加更多有关该参数的信息。

这些信息会包含在生成的 OpenAPI 中，并被文档用户界面和外部工具使用。

你可以添加 `title`，以及`description`

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Annotated[
        str | None,
        Query(
            title="Query string",
            description="Query string for the items to search in the database that have a good match",
            min_length=3,
        ),
    ] = None,
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```





---

### 别名参数

假设你想要参数名为 `item-query`。

像这样：

```python
http://127.0.0.1:8000/items/?item-query=foobaritems
```

但 `item-query` 不是有效的 Python 变量名。

最接近的有效名称是 `item_query`。

但你仍然需要它在 URL 中就是 `item-query`...

这时可以用 `alias` 参数声明一个别名，FastAPI 会用该别名在 URL 中查找参数值：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/query-params-str-validations/#__tabbed_28_1)

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(q: Annotated[str | None, Query(alias="item-query")] = None):  #````
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```







---

### 弃用参数

现在假设你不再喜欢这个参数了。

由于还有客户端在使用它，你不得不保留一段时间，但你希望文档清楚地将其展示为已弃用。

那么将参数 `deprecated=True` 传给 `Query`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/query-params-str-validations/#__tabbed_30_1)

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: Annotated[
        str | None,
        Query(
            alias="item-query",
            title="Query string",
            description="Query string for the items to search in the database that have a good match",
            min_length=3,
            max_length=50,
            pattern="^fixedquery$",
            deprecated=True,
        ),
    ] = None,
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```



文档将会像下面这样展示它：

![截屏2026-06-10 16.43.21](images/截屏2026-06-10 16.43.21.png)

### 从 OpenAPI 中排除参数

要把某个查询参数从生成的 OpenAPI 模式中排除（从而也不会出现在自动文档系统中），将 `Query` 的参数 `include_in_schema` 设为 `False`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/query-params-str-validations/#__tabbed_32_1)

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    hidden_query: Annotated[str | None, Query(include_in_schema=False)] = None,
):
    if hidden_query:
        return {"hidden_query": hidden_query}
    else:
        return {"hidden_query": "Not found"}
```



---

### 自定义校验

有些情况下你需要做一些无法通过上述参数完成的**自定义校验**。

在这些情况下，你可以使用**自定义校验函数**，该函数会在正常校验之后应用（例如，在先校验值是 `str` 之后）。

你可以在 `Annotated` 中使用 [Pydantic 的 `AfterValidator`](https://docs.pydantic.dev/latest/concepts/validators/#field-after-validator) 来实现。



例如，这个自定义校验器会检查条目 ID 是否以 `isbn-`（用于 ISBN 书号）或 `imdb-`（用于 IMDB 电影 URL 的 ID）开头：



```python
import random
from typing import Annotated

from fastapi import FastAPI
from pydantic import AfterValidator

app = FastAPI()

data = {
    "isbn-9781529046137": "The Hitchhiker's Guide to the Galaxy",
    "imdb-tt0371724": "The Hitchhiker's Guide to the Galaxy",
    "isbn-9781439512982": "Isaac Asimov: The Complete Stories, Vol. 2",
}


def check_valid_id(id: str):
    if not id.startswith(("isbn-", "imdb-")):
        raise ValueError('Invalid ID format, it must start with "isbn-" or "imdb-"')
    return id


@app.get("/items/")
async def read_items(
    id: Annotated[str | None, AfterValidator(check_valid_id)] = None,
):
    if id:
        item = data.get(id)
    else:
        id, item = random.choice(list(data.items()))
    return {"id": id, "name": item}
```



#### 理解这段代码

关键点仅仅是：在 `Annotated` 中使用带函数的 **AfterValidator**。不感兴趣可以跳过这一节。🤸

------

但如果你对这个具体示例好奇，并且还愿意继续看，这里有一些额外细节。

#### 字符串与 `value.startswith()`

注意到了吗？字符串的 `value.startswith()` 可以接收一个元组，它会检查元组中的每个值：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/query-params-str-validations/#__tabbed_35_1)

```python
# Code above omitted 👆

def check_valid_id(id: str):
    if not id.startswith(("isbn-", "imdb-")):
        raise ValueError('Invalid ID format, it must start with "isbn-" or "imdb-"')
    return id

# Code below omitted 👇
```



#### 一个随机条目

使用 `data.items()` 我们会得到一个包含每个字典项键和值的元组的 可迭代对象。

我们用 `list(data.items())` 把这个可迭代对象转换成一个真正的 `list`。

然后用 `random.choice()` 可以从该列表中获取一个**随机值**，也就是一个 `(id, name)` 的元组。它可能像 `("imdb-tt0371724", "The Hitchhiker's Guide to the Galaxy")` 这样。

接着我们把这个元组的**两个值**分别赋给变量 `id` 和 `name`。

所以，即使用户没有提供条目 ID，他们仍然会收到一个随机推荐。

...而我们把这些都放在**一行简单的代码**里完成。🤯 你不爱 Python 吗？🐍

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/query-params-str-validations/#__tabbed_37_1)

```
# Code above omitted 👆

@app.get("/items/")
async def read_items(
    id: Annotated[str | None, AfterValidator(check_valid_id)] = None,
):
    if id:
        item = data.get(id)
    else:
        id, item = random.choice(list(data.items()))
    return {"id": id, "name": item}
```



---

### 总结

你可以为参数声明额外的校验和元数据。

通用的校验和元数据：

- `alias`
- `title`
- `description`
- `deprecated`

字符串特有的校验：

- `min_length`
- `max_length`
- `pattern`

也可以使用 `AfterValidator` 进行自定义校验。

在这些示例中，你看到了如何为 `str` 值声明校验。

参阅下一章节，了解如何为其他类型（例如数值）声明校验。



----





## 路径参数和数值校验

与使用 `Query` 为查询参数声明更多的校验和元数据的方式相同，你也可以使用 `Path` 为路径参数声明相同类型的校验和元数据。

### 导入 `Path`

首先，从 `fastapi` 导入 `Path`，并导入 `Annotated`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/path-params-numeric-validations/#__tabbed_1_1)

```python 
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    item_id: Annotated[int, Path(title="The ID of the item to get")],
    q: Annotated[str | None, Query(alias="item-query")] = None,
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```



---

### 声明元数据

你可以声明与 `Query` 相同的所有参数。

例如，要为路径参数 `item_id` 声明 `title` 元数据值，你可以这样写：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/path-params-numeric-validations/#__tabbed_3_1)

```python
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    item_id: Annotated[int, Path(title="The ID of the item to get")],
    q: Annotated[str | None, Query(alias="item-query")] = None,
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```





> ⚠️ 
>
> 路径参数总是必需的，因为它必须是路径的一部分。即使你将其声明为 `None` 或设置了默认值，也不会产生任何影响，它依然始终是必需参数。



---

### 按需对参数排序

> 🔔
>
> 如果你使用 `Annotated`，这点可能不那么重要或必要。

假设你想要将查询参数 `q` 声明为必需的 `str`。

并且你不需要为该参数声明其他内容，所以实际上不需要用到 `Query`。

但是你仍然需要为路径参数 `item_id` 使用 `Path`。并且出于某些原因你不想使用 `Annotated`。

如果你将带有“默认值”的参数放在没有“默认值”的参数之前，Python 会报错。

不过你可以重新排序，让没有默认值的参数（查询参数 `q`）放在最前面。

对 **FastAPI** 来说这无关紧要。它会通过参数的名称、类型和默认值声明（`Query`、`Path` 等）来检测参数，而不关心顺序。

因此，你可以将函数声明为：

```python
from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(q: str, item_id: int = Path(title="The ID of the item to get")):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```

#### 使用 `Annotated` 更好

但请记住，如果你使用 `Annotated`，你就不会遇到这个问题，因为你没有使用 `Query()` 或 `Path()` 作为函数参数的默认值。

```python
from typing import Annotated

from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    q: str, item_id: Annotated[int, Path(title="The ID of the item to get")]
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```

---



### 数值校验：大于等于

使用 `Query` 和 `Path`（以及你稍后会看到的其他类）你可以声明数值约束。

在这里，使用 `ge=1` 后，`item_id` 必须是一个整数，值要「`g`reater than or `e`qual」1。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/path-params-numeric-validations/#__tabbed_13_1)

```python
from typing import Annotated

from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    item_id: Annotated[int, Path(title="The ID of the item to get", ge=1)], q: str
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```

### 数值校验：浮点数、大于和小于

数值校验同样适用于 `float` 值。

能够声明 `gt` 而不仅仅是 `ge` 在这里变得很重要。例如，你可以要求一个值必须大于 `0`，即使它小于 `1`。

因此，`0.5` 将是有效值。但是 `0.0` 或 `0` 不是。

对于 `lt` 也是一样的。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/path-params-numeric-validations/#__tabbed_17_1)

```python
from typing import Annotated

from fastapi import FastAPI, Path, Query

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    *,
    item_id: Annotated[int, Path(title="The ID of the item to get", ge=0, le=1000)],
    q: str,
    size: Annotated[float, Query(gt=0, lt=10.5)],
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    if size:
        results.update({"size": size})
    return results
```

----



### 总结

你能够以与[查询参数和字符串校验](https://fastapi.tiangolo.com/zh/tutorial/query-params-str-validations/)相同的方式使用 `Query`、`Path`（以及其他你还没见过的类）声明元数据和字符串校验。

而且你还可以声明数值校验：

- `gt`：大于（`g`reater `t`han）
- `ge`：大于等于（`g`reater than or `e`qual）
- `lt`：小于（`l`ess `t`han）
- `le`：小于等于（`l`ess than or `e`qual）





---

## 查询参数模型

如果你有一组具有相关性的**查询参数**，你可以创建一个 **Pydantic 模型**来声明它们。这将允许你在**多个地方**去**复用模型**，并且一次性为所有参数声明验证和元数据。😎

### 使用 Pydantic 模型的查询参数

在一个 **Pydantic 模型**中声明你需要的**查询参数**，然后将参数声明为 `Query`：

```python
from typing import Annotated, Literal

from fastapi import FastAPI, Query
from pydantic import BaseModel, Field

app = FastAPI()


class FilterParams(BaseModel):
    limit: int = Field(100, gt=0, le=100)
    offset: int = Field(0, ge=0)
    order_by: Literal["created_at", "updated_at"] = "created_at"
    tags: list[str] = []


@app.get("/items/")
async def read_items(filter_query: Annotated[FilterParams, Query()]):
    return filter_query
```



**FastAPI** 将会从请求的**查询参数**中**提取**出**每个字段**的数据，并将其提供给你定义的 Pydantic 模型。

### 查看文档

你可以在 `/docs` 页面的 UI 中查看查询参数：

![截屏2026-06-10 21.16.08](images/截屏2026-06-10 21.16.08.png)



---

### 禁止额外的查询参数

在一些特殊的使用场景中（可能不是很常见），你可能希望**限制**你要接收的查询参数。

你可以使用 Pydantic 的模型配置来 `forbid` 任何 `extra` 字段：

```python
from typing import Annotated, Literal

from fastapi import FastAPI, Query
from pydantic import BaseModel, Field

app = FastAPI()


class FilterParams(BaseModel):
    model_config = {"extra": "forbid"}

    limit: int = Field(100, gt=0, le=100)
    offset: int = Field(0, ge=0)
    order_by: Literal["created_at", "updated_at"] = "created_at"
    tags: list[str] = []


@app.get("/items/")
async def read_items(filter_query: Annotated[FilterParams, Query()]):
    return filter_query
```

假设有一个客户端尝试在**查询参数**中发送一些**额外的**数据，它将会收到一个**错误**响应。

例如，如果客户端尝试发送一个值为 `plumbus` 的 `tool` 查询参数，如：

```python
https://example.com/items/?limit=10&tool=plumbus
```

他们将收到一个**错误**响应，告诉他们查询参数 `tool` 是不允许的：

```python
{
    "detail": [
        {
            "type": "extra_forbidden",
            "loc": ["query", "tool"],
            "msg": "Extra inputs are not permitted",
            "input": "plumbus"
        }
    ]
}
```



### 总结

你可以使用 **Pydantic 模型**在 **FastAPI** 中声明**查询参数**。😎



> 🔥提示
>
> **剧透警告**：你也可以使用 Pydantic 模型来声明 cookie 和 headers，但你将在本教程的后面部分阅读到这部分内容。🤫







---

## 请求体 - 多个参数

既然我们已经知道了如何使用 `Path` 和 `Query`，下面让我们来了解一下请求体声明的更高级用法。



### 混合使用 `Path`、`Query` 和请求体参数

首先，毫无疑问地，你可以随意地混合使用 `Path`、`Query` 和请求体参数声明，**FastAPI** 会知道该如何处理。

你还可以通过将默认值设置为 `None` 来将请求体参数声明为可选参数：

```python
from typing import Annotated

from fastapi import FastAPI, Path
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(
    item_id: Annotated[int, Path(title="The ID of the item to get", ge=0, le=1000)],
    q: str | None = None,
    item: Item | None = None,
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    if item:
        results.update({"item": item})
    return results
```



> ⚠️
>
> 请注意，在这种情况下，将从请求体获取的 `item` 是可选的。因为它的默认值为 `None`。



---



### 多个请求体参数

在上面的示例中，*路径操作*将期望一个具有 `Item` 的属性的 JSON 请求体，就像：

```python
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2
}
```

但是你也可以声明多个请求体参数，例如 `item` 和 `user`：

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


class User(BaseModel):
    username: str
    full_name: str | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item, user: User):
    results = {"item_id": item_id, "item": item, "user": user}
    return results
```

在这种情况下，**FastAPI** 将注意到该函数中有多个请求体参数（两个 Pydantic 模型参数）。

因此，它将使用参数名称作为请求体中的键（字段名称），并期望一个类似于以下内容的请求体：

```python
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
    }
}
```



**FastAPI** 将自动对请求中的数据进行转换，因此 `item` 参数将接收指定的内容，`user` 参数也是如此。

它将执行对复合数据的校验，并且像现在这样为 OpenAPI 模式和自动化文档对其进行记录。





---





### 请求体中的单一值

与使用 `Query` 和 `Path` 为查询参数和路径参数定义额外数据的方式相同，**FastAPI** 提供了一个同等的 `Body`。

例如，为了扩展先前的模型，你可能决定除了 `item` 和 `user` 之外，还想在同一请求体中具有另一个键 `importance`。

如果你就按原样声明它，因为它是一个单一值，**FastAPI** 将假定它是一个查询参数。

但是你可以使用 `Body` 指示 **FastAPI** 将其作为请求体的另一个键进行处理。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-multiple-params/#__tabbed_4_1)

```
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


class User(BaseModel):
    username: str
    full_name: str | None = None


@app.put("/items/{item_id}")
async def update_item(
    item_id: int, item: Item, user: User, importance: Annotated[int, Body()]
):
    results = {"item_id": item_id, "item": item, "user": user, "importance": importance}
    return results
```

在这种情况下，**FastAPI** 将期望像这样的请求体：

```python
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

同样的，它将转换数据类型，校验，生成文档等。



---



### 多个请求体参数和查询参数

当然，除了请求体参数外，你还可以在任何需要的时候声明额外的查询参数。

由于默认情况下单一值会被解释为查询参数，因此你不必显式地添加 `Query`，你可以这样写：

```python
q: str | None = None
```

比如：

```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


class User(BaseModel):
    username: str
    full_name: str | None = None


@app.put("/items/{item_id}")
async def update_item(
    *,
    item_id: int,
    item: Item,
    user: User,
    importance: Annotated[int, Body(gt=0)],
    q: str | None = None,
):
    results = {"item_id": item_id, "item": item, "user": user, "importance": importance}
    if q:
        results.update({"q": q})
    return results
```



> 🔔
>
> `Body` 同样具有与 `Query`、`Path` 以及其他后面将看到的类完全相同的额外校验和元数据参数。





---

### 嵌入单个请求体参数

假设你只有一个来自 Pydantic 模型 `Item` 的请求体参数 `item`。

默认情况下，**FastAPI** 将直接期望这样的请求体。

但是，如果你希望它期望一个拥有 `item` 键并在值中包含模型内容的 JSON，就像在声明额外的请求体参数时所做的那样，则可以使用一个特殊的 `Body` 参数 `embed`：

```python
item: Item = Body(embed=True)
```

比如：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-multiple-params/#__tabbed_8_1)

```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Annotated[Item, Body(embed=True)]):
    results = {"item_id": item_id, "item": item}
    return results
```



在这种情况下，**FastAPI** 将期望像这样的请求体：

```python
{
    "item": {
        "name": "Foo",
        "description": "The pretender",
        "price": 42.0,
        "tax": 3.2
    }
}
```

而不是：

```python
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2
}
```

### 总结

你可以添加多个请求体参数到*路径操作函数*中，即使一个请求只能有一个请求体。

但是 **FastAPI** 会处理它，在函数中为你提供正确的数据，并在*路径操作*中校验并记录正确的模式。

你还可以声明将作为请求体的一部分所接收的单一值。

你还可以指示 **FastAPI** 在仅声明了一个请求体参数的情况下，将原本的请求体嵌入到一个键中。





---





## 请求体 - 字段

与在*路径操作函数*中使用 `Query`、`Path` 、`Body` 声明校验与元数据的方式一样，可以使用 Pydantic 的 `Field` 在 Pydantic 模型内部声明校验和元数据。

### 导入 `Field`

首先，从 Pydantic 中导入 `Field`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-fields/#__tabbed_1_1)

```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel, Field

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = Field(
        default=None, title="The description of the item", max_length=300
    )
    price: float = Field(gt=0, description="The price must be greater than zero")
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Annotated[Item, Body(embed=True)]):
    results = {"item_id": item_id, "item": item}
    return results
```



> ⚠️警告
>
> 注意，与从 `fastapi` 导入 `Query`，`Path`、`Body` 不同，要直接从 `pydantic` 导入 `Field` 。



### 声明模型属性

然后，使用 `Field` 定义模型的属性：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-fields/#__tabbed_3_1)

```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel, Field

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = Field(
        default=None, title="The description of the item", max_length=300
    )
    price: float = Field(gt=0, description="The price must be greater than zero")
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Annotated[Item, Body(embed=True)]):
    results = {"item_id": item_id, "item": item}
    return results
```

`Field` 的工作方式和 `Query`、`Path`、`Body` 相同，参数也相同。



> ✏️ 技术细节
>
> 实际上，`Query`、`Path` 以及你接下来会看到的其它对象，会创建公共 `Param` 类的子类的对象，而 `Param` 本身是 Pydantic 中 `FieldInfo` 的子类。
>
> Pydantic 的 `Field` 返回也是 `FieldInfo` 的类实例。
>
> `Body` 直接返回的也是 `FieldInfo` 的子类的对象。后文还会介绍一些 `Body` 的子类。
>
> 注意，从 `fastapi` 导入的 `Query`、`Path` 等对象实际上都是返回特殊类的函数。





---



### 添加更多信息

`Field`、`Query`、`Body` 等对象里可以声明更多信息，并且 JSON Schema 中也会集成这些信息。

*声明示例*一章中将详细介绍添加更多信息的知识。



### 小结

Pydantic 的 `Field` 可以为模型属性声明更多校验和元数据。

传递 JSON Schema 元数据还可以使用更多关键字参数。



















---







## 请求体 - 嵌套模型

使用 **FastAPI**，你可以定义、校验、记录文档并使用任意深度嵌套的模型（归功于Pydantic）。



### List 字段

你可以将一个属性定义为一个子类型。例如，Python `list`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-nested-models/#__tabbed_1_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: list = []


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```





这将使 `tags` 成为一个由元素组成的列表。不过它没有声明每个元素的类型。

### 带类型参数的 List 字段

不过，Python 有一种用于声明具有内部类型（类型参数）的列表的特定方式：

#### 声明带类型参数的 `list`

要声明具有类型参数（内部类型）的类型，例如 `list`、`dict`、`tuple`，使用方括号 `[` 和 `]` 传入内部类型作为「类型参数」：

```python
my_list: list[str]
```

这完全是用于类型声明的标准 Python 语法。

对具有内部类型的模型属性也使用相同的标准语法。

因此，在我们的示例中，我们可以将 `tags` 明确地指定为一个「字符串列表」：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-nested-models/#__tabbed_2_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: list[str] = []


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

### Set 类型

但是随后我们考虑了一下，意识到标签不应该重复，它们很大可能会是唯一的字符串。

而 Python 有一种用于保存唯一元素集合的特殊数据类型 `set`。

然后我们可以将 `tags` 声明为一个由字符串组成的 set：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-nested-models/#__tabbed_3_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```



这样，即使你收到带有重复数据的请求，这些数据也会被转换为一组唯一项。

而且，每当你输出该数据时，即使源数据有重复，它们也将作为一组唯一项输出。

并且还会被相应地标注 / 记录文档。



----





### 嵌套模型

Pydantic 模型的每个属性都具有类型。

但是这个类型本身可以是另一个 Pydantic 模型。

因此，你可以声明拥有特定属性名称、类型和校验的深度嵌套的 JSON 对象。

上述这些都可以任意的嵌套。



#### 定义子模型

例如，我们可以定义一个 `Image` 模型：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-nested-models/#__tabbed_4_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Image(BaseModel):
    url: str
    name: str


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    image: Image | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```





#### 将子模型用作类型

然后我们可以将其用作一个属性的类型：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-nested-models/#__tabbed_5_1)

```python 
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Image(BaseModel):
    url: str
    name: str


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    image: Image | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```





这意味着 **FastAPI** 将期望类似于以下内容的请求体：

```python
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2,
    "tags": ["rock", "metal", "bar"],
    "image": {
        "url": "http://example.com/baz.jpg",
        "name": "The Foo live"
    }
}
```

再一次，仅仅进行这样的声明，你将通过 **FastAPI** 获得：

- 对被嵌入的模型也适用的编辑器支持（自动补全等）
- 数据转换
- 数据校验
- 自动生成文档





----

### 特殊的类型和校验

除了普通的单一值类型（如 `str`、`int`、`float` 等）外，你还可以使用从 `str` 继承的更复杂的单一值类型。

要了解所有的可用选项，请查看 [Pydantic 的类型概览](https://docs.pydantic.dev/latest/concepts/types/)。你将在下一章节中看到一些示例。

例如，在 `Image` 模型中我们有一个 `url` 字段，我们可以把它声明为 Pydantic 的 `HttpUrl`，而不是 `str`：

```python
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()


class Image(BaseModel):
    url: HttpUrl
    name: str


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    image: Image | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```

该字符串将被检查是否为有效的 URL，并在 JSON Schema / OpenAPI 文档中进行记录。



### 带有一组子模型的属性

你还可以将 Pydantic 模型用作 `list`、`set` 等的子类型：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-nested-models/#__tabbed_7_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()


class Image(BaseModel):
    url: HttpUrl
    name: str


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    images: list[Image] | None = None


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```





这将期望（转换，校验，记录文档等）下面这样的 JSON 请求体：

```python 
{
    "name": "Foo",
    "description": "The pretender",
    "price": 42.0,
    "tax": 3.2,
    "tags": [
        "rock",
        "metal",
        "bar"
    ],
    "images": [
        {
            "url": "http://example.com/baz.jpg",
            "name": "The Foo live"
        },
        {
            "url": "http://example.com/dave.jpg",
            "name": "The Baz"
        }
    ]
}
```



### 深度嵌套模型

你可以定义任意深度的嵌套模型：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-nested-models/#__tabbed_8_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()


class Image(BaseModel):
    url: HttpUrl
    name: str


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()
    images: list[Image] | None = None


class Offer(BaseModel):
    name: str
    description: str | None = None
    price: float
    items: list[Item]


@app.post("/offers/")
async def create_offer(offer: Offer):
    return offer
```



### 纯列表请求体

如果你期望的 JSON 请求体的最外层是一个 JSON `array`（即 Python `list`），则可以在路径操作函数的参数中声明此类型，就像声明 Pydantic 模型一样：

```python
images: list[Image]
```

例如：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-nested-models/#__tabbed_9_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel, HttpUrl

app = FastAPI()


class Image(BaseModel):
    url: HttpUrl
    name: str


@app.post("/images/multiple/")
async def create_multiple_images(images: list[Image]):
    return images
```



### 无处不在的编辑器支持

你可以随处获得编辑器支持。

即使是列表中的元素：



![截屏2026-06-11 11.49.28](images/截屏2026-06-11 11.49.28.png)



如果你直接使用 `dict` 而不是 Pydantic 模型，那你将无法获得这种编辑器支持。

但是你根本不必担心这两者，传入的字典会自动被转换，你的输出也会自动被转换为 JSON。





---





### 任意 `dict` 构成的请求体

你也可以将请求体声明为使用某类型的键和其他类型值的 `dict`。

无需事先知道有效的字段/属性（在使用 Pydantic 模型的场景）名称是什么。

如果你想接收一些尚且未知的键，这将很有用。

------

其他有用的场景是当你想要接收其他类型的键时，例如 `int`。

这也是我们在接下来将看到的。

在下面的例子中，你将接受任意键为 `int` 类型并且值为 `float` 类型的 `dict`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-nested-models/#__tabbed_10_1)

```python 
from fastapi import FastAPI

app = FastAPI()


@app.post("/index-weights/")
async def create_index_weights(weights: dict[int, float]):
    return weights
```



> 🔥 提示
>
> 请记住 JSON 仅支持将 `str` 作为键。
>
> 但是 Pydantic 具有自动转换数据的功能。
>
> 这意味着，即使你的 API 客户端只能将字符串作为键发送，只要这些字符串内容仅包含整数，Pydantic 就会对其进行转换并校验。
>
> 然后你接收的名为 `weights` 的 `dict` 实际上将具有 `int` 类型的键和 `float` 类型的值。



----



### 总结

使用 **FastAPI** 你可以拥有 Pydantic 模型提供的极高灵活性，同时保持代码的简单、简短和优雅。

而且还具有下列好处：

- 编辑器支持（处处皆可自动补全！）
- 数据转换（也被称为解析/序列化）
- 数据校验
- 模式文档
- 自动生成的文档







---



## 声明请求实例数据

你可以为你的应用将接受的数据声明示例。

下面有几种实现方式：

### Pydantic模型中的额外 JSON Schema 数据

你可以为一个 Pydantic 模型声明 `examples`，它们会被添加到生成的 JSON Schema 中。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/schema-extra-example/#__tabbed_1_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None

    model_config = {
        "json_schema_extra": {
            "examples": [
                {
                    "name": "Foo",
                    "description": "A very nice Item",
                    "price": 35.4,
                    "tax": 3.2,
                }
            ]
        }
    }


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```



这些额外信息会原样添加到该模型输出的 JSON Schema 中，并会在 API 文档中使用。

你可以使用属性 `model_config`，它接收一个 `dict`，详见 [Pydantic 文档：配置](https://docs.pydantic.dev/latest/api/config/)。

你可以设置 `"json_schema_extra"`，其值为一个 `dict`，包含你希望出现在生成 JSON Schema 中的任意附加数据，包括 `examples`。



----

### `Field` 的附加参数

在 Pydantic 模型中使用 `Field()` 时，你也可以声明额外的 `examples`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/schema-extra-example/#__tabbed_2_1)

```python 
from fastapi import FastAPI
from pydantic import BaseModel, Field

app = FastAPI()


class Item(BaseModel):
    name: str = Field(examples=["Foo"])
    description: str | None = Field(default=None, examples=["A very nice Item"])
    price: float = Field(examples=[35.4])
    tax: float | None = Field(default=None, examples=[3.2])


@app.put("/items/{item_id}")
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```



---

### JSON Schema 中的 `examples` - OpenAPI

在以下任意场景中使用：

- `Path()`
- `Query()`
- `Header()`
- `Cookie()`
- `Body()`
- `Form()`
- `File()`

你也可以声明一组 `examples`，这些带有附加信息的示例将被添加到它们在 OpenAPI 中的 JSON Schema 里。



---

#### 带有 `examples` 的 `Body`

这里我们向 `Body()` 传入 `examples`，其中包含一个期望的数据示例：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/schema-extra-example/#__tabbed_3_1)

```python
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(
    item_id: int,
    item: Annotated[
        Item,
        Body(
            examples=[
                {
                    "name": "Foo",
                    "description": "A very nice Item",
                    "price": 35.4,
                    "tax": 3.2,
                }
            ],
        ),
    ],
):
    results = {"item_id": item_id, "item": item}
    return results
```

#### 文档 UI 中的示例

使用上述任一方法，在 `/docs` 中看起来会是这样：

![截屏2026-06-11 12.18.32](images/截屏2026-06-11 12.18.32.png)



#### 带有多个 `examples` 的 `Body`

当然，你也可以传入多个 `examples`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/schema-extra-example/#__tabbed_5_1)

```python 
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(
    *,
    item_id: int,
    item: Annotated[
        Item,
        Body(
            examples=[
                {
                    "name": "Foo",
                    "description": "A very nice Item",
                    "price": 35.4,
                    "tax": 3.2,
                },
                {
                    "name": "Bar",
                    "price": "35.4",
                },
                {
                    "name": "Baz",
                    "price": "thirty five point four",
                },
            ],
        ),
    ],
):
    results = {"item_id": item_id, "item": item}
    return results
```

这样做时，这些示例会成为该请求体数据内部 JSON Schema 的一部分。

不过，在撰写本文时，用于展示文档 UI 的 Swagger UI 并不支持显示 JSON Schema 中数据的多个示例。但请继续阅读，下面有一种变通方法。











----

#### OpenAPI 特定的 `examples`

在 JSON Schema 支持 `examples` 之前，OpenAPI 就已支持一个同名但不同的字段 `examples`。

这个面向 OpenAPI 的 `examples` 位于 OpenAPI 规范的另一处。它放在每个路径操作的详细信息中，而不是每个 JSON Schema 里。

而 Swagger UI 早就支持这个特定的 `examples` 字段。因此，你可以用它在文档 UI 中展示不同的示例。

这个 OpenAPI 特定字段 `examples` 的结构是一个包含多个示例的 `dict`（而不是一个 `list`），每个示例都包含会被添加到 OpenAPI 的额外信息。

这不放在 OpenAPI 内部包含的各个 JSON Schema 里，而是直接放在路径操作上。



----

#### 使用 `openapi_examples` 参数

你可以在 FastAPI 中通过参数 `openapi_examples` 来声明这个 OpenAPI 特定的 `examples`，适用于：

- `Path()`
- `Query()`
- `Header()`
- `Cookie()`
- `Body()`
- `Form()`
- `File()`

这个 `dict` 的键用于标识每个示例，每个值是另一个 `dict`。

`examples` 中每个具体示例的 `dict` 可以包含：

- `summary`：该示例的简短描述。
- `description`：较长描述，可以包含 Markdown 文本。
- `value`：实际展示的示例，例如一个 `dict`。
- `externalValue`：`value` 的替代项，指向该示例的 URL。不过它的工具支持度可能不如 `value`。

你可以这样使用：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/schema-extra-example/#__tabbed_7_1)

```python 
from typing import Annotated

from fastapi import Body, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None


@app.put("/items/{item_id}")
async def update_item(
    *,
    item_id: int,
    item: Annotated[
        Item,
        Body(
            openapi_examples={
                "normal": {
                    "summary": "A normal example",
                    "description": "A **normal** item works correctly.",
                    "value": {
                        "name": "Foo",
                        "description": "A very nice Item",
                        "price": 35.4,
                        "tax": 3.2,
                    },
                },
                "converted": {
                    "summary": "An example with converted data",
                    "description": "FastAPI can convert price `strings` to actual `numbers` automatically",
                    "value": {
                        "name": "Bar",
                        "price": "35.4",
                    },
                },
                "invalid": {
                    "summary": "Invalid data is rejected with an error",
                    "value": {
                        "name": "Baz",
                        "price": "thirty five point four",
                    },
                },
            },
        ),
    ],
):
    results = {"item_id": item_id, "item": item}
    return results
```





---

#### 文档 UI 中的 OpenAPI 示例

当把 `openapi_examples` 添加到 `Body()` 后，`/docs` 会如下所示：


<img width="1350" height="776" alt="截屏2026-06-11 12 32 20" src="https://github.com/user-attachments/assets/880e4f68-34b0-446a-9a35-aff7fc63555d" />







---

## 额外数据类型

到目前为止，您一直在使用常见的数据类型，如:

- `int`
- `float`
- `str`
- `bool`

但是您也可以使用更复杂的数据类型。

您仍然会拥有现在已经看到的相同的特性:

- 很棒的编辑器支持。
- 传入请求的数据转换。
- 响应数据转换。
- 数据验证。
- 自动注解和文档。

### 其他数据类型

下面是一些你可以使用的其他数据类型:

- `UUID` :
  - 一种标准的 "通用唯一标识符" ，在许多数据库和系统中用作ID。

  - 在请求和响应中将以 `str` 表示。

 - `datetime.datetime`:

  - 一个 Python `datetime.datetime`.

  - 在请求和响应中将表示为 ISO 8601 格式的 `str` ，比如: `2008-09-15T15:53:00+05:00`.

- `datetime.date`:
  - Python `datetime.date`.

  - 在请求和响应中将表示为 ISO 8601 格式的 `str` ，比如: `2008-09-15`.

- `datetime.time` :
  - 一个 Python `datetime.time`.

  - 在请求和响应中将表示为 ISO 8601 格式的 `str` ，比如: `14:23:55.003`.

- `datetime.timedelta`:

  - 一个 Python `datetime.timedelta`.

  - 在请求和响应中将表示为 `float` 代表总秒数。

  - Pydantic 也允许将其表示为 "ISO 8601 时间差异编码"。

- `frozenset`:
  - 在请求和响应中，作为`set`对待：
    - 在请求中，列表将被读取，消除重复，并将其转换为一个 `set`。

    - 在响应中 `set` 将被转换为 `list` 。

    - 产生的模式将指定那些 `set` 的值是唯一的 (使用 JSON Schema 的 `uniqueItems`)。

- `bytes`:
  - 标准的 Python `bytes`。

  - 在请求和响应中被当作 `str` 处理。

  - 生成的模式将指定这个 `str` 是 `binary` "格式"。

- `Decimal`:
  - 标准的 Python `Decimal`。
  - 在请求和响应中被当做 `float` 一样处理。

- 您可以在这里检查所有有效的 Pydantic 数据类型: [Pydantic data types](https://docs.pydantic.dev/latest/usage/types/types/)。



### 例子

下面是一个*路径操作*的示例，其中的参数使用了上面的一些类型。

```python
from datetime import datetime, time, timedelta
from typing import Annotated
from uuid import UUID

from fastapi import Body, FastAPI

app = FastAPI()


@app.put("/items/{item_id}")
async def read_items(
    item_id: UUID,
    start_datetime: Annotated[datetime, Body()],
    end_datetime: Annotated[datetime, Body()],
    process_after: Annotated[timedelta, Body()],
    repeat_at: Annotated[time | None, Body()] = None,
):
    start_process = start_datetime + process_after
    duration = end_datetime - start_process
    return {
        "item_id": item_id,
        "start_datetime": start_datetime,
        "end_datetime": end_datetime,
        "process_after": process_after,
        "repeat_at": repeat_at,
        "start_process": start_process,
        "duration": duration,
    }
```







---

## Cookie参数

定义 `Cookie` 参数与定义 `Query` 和 `Path` 参数一样。



### 导入 `Cookie`

首先，导入COOkie

```python
from typing import Annotated

from fastapi import FastAPI, Cookie

app = FastAPI()


@app.get("/items/")
async def read_items(ads_id: Annotated[str | None, Cookie()] = None):
    return {"ads_id": ads_id}
```



### 声明 `Cookie` 参数

声明 `Cookie` 参数的方式与声明 `Query` 和 `Path` 参数相同。

第一个值是默认值，还可以传递所有验证参数或注释参数：



> ​                                                                                                  ⚠️
>
> ​                                                  必须使用 `Cookie` 声明 cookie 参数，否则该参数会被解释为查询参数。



### 小结

使用 `Cookie` 声明 cookie 参数的方式与 `Query` 和 `Path` 相同。





---



## Header参数

定义 `Header` 参数的方式与定义 `Query`、`Path`、`Cookie` 参数相同。



### 导入 `Header`

首先，导入Header：

```python
from typing import Annotated

from fastapi import FastAPI, Header

app = FastAPI()


@app.get("/items/")
async def read_items(user_agent: Annotated[str | None, Header()] = None):
    return {"User-Agent": user_agent}
```



---



### 声明 `Header` 参数

然后，使用和 `Path`、`Query`、`Cookie` 一样的结构定义 header 参数。

第一个值是默认值，还可以传递所有验证参数或注释参数：

```python
from typing import Annotated

from fastapi import FastAPI, Header

app = FastAPI()


@app.get("/items/")
async def read_items(user_agent: Annotated[str | None, Header()] = None):
    return {"User-Agent": user_agent}
```



>  🖊️
>
> `Header` 是 `Path`、`Query`、`Cookie` 的**兄弟类**，都继承自共用的 `Param` 类。
>
> 注意，从 `fastapi` 导入的 `Query`、`Path`、`Header` 等对象，实际上是返回特殊类的函数。
>
> ⚠️
>
> 必须使用 `Header` 声明 header 参数，否则该参数会被解释为查询参数。



---



### 自动转换

`Header` 比 `Path`、`Query` 和 `Cookie` 提供了更多功能。

大部分标准请求头用**连字符**分隔，即**减号**（`-`）。

但是 `user-agent` 这样的变量在 Python 中是无效的。

因此，默认情况下，`Header` 把参数名中的字符由下划线（`_`）改为连字符（`-`）来提取并存档请求头 。

同时，HTTP 的请求头不区分大小写，可以使用 Python 标准样式（即 **snake_case**）进行声明。

因此，可以像在 Python 代码中一样使用 `user_agent` ，无需把首字母大写为 `User_Agent` 等形式。

如需禁用下划线自动转换为连字符，可以把 `Header` 的 `convert_underscores` 参数设置为 `False`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/header-params/#__tabbed_5_1)

```python
from typing import Annotated

from fastapi import FastAPI, Header

app = FastAPI()


@app.get("/items/")
async def read_items(
    strange_header: Annotated[str | None, Header(convert_underscores=False)] = None,
):
    return {"strange_header": strange_header}
```



> ⚠️ `警告`
>
> 注意，使用 `convert_underscores = False` 要慎重，有些 HTTP 代理和服务器不支持使用带有下划线的请求头。



---



### 重复的请求头

有时，可能需要接收重复的请求头。即同一个请求头有多个值。

类型声明中可以使用 `list` 定义多个请求头。

使用 Python `list` 可以接收重复请求头所有的值。

例如，声明 `X-Token` 多次出现的请求头，可以写成这样：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/header-params/#__tabbed_7_1)

```python 
from typing import Annotated

from fastapi import FastAPI, Header

app = FastAPI()


@app.get("/items/")
async def read_items(x_token: Annotated[list[str] | None, Header()] = None):
    return {"X-Token values": x_token}
```



与*路径操作*通信时，以下面的方式发送两个 HTTP 请求头：

```json
X-Token: foo
X-Token: bar
```

响应结果是：

```json
{
    "X-Token values": [
        "bar",
        "foo"
    ]
}
```



-----

### 小结

使用 `Header` 声明请求头的方式与 `Query`、`Path` 、`Cookie` 相同。

不用担心变量中的下划线，**FastAPI** 可以自动转换。





-----





## Cookie参数模型

如果您有一组相关的 **cookie**，您可以创建一个 **Pydantic 模型**来声明它们。🍪

这将允许您在**多个地方**能够**重用模型**，并且可以一次性声明所有参数的验证方式和元数据。😎



> 🔔
>
> 此技术同样适用于 `Query` 、 `Cookie` 和 `Header` 。😎



### 带有 Pydantic 模型的 Cookie

在 **Pydantic** 模型中声明所需的 **cookie** 参数，然后将参数声明为 `Cookie` ：

```python
from typing import Annotated

from fastapi import Cookie, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Cookies(BaseModel):
    session_id: str
    fatebook_tracker: str | None = None
    googall_tracker: str | None = None


@app.get("/items/")
async def read_items(cookies: Annotated[Cookies, Cookie()]):
    return cookies
```



**FastAPI** 将从请求中接收到的 **cookie** 中**提取**出**每个字段**的数据，并提供您定义的 Pydantic 模型。



---



### 查看文档

您可以在文档 UI 的 `/docs` 中查看定义的 cookie：

![截屏2026-06-11 18.29.00](images/截屏2026-06-11 18.29.00.png)

> 📒 `information`
>
> 请记住，由于**浏览器**以特殊方式**处理 cookie**，并在后台进行操作，因此它们**不会**轻易允许 **JavaScript** 访问这些 cookie。
>
> 如果您访问 `/docs` 的 **API 文档 UI**，您将能够查看您*路径操作*的 cookie **文档**。
>
> 但是即使您**填写数据**并点击“执行”，由于文档界面使用 **JavaScript**，cookie 将不会被发送。而您会看到一条**错误**消息，就好像您没有输入任何值一样。



---



### 禁止额外的 Cookie

在某些特殊使用情况下（可能并不常见），您可能希望**限制**您想要接收的 cookie。

您的 API 现在可以控制自己的 cookie 同意。🤪🍪

您可以使用 Pydantic 的模型配置来禁止（ `forbid` ）任何额外（ `extra` ）字段：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/cookie-param-models/#__tabbed_3_1)

```python
from typing import Annotated

from fastapi import Cookie, FastAPI
from pydantic import BaseModel

app = FastAPI()


class Cookies(BaseModel):
    model_config = {"extra": "forbid"}

    session_id: str
    fatebook_tracker: str | None = None
    googall_tracker: str | None = None


@app.get("/items/")
async def read_items(cookies: Annotated[Cookies, Cookie()]):
    return cookies
```



如果客户端尝试发送一些**额外的 cookie**，他们将收到**错误**响应。

可怜的 cookie 通知条，费尽心思为了获得您的同意，却被API 拒绝了。🍪

例如，如果客户端尝试发送一个值为 `good-list-please` 的 `santa_tracker` cookie，客户端将收到一个**错误**响应，告知他们 `santa_tracker` cookie 是不允许的：

```python
{
    "detail": [
        {
            "type": "extra_forbidden",
            "loc": ["cookie", "santa_tracker"],
            "msg": "Extra inputs are not permitted",
            "input": "good-list-please",
        }
    ]
}
```



---



### 总结

您可以使用 **Pydantic 模型**在 **FastAPI** 中声明 **cookie**。😎





---

---



## Header参数模型

如果您有一组相关的 **header 参数**，您可以创建一个 **Pydantic 模型**来声明它们。

这将允许您在**多个地方**能够**重用模型**，并且可以一次性声明所有参数的验证和元数据。😎

### 使用 Pydantic 模型的 Header 参数

在 **Pydantic 模型**中声明所需的 **header 参数**，然后将参数声明为 `Header` :

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/header-param-models/#__tabbed_1_1)

```python
from typing import Annotated

from fastapi import FastAPI, Header
from pydantic import BaseModel

app = FastAPI()


class CommonHeaders(BaseModel):
    host: str
    save_data: bool
    if_modified_since: str | None = None
    traceparent: str | None = None
    x_tag: list[str] = []


@app.get("/items/")
async def read_items(headers: Annotated[CommonHeaders, Header()]):
    return headers
```

**FastAPI** 将从请求中接收到的 **headers** 中**提取**出**每个字段**的数据，并提供您定义的 Pydantic 模型。



---



### 查看文档

您可以在文档 UI 的 `/docs` 中查看所需的 headers：



![截屏2026-06-11 19.54.50](images/截屏2026-06-11 19.54.50.png)



---



### 禁止额外的 Headers

在某些特殊使用情况下（可能并不常见），您可能希望**限制**您想要接收的 headers。

您可以使用 Pydantic 的模型配置来禁止（ `forbid` ）任何额外（ `extra` ）字段：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/header-param-models/#__tabbed_3_1)

```python 
from typing import Annotated

from fastapi import FastAPI, Header
from pydantic import BaseModel

app = FastAPI()


class CommonHeaders(BaseModel):
    model_config = {"extra": "forbid"}

    host: str
    save_data: bool
    if_modified_since: str | None = None
    traceparent: str | None = None
    x_tag: list[str] = []


@app.get("/items/")
async def read_items(headers: Annotated[CommonHeaders, Header()]):
    return headers
```



如果客户尝试发送一些**额外的 headers**，他们将收到**错误**响应。

例如，如果客户端尝试发送一个值为 `plumbus` 的 `tool` header，客户端将收到一个**错误**响应，告知他们 header 参数 `tool` 是不允许的：

```json
{
    "detail": [
        {
            "type": "extra_forbidden",
            "loc": ["header", "tool"],
            "msg": "Extra inputs are not permitted",
            "input": "plumbus",
        }
    ]
}
```



---



### 禁用下划线转换

与常规的 header 参数相同，当参数名中包含下划线时，会**自动转换为连字符**。

例如，如果你的代码中有一个名为 `save_data` 的 header 参数，那么预期的 HTTP 头将是 `save-data`，并且在文档中也会以这种形式显示。

如果由于某些原因你需要禁用这种自动转换，你也可以在用于 header 参数的 Pydantic 模型中进行设置。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/header-param-models/#__tabbed_5_1)

```
from typing import Annotated

from fastapi import FastAPI, Header
from pydantic import BaseModel

app = FastAPI()


class CommonHeaders(BaseModel):
    host: str
    save_data: bool
    if_modified_since: str | None = None
    traceparent: str | None = None
    x_tag: list[str] = []


@app.get("/items/")
async def read_items(
    headers: Annotated[CommonHeaders, Header(convert_underscores=False)],
):
    return headers
```



> ⚠️ 警告
>
> 在将 `convert_underscores` 设为 `False` 之前，请注意某些 HTTP 代理和服务器不允许使用带下划线的 headers。

### 总结

您可以使用 **Pydantic 模型**在 **FastAPI** 中声明 **headers**。😎



----

----



## 响应模型 - 返回类型

你可以通过为*路径操作函数*的**返回类型**添加注解来声明用于响应的类型。

与为输入数据在函数**参数**里做类型注解的方式相同，你可以使用 Pydantic 模型、`list`、`dict`、以及整数、布尔值等标量类型。

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: list[str] = []


@app.post("/items/")
async def create_item(item: Item) -> Item:
    return item


@app.get("/items/")
async def read_items() -> list[Item]:
    return [
        Item(name="Portal Gun", price=42.0),
        Item(name="Plumbus", price=32.0),
    ]
```



FastAPI 会使用这个返回类型来：

- 对返回数据进行校验。
  - 如果数据无效（例如缺少某个字段），这意味着你的应用代码有问题，没有返回应有的数据，FastAPI 将返回服务器错误而不是返回错误的数据。这样你和你的客户端都可以确定会收到期望的数据及其结构。
- 在 OpenAPI 的路径操作中为响应添加JSON Schema。
  - 它会被**自动文档**使用。
  - 它也会被自动客户端代码生成工具使用。
- 使用 Pydantic 将返回数据**序列化**为 JSON。Pydantic 使用**Rust**编写，因此会**快很多**。

但更重要的是：

- 它会将输出数据限制并过滤为返回类型中定义的内容。
  - 这对**安全性**尤为重要，下面会进一步介绍。



---



### `response_model` 参数

在一些情况下，你需要或希望返回的数据与声明的类型不完全一致。

例如，你可能希望**返回一个字典**或数据库对象，但**将其声明为一个 Pydantic 模型**。这样 Pydantic 模型就会为你返回的对象（例如字典或数据库对象）完成文档、校验等工作。

如果你添加了返回类型注解，工具和编辑器会（正确地）报错，提示你的函数返回的类型（例如 `dict`）与声明的类型（例如一个 Pydantic 模型）不同。

在这些情况下，你可以使用*路径操作装饰器*参数 `response_model`，而不是返回类型。

你可以在任意*路径操作*中使用 `response_model` 参数：

- `@app.get()`
- `@app.post()`
- `@app.put()`
- `@app.delete()`
- 等等。

```python
from typing import Any

from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: list[str] = []


@app.post("/items/", response_model=Item)
async def create_item(item: Item) -> Any:
    return item


@app.get("/items/", response_model=list[Item])
async def read_items() -> Any:
    return [
        {"name": "Portal Gun", "price": 42.0},
        {"name": "Plumbus", "price": 32.0},
    ]
```



> ⚠️
>
> 注意，`response_model` 是「装饰器」方法（`get`、`post` 等）的一个参数。不是你的*路径操作函数*的参数，不像所有查询参数和请求体那样。

`response_model` 接收的类型与为 Pydantic 模型字段声明的类型相同，因此它可以是一个 Pydantic 模型，也可以是一个由 Pydantic 模型组成的 `list`，例如 `List[Item]`。

FastAPI 会使用这个 `response_model` 来完成数据文档、校验等，并且还会将输出数据**转换并过滤**为其类型声明。



#### `response_model` 的优先级

如果你同时声明了返回类型和 `response_model`，`response_model` 会具有优先级并由 FastAPI 使用。

这样，即使你返回的类型与响应模型不同，你也可以为函数添加正确的类型注解，供编辑器和 mypy 等工具使用。同时你仍然可以让 FastAPI 使用 `response_model` 进行数据校验、文档等。

你也可以使用 `response_model=None` 来禁用该*路径操作*的响应模型生成；当你为一些不是有效 Pydantic 字段的东西添加类型注解时，可能需要这样做，下面的章节会有示例。



---



### 返回与输入相同的数据

这里我们声明一个 `UserIn` 模型，它包含一个明文密码;我们使用这个模型来声明输入，同时也用相同的模型来声明输出：

```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()


class UserIn(BaseModel):
    username: str
    password: str
    email: EmailStr
    full_name: str | None = None


# Don't do this in production!
@app.post("/user/")
async def create_user(user: UserIn) -> UserIn:
    return user
```



现在，每当浏览器使用密码创建用户时，API 会在响应中返回相同的密码。

在这个场景下，这可能不算问题，因为发送密码的是同一个用户。

但如果我们在其他*路径操作*中使用相同的模型，就可能会把用户的密码发送给每个客户端。





> ‼️
>
> 除非你非常清楚所有注意事项并确实知道自己在做什么，否则永远不要存储用户的明文密码，也不要像这样在响应中发送它。



---



### 添加输出模型

相反，我们可以创建一个包含明文密码的输入模型和一个不包含它的输出模型;这里，即使我们的*路径操作函数*返回的是包含密码的同一个输入用户：



```python
from typing import Any

from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()


class UserIn(BaseModel):
    username: str
    password: str
    email: EmailStr
    full_name: str | None = None


class UserOut(BaseModel):
    username: str
    email: EmailStr
    full_name: str | None = None


@app.post("/user/", response_model=UserOut)
async def create_user(user: UserIn) -> Any:
    return user
```



因此，**FastAPI** 会负责过滤掉输出模型中未声明的所有数据（使用 Pydantic）。



#### `response_model` 还是返回类型

在这个例子中，因为两个模型不同，如果我们将函数返回类型注解为 `UserOut`，编辑器和工具会抱怨我们返回了无效类型，因为它们是不同的类。

这就是为什么在这个例子里我们必须在 `response_model` 参数中声明它。

……但继续往下读，看看如何更好地处理这种情况。



---



### 返回类型与数据过滤

延续上一个例子。我们希望**用一种类型来注解函数**，但希望从函数返回的内容实际上可以**包含更多数据**。

我们希望 FastAPI 继续使用响应模型来**过滤**数据。这样即使函数返回了更多数据，响应也只会包含响应模型中声明的字段。

在上一个例子中，因为类不同，我们不得不使用 `response_model` 参数。但这也意味着我们无法从编辑器和工具处获得对函数返回类型的检查支持。

不过在大多数需要这样做的场景里，我们只是希望模型像这个例子中那样**过滤/移除**一部分数据。

在这些场景里，我们可以使用类和继承，既利用函数的**类型注解**获取更好的编辑器和工具支持，又能获得 FastAPI 的**数据过滤**。



```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()


class BaseUser(BaseModel):
    username: str
    email: EmailStr
    full_name: str | None = None


class UserIn(BaseUser):
    password: str


@app.post("/user/")
async def create_user(user: UserIn) -> BaseUser:
    return user
```



这样一来，我们既能从编辑器和 mypy 获得工具支持（这段代码在类型上是正确的），也能从 FastAPI 获得数据过滤。

这是如何做到的？我们来看看。🤓

#### 类型注解与工具链

先看看编辑器、mypy 和其他工具会如何看待它。

`BaseUser` 有基础字段。然后 `UserIn` 继承自 `BaseUser` 并新增了 `password` 字段，因此它包含了两个模型的全部字段。

我们把函数返回类型注解为 `BaseUser`，但实际上返回的是一个 `UserIn` 实例。

编辑器、mypy 和其他工具不会对此抱怨，因为在类型系统里，`UserIn` 是 `BaseUser` 的子类，这意味着当期望 `BaseUser` 时，返回 `UserIn` 是*合法*的。



#### FastAPI 的数据过滤

对于 FastAPI，它会查看返回类型并确保你返回的内容**只**包含该类型中声明的字段。

FastAPI 在内部配合 Pydantic 做了多项处理，确保不会把类继承的这些规则用于返回数据的过滤，否则你可能会返回比预期多得多的数据。

这样，你就能兼得两方面的优势：带有**工具支持**的类型注解和**数据过滤**。



---



### 在文档中查看

当你查看自动文档时，你会看到输入模型和输出模型都会有各自的 JSON Schema：

![截屏2026-06-11 23.55.54](images/截屏2026-06-11 23.55.54.png)

并且两个模型都会用于交互式 API 文档：

![截屏2026-06-11 23.59.06](images/截屏2026-06-11 23.59.06.png)



---



### 其他返回类型注解

有些情况下你会返回一些不是有效 Pydantic 字段的内容，并在函数上做了相应注解，只是为了获得工具链（编辑器、mypy 等）的支持。



#### 直接返回 Response

最常见的情况是[直接返回 Response，详见进阶文档](https://fastapi.tiangolo.com/zh/advanced/response-directly/)。

```python
from fastapi import FastAPI, Response
from fastapi.responses import JSONResponse, RedirectResponse

app = FastAPI()


@app.get("/portal")
async def get_portal(teleport: bool = False) -> Response:
    if teleport:
        return RedirectResponse(url="https://www.youtube.com/watch?v=dQw4w9WgXcQ")
    return JSONResponse(content={"message": "Here's your interdimensional portal."})
```

这个简单场景 FastAPI 会自动处理，因为返回类型注解是 `Response`（或其子类）。

工具也会满意，因为 `RedirectResponse` 和 `JSONResponse` 都是 `Response` 的子类，所以类型注解是正确的。



#### 注解 Response 的子类

你也可以在类型注解中使用 `Response` 的子类：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/response-model/#__tabbed_10_1)

```python
from fastapi import FastAPI
from fastapi.responses import RedirectResponse

app = FastAPI()


@app.get("/teleport")
async def get_teleport() -> RedirectResponse:
    return RedirectResponse(url="https://www.youtube.com/watch?v=dQw4w9WgXcQ")
```



这同样可行，因为 `RedirectResponse` 是 `Response` 的子类，FastAPI 会自动处理这个简单场景。



#### 无效的返回类型注

但当你返回其他任意对象（如数据库对象）而它不是有效的 Pydantic 类型，并在函数中按此进行了注解时，FastAPI 会尝试基于该类型注解创建一个 Pydantic 响应模型，但会失败。

如果你有一个在多个类型之间的联合类型，其中一个或多个不是有效的 Pydantic 类型，也会发生同样的情况，例如这个会失败 💥：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/response-model/#__tabbed_11_1)

```python
from fastapi import FastAPI, Response
from fastapi.responses import RedirectResponse

app = FastAPI()


@app.get("/portal")
async def get_portal(teleport: bool = False) -> Response | dict:
    if teleport:
        return RedirectResponse(url="https://www.youtube.com/watch?v=dQw4w9WgXcQ")
    return {"message": "Here's your interdimensional portal."}
```



……它失败是因为该类型注解不是 Pydantic 类型，也不只是单个 `Response` 类或其子类，而是 `Response` 与 `dict` 的联合类型（任意其一）。



#### 禁用响应模型

延续上面的例子，你可能不想要 FastAPI 执行默认的数据校验、文档、过滤等。

但你可能仍然想在函数上保留返回类型注解，以获得编辑器和类型检查器（如 mypy）的支持。

在这种情况下，你可以通过设置 `response_model=None` 来禁用响应模型生成：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/response-model/#__tabbed_12_1)

```python
from fastapi import FastAPI, Response
from fastapi.responses import RedirectResponse

app = FastAPI()


@app.get("/portal", response_model=None)
async def get_portal(teleport: bool = False) -> Response | dict:
    if teleport:
        return RedirectResponse(url="https://www.youtube.com/watch?v=dQw4w9WgXcQ")
    return {"message": "Here's your interdimensional portal."}
```



这会让 FastAPI 跳过响应模型的生成，这样你就可以按需使用任意返回类型注解，而不会影响你的 FastAPI 应用。🤓



---



### 响应模型的编码参数

你的响应模型可以具有默认值，例如：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/response-model/#__tabbed_13_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float = 10.5
    tags: list[str] = []


items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The bartenders", "price": 62, "tax": 20.2},
    "baz": {"name": "Baz", "description": None, "price": 50.2, "tax": 10.5, "tags": []},
}


@app.get("/items/{item_id}", response_model=Item, response_model_exclude_unset=True)
async def read_item(item_id: str):
    return items[item_id]
```



- `description: Union[str, None] = None`（或在 Python 3.10 中的 `str | None = None`）默认值为 `None`。
- `tax: float = 10.5` 默认值为 `10.5`。
- `tags: List[str] = []` 默认值为一个空列表：`[]`。

但如果它们并没有被实际存储，你可能希望在结果中省略这些默认值。

例如，当你在 NoSQL 数据库中保存了具有许多可选属性的模型，但又不想发送充满默认值的冗长 JSON 响应。



#### 使用 `response_model_exclude_unset` 参数

你可以设置*路径操作装饰器*参数 `response_model_exclude_unset=True`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/response-model/#__tabbed_14_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float = 10.5
    tags: list[str] = []


items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The bartenders", "price": 62, "tax": 20.2},
    "baz": {"name": "Baz", "description": None, "price": 50.2, "tax": 10.5, "tags": []},
}


@app.get("/items/{item_id}", response_model=Item, response_model_exclude_unset=True)
async def read_item(item_id: str):
    return items[item_id]
```



这样响应中将不会包含那些默认值，而只包含实际设置的值。

因此，如果你向该*路径操作*请求 ID 为 `foo` 的商品，响应（不包括默认值）将为：

```python
{
    "name": "Foo",
    "price": 50.2
}
```



> 🔔
>
> 你还可以使用：
>
> - `response_model_exclude_defaults=True`
> - `response_model_exclude_none=True`
>
> 详见 [Pydantic 文档](https://docs.pydantic.dev/1.10/usage/exporting_models/#modeldict)中对 `exclude_defaults` 和 `exclude_none` 的说明。



##### 默认字段有实际值的数据

但是，如果你的数据在具有默认值的模型字段中有实际的值，例如 ID 为 `bar` 的项：

```python
{
    "name": "Bar",
    "description": "The bartenders",
    "price": 62,
    "tax": 20.2
}
```

这些值将包含在响应中。



##### 具有与默认值相同值的数据

如果数据具有与默认值相同的值，例如 ID 为 `baz` 的项：

```python
{
    "name": "Baz",
    "description": None,
    "price": 50.2,
    "tax": 10.5,
    "tags": []
}
```

FastAPI 足够聪明（实际上是 Pydantic 足够聪明）去认识到，即使 `description`、`tax` 和 `tags` 的值与默认值相同，它们也是被显式设置的（而不是取自默认值）。

因此，它们将包含在 JSON 响应中。



---

#### `response_model_include` 和 `response_model_exclude

你还可以使用*路径操作装饰器*的 `response_model_include` 和 `response_model_exclude` 参数。

它们接收一个由属性名 `str` 组成的 `set`，用于包含（忽略其他）或排除（包含其他）这些属性。

当你只有一个 Pydantic 模型，并且想要从输出中移除一些数据时，这可以作为一种快捷方式。

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float = 10.5


items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The Bar fighters", "price": 62, "tax": 20.2},
    "baz": {
        "name": "Baz",
        "description": "There goes my baz",
        "price": 50.2,
        "tax": 10.5,
    },
}


@app.get(
    "/items/{item_id}/name",
    response_model=Item,
    response_model_include={"name", "description"},
)
async def read_item_name(item_id: str):
    return items[item_id]


@app.get("/items/{item_id}/public", response_model=Item, response_model_exclude={"tax"})
async def read_item_public_data(item_id: str):
    return items[item_id]
```



##### 使用 `list` 而不是 `set`

如果你忘记使用 `set` 而是使用了 `list` 或 `tuple`，FastAPI 仍会将其转换为 `set` 并正常工作：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/response-model/#__tabbed_16_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float = 10.5


items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The Bar fighters", "price": 62, "tax": 20.2},
    "baz": {
        "name": "Baz",
        "description": "There goes my baz",
        "price": 50.2,
        "tax": 10.5,
    },
}


@app.get(
    "/items/{item_id}/name",
    response_model=Item,
    response_model_include=["name", "description"],
)
async def read_item_name(item_id: str):
    return items[item_id]


@app.get("/items/{item_id}/public", response_model=Item, response_model_exclude=["tax"])
async def read_item_public_data(item_id: str):
    return items[item_id]
```





### 总结

使用*路径操作装饰器*的 `response_model` 参数来定义响应模型，尤其是确保私有数据被过滤掉。

使用 `response_model_exclude_unset` 来仅返回显式设置的值。







-----

----

## 更多模型

书接上文，多个关联模型这种情况很常见。

特别是用户模型，因为：

- **输入模型**应该含密码

- **输出模型**不应含密码

- **数据库模型**可能需要包含哈希后的密码

     

> ⚡️ `danger`
>
> 不要存储用户的明文密码。始终只存储之后可用于校验的“安全哈希”。
>
> 如果你还不了解，可以在[安全性章节](https://fastapi.tiangolo.com/zh/tutorial/security/simple-oauth2/#password-hashing)中学习什么是“密码哈希”。



### 多个模型

下面的代码展示了不同模型处理密码字段的方式，及使用位置的大致思路：

```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()


class UserIn(BaseModel):
    username: str
    password: str
    email: EmailStr
    full_name: str | None = None


class UserOut(BaseModel):
    username: str
    email: EmailStr
    full_name: str | None = None


class UserInDB(BaseModel):
    username: str
    hashed_password: str
    email: EmailStr
    full_name: str | None = None


def fake_password_hasher(raw_password: str):
    return "supersecret" + raw_password


def fake_save_user(user_in: UserIn):
    hashed_password = fake_password_hasher(user_in.password)
    user_in_db = UserInDB(**user_in.model_dump(), hashed_password=hashed_password)
    print("User saved! ..not really")
    return user_in_db


@app.post("/user/", response_model=UserOut)
async def create_user(user_in: UserIn):
    user_saved = fake_save_user(user_in)
    return user_saved
```



#### 关于 `**user_in.model_dump()`

##### Pydantic 的 `.model_dump()`

`user_in` 是类 `UserIn` 的 Pydantic 模型。

Pydantic 模型有 `.model_dump()` 方法，会返回包含模型数据的 `dict`。

因此，如果使用如下方式创建 Pydantic 对象 `user_in`：

```python
user_in = UserIn(username="john", password="secret", email="john.doe@example.com")
```

就能以如下方式调用：

```python
user_dict = user_in.model_dump()
```

现在，变量 `user_dict` 中的是包含数据的 `dict`（它是 `dict`，不是 Pydantic 模型对象）。

以如下方式调用：

```python
print(user_dict)
```

输出的就是 Python `dict`：

```json 
{
    'username': 'john',
    'password': 'secret',
    'email': 'john.doe@example.com',
    'full_name': None,
}
```



---



##### 解包 `dict`

把 `dict`（如 `user_dict`）以 `**user_dict` 形式传递给函数（或类），Python 会执行“解包”。它会把 `user_dict` 的键和值作为关键字参数直接传递。

因此，接着上面的 `user_dict` 继续编写如下代码：

```python
UserInDB(**user_dict)
```

就会生成如下结果：

```python
UserInDB(
    username="john",
    password="secret",
    email="john.doe@example.com",
    full_name=None,
)
```

或更精准，直接使用 `user_dict`（无论它将来包含什么字段）：

```python
UserInDB(
    username = user_dict["username"],
    password = user_dict["password"],
    email = user_dict["email"],
    full_name = user_dict["full_name"],
```



##### 用另一个模型的内容生成 Pydantic 模型

上例中 ，从 `user_in.model_dump()` 中得到了 `user_dict`，下面的代码：

```python
user_dict = user_in.model_dump()
UserInDB(**user_dict)
```

等效于：

```python
UserInDB(**user_in.model_dump())
```

...因为 `user_in.model_dump()` 是 `dict`，在传递给 `UserInDB` 时，把 `**` 加在 `user_in.model_dump()` 前，可以让 Python 进行解包。

这样，就可以用其它 Pydantic 模型中的数据生成 Pydantic 模型。



#### 解包 `dict` 并添加额外关键字参数[¶](https://fastapi.tiangolo.com/zh/tutorial/extra-models/#unpacking-a-dict-and-extra-keywords)

接下来，继续添加关键字参数 `hashed_password=hashed_password`，例如：

```python
UserInDB(**user_in.model_dump(), hashed_password=hashed_password)
```

...输出结果如下：

```python
UserInDB(
    username = user_dict["username"],
    password = user_dict["password"],
    email = user_dict["email"],
    full_name = user_dict["full_name"],
    hashed_password = hashed_password,
)
```





---





### 减少重复

减少代码重复是 **FastAPI** 的核心思想之一。

代码重复会导致 bug、安全问题、代码失步等问题（更新了某个位置的代码，但没有同步更新其它位置的代码）。

上面的这些模型共享了大量数据，拥有重复的属性名和类型。

我们可以做得更好。

声明 `UserBase` 模型作为其它模型的基类。然后，用该类衍生出继承其属性（类型声明、校验等）的子类。

所有数据转换、校验、文档等功能仍将正常运行。

这样，就可以仅声明模型之间的差异部分（具有明文的 `password`、具有 `hashed_password` 以及不包括密码）：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/extra-models/#__tabbed_2_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel, EmailStr

app = FastAPI()


class UserBase(BaseModel):
    username: str
    email: EmailStr
    full_name: str | None = None


class UserIn(UserBase):
    password: str


class UserOut(UserBase):
    pass


class UserInDB(UserBase):
    hashed_password: str


def fake_password_hasher(raw_password: str):
    return "supersecret" + raw_password


def fake_save_user(user_in: UserIn):
    hashed_password = fake_password_hasher(user_in.password)
    user_in_db = UserInDB(**user_in.model_dump(), hashed_password=hashed_password)
    print("User saved! ..not really")
    return user_in_db


@app.post("/user/", response_model=UserOut)
async def create_user(user_in: UserIn):
    user_saved = fake_save_user(user_in)
    return user_saved
```





### `Union` 或 `anyOf`

响应可以声明为两个或多个类型的 `Union`，即该响应可以是这些类型中的任意一种。

在 OpenAPI 中会用 `anyOf` 表示。

为此，请使用 Python 标准类型提示 [`typing.Union`](https://docs.python.org/3/library/typing.html#typing.Union)：

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class BaseItem(BaseModel):
    description: str
    type: str


class CarItem(BaseItem):
    type: str = "car"


class PlaneItem(BaseItem):
    type: str = "plane"
    size: int


items = {
    "item1": {"description": "All my friends drive a low rider", "type": "car"},
    "item2": {
        "description": "Music is my aeroplane, it's my aeroplane",
        "type": "plane",
        "size": 5,
    },
}


@app.get("/items/{item_id}", response_model=PlaneItem | CarItem)
async def read_item(item_id: str):
    return items[item_id]
```



#### Python 3.10 中的 `Union`

在这个示例中，我们把 `Union[PlaneItem, CarItem]` 作为参数 `response_model` 的值传入。

因为这是作为“参数的值”而不是放在“类型注解”中，所以即使在 Python 3.10 也必须使用 `Union`。

如果是在类型注解中，我们就可以使用竖线：

```python
some_variable: PlaneItem | CarItem
```

但如果把它写成赋值 `response_model=PlaneItem | CarItem`，就会报错，因为 Python 会尝试在 `PlaneItem` 和 `CarItem` 之间执行一个“无效的运算”，而不是把它当作类型注解来解析。



### 模型列表

同样地，你可以声明由对象列表构成的响应。

为此，请使用标准的 Python `list`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/extra-models/#__tabbed_4_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str


items = [
    {"name": "Foo", "description": "There comes my hero"},
    {"name": "Red", "description": "It's my aeroplane"},
]


@app.get("/items/", response_model=list[Item])
async def read_items():
    return items
```



---



### 任意 `dict` 的响应

你也可以使用普通的任意 `dict` 来声明响应，只需声明键和值的类型，无需使用 Pydantic 模型。

如果你事先不知道有效的字段/属性名（Pydantic 模型需要预先知道字段）时，这很有用。

此时，可以使用 `dict`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/extra-models/#__tabbed_5_1)

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/keyword-weights/", response_model=dict[str, float])
async def read_keyword_weights():
    return {"foo": 2.3, "bar": 3.4}
```





### 小结

针对不同场景，可以随意使用不同的 Pydantic 模型并通过继承复用。

当一个实体需要具备不同的“状态”时，无需只为该实体定义一个数据模型。例如，用户“实体”就可能有包含 `password`、包含 `password_hash` 以及不含密码等多种状态。





---

---

## 响应状态码

与指定响应模型的方式相同，在以下任意*路径操作*中，可以使用 `status_code` 参数声明用于响应的 HTTP 状态码：

- `@app.get()`
- `@app.post()`
- `@app.put()`
- `@app.delete()`
- 等...

```python
from fastapi import FastAPI

app = FastAPI()


@app.post("/items/", status_code=201)
async def create_item(name: str):
    return {"name": name}
```



> ⚡️ 注意
>
> 注意，`status_code` 是（`get`、`post` 等）**装饰器**方法中的参数。与之前的参数和请求体不同，不是*路径操作函数*的参数。



`status_code` 参数接收表示 HTTP 状态码的数字。

它可以：

- 在响应中返回状态码
- 在 OpenAPI 概图（及用户界面）中存档：

![截屏2026-06-12 16.27.13](images/截屏2026-06-12 16.27.13.png)

> 📝
>
> 某些响应状态码表示响应没有响应体（参阅下一章）。
>
> FastAPI 可以进行识别，并生成表明无响应体的 OpenAPI 文档。



### 关于 HTTP 状态码

在 HTTP 协议中，发送 3 位数的数字状态码是响应的一部分。

这些状态码都具有便于识别的关联名称，但是重要的还是数字。

简言之：

- `100 - 199` 用于返回“信息”。这类状态码很少直接使用。具有这些状态码的响应不能包含响应体

- `200 - 299`

   

  用于表示“成功”。这些状态码是最常用的

  - `200` 是默认状态码，表示一切“OK”
  - `201` 表示“已创建”，通常在数据库中创建新记录后使用
  - `204` 是一种特殊的例子，表示“无内容”。该响应在没有为客户端返回内容时使用，因此，该响应不能包含响应体

- **300 - 399** 用于“重定向”。具有这些状态码的响应不一定包含响应体，但 `304`“未修改”是个例外，该响应不得包含响应体

- `400 - 499`

   

  用于表示“客户端错误”。这些可能是第二常用的类型

  - `404`，用于“未找到”响应
  - 对于来自客户端的一般错误，可以只使用 `400`

- `500 - 599` 用于表示服务器端错误。几乎永远不会直接使用这些状态码。应用代码或服务器出现问题时，会自动返回这些状态码



> 🔥 Tip
>
> 想了解每个状态码的更多信息以及适用场景，请参阅 [MDN 的 HTTP 状态码文档](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)。



## 状态码名称快捷方式[¶](https://fastapi.tiangolo.com/zh/tutorial/response-status-code/#shortcut-to-remember-the-names)

再看下之前的例子：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/response-status-code/#__tabbed_2_1)

```python
from fastapi import FastAPI

app = FastAPI()


@app.post("/items/", status_code=201)
async def create_item(name: str):
    return {"name": name}
```





`201` 表示“已创建”的状态码。

但我们没有必要记住所有代码的含义。

可以使用 `fastapi.status` 中的快捷变量。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/response-status-code/#__tabbed_3_1)

```python
from fastapi import FastAPI, status

app = FastAPI()


@app.post("/items/", status_code=status.HTTP_201_CREATED)
async def create_item(name: str):
    return {"name": name}
```





这只是一种快捷方式，具有相同的数字代码，但它可以使用编辑器的自动补全功能：

![截屏2026-06-12 16.43.05](images/截屏2026-06-12 16.43.05.png)

### 更改默认状态码

稍后在[高级用户指南](https://fastapi.tiangolo.com/zh/advanced/response-change-status-code/)中，你将看到如何返回与此处声明的默认状态码不同的状态码。









-------

---

## 表单数据

当你需要接收表单字段而不是 `JSON `时，可以使用 `Form`。



> 🔔
>
> 要使用表单，首先安装 [`python-multipart`](https://github.com/Kludex/python-multipart)。
>
> 请先创建并激活一个[虚拟环境](https://fastapi.tiangolo.com/zh/virtual-environments/)，然后再进行安装，例如：
>
> ```python
> $ pip install python-multipart
> ```

例如，在 OAuth2 规范的一种使用方式（称为“密码流”）中，要求将 `username` 和 `password` 作为表单字段发送。

规范要求这些字段必须精确命名为 `username` 和 `password`，并且作为表单字段发送，而不是 JSON。

使用 `Form` 可以像使用 `Body`（以及 `Query`、`Path`、`Cookie`）一样声明相同的配置，包括校验、示例、别名（例如将 `username` 写成 `user-name`）等。



> ❗️
>
> `Form` 是直接继承自 `Body` 的类。
>
> 要声明表单请求体，必须显式使用 `Form`，否则这些参数会被当作查询参数或请求体（JSON）参数。



### 关于 "表单字段"

HTML 表单（`<form></form>`）向服务器发送数据时通常会对数据使用一种“特殊”的编码方式，这与 JSON 不同。

**FastAPI** 会确保从正确的位置读取这些数据，而不是从 JSON 中读取。



> ⚠️ Warning
>
> 你可以在一个路径操作中声明多个 `Form` 参数，但不能同时再声明要接收为 JSON 的 `Body` 字段，因为此时请求体会使用 `application/x-www-form-urlencoded` 而不是 `application/json` 进行编码。
>
> 这不是 **FastAPI** 的限制，而是 HTTP 协议的一部分。



### 小结

使用 `Form` 来声明表单数据输入参数。





-----

----

## 表单模型

你可以在 FastAPI 中使用 **Pydantic 模型**声明**表单字段**。



### 表单的 Pydantic 模型

你只需声明一个 **Pydantic 模型**，其中包含你希望接收的**表单字段**，然后将参数声明为 `Form`：

```python
from typing import Annotated

from fastapi import FastAPI, Form
from pydantic import BaseModel

app = FastAPI()


class FormData(BaseModel):
    username: str
    password: str


@app.post("/login/")
async def login(data: Annotated[FormData, Form()]):
    return data
```



**FastAPI** 将从请求中的**表单数据**中**提取**出**每个字段**的数据，并提供你定义的 Pydantic 模型。





### 检查文档

你可以在文档 UI 中验证它，地址为 `/docs`：

![截屏2026-06-12 17.13.21](images/截屏2026-06-12 17.13.21.png)



### 禁止额外的表单字段

在某些特殊使用情况下（可能并不常见），你可能希望将表单字段**限制**为仅在 Pydantic 模型中声明过的字段，并**禁止**任何**额外**的字段。你可以使用 Pydantic 的模型配置来 `forbid` 任何 `extra` 字段：

```python
from typing import Annotated

from fastapi import FastAPI, Form
from pydantic import BaseModel

app = FastAPI()


class FormData(BaseModel):
    username: str
    password: str
    model_config = {"extra": "forbid"}


@app.post("/login/")
async def login(data: Annotated[FormData, Form()]):
    return data
```

如果客户端尝试发送一些额外的数据，他们将收到**错误**响应。

例如，客户端尝试发送如下表单字段：

- `username`: `Rick`
- `password`: `Portal Gun`
- `extra`: `Mr. Poopybutthole`

他们将收到一条错误响应，表明字段 `extra` 不被允许：

```python
{
    "detail": [
        {
            "type": "extra_forbidden",
            "loc": ["body", "extra"],
            "msg": "Extra inputs are not permitted",
            "input": "Mr. Poopybutthole"
        }
    ]
}
```



### 总结

你可以使用 Pydantic 模型在 FastAPI 中声明表单字段。😎







---

---

## 请求文件

你可以使用 `File` 定义由客户端上传的文件。

### 导入 `File`

从 `fastapi` 导入 `File` 和 `UploadFile`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/request-files/#__tabbed_1_1)

```python
from typing import Annotated

from fastapi import FastAPI, File, UploadFile

app = FastAPI()


@app.post("/files/")
async def create_file(file: Annotated[bytes, File()]):
    return {"file_size": len(file)}


@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile):
    return {"filename": file.filename}
```



### 定义 `File` 参数

像为 `Body` 或 `Form` 一样创建文件参数：

```python
async def create_file(file: Annotated[bytes, File()]):
```



> ⚠️
>
> `File` 是直接继承自 `Form` 的类。
>
> 但要注意，从 `fastapi` 导入的 `Query`、`Path`、`File` 等项，实际上是返回特定类的函数。



> 🔔
>
> 声明文件体必须使用 `File`，否则，这些参数会被当作查询参数或请求体（JSON）参数。

文件将作为「表单数据」上传。

如果把*路径操作函数*参数的类型声明为 `bytes`，**FastAPI** 会为你读取文件，并以 `bytes` 的形式接收其内容。

请注意，这意味着整个内容会存储在内存中，适用于小型文件。

不过，在很多情况下，使用 `UploadFile` 会更有优势。



### 含 `UploadFile` 的文件参数

将文件参数的类型声明为 `UploadFile`：

```python
async def create_upload_file(file: UploadFile):
    return {"filename": file.filename}
```



与 `bytes` 相比，使用 `UploadFile` 有多项优势：

- 无需在参数的默认值中使用 `File()`。
- 它使用“spooled”文件：
  - 文件会先存储在内存中，直到达到最大上限，超过该上限后会写入磁盘。
- 因此，非常适合处理图像、视频、大型二进制等大文件，而不会占用所有内存。
- 你可以获取上传文件的元数据。
- 它提供 [file-like](https://docs.python.org/3/glossary.html#term-file-like-object) 的 `async` 接口。
- 它暴露了一个实际的 Python [`SpooledTemporaryFile`](https://docs.python.org/3/library/tempfile.html#tempfile.SpooledTemporaryFile) 对象，你可以直接传给期望「file-like」对象的其他库。

#### `UploadFile`

`UploadFile` 的属性如下：

- `filename`：上传的原始文件名字符串（`str`），例如 `myimage.jpg`。
- `content_type`：内容类型（MIME 类型 / 媒体类型）的字符串（`str`），例如 `image/jpeg`。
- `file`：[`SpooledTemporaryFile`](https://docs.python.org/3/library/tempfile.html#tempfile.SpooledTemporaryFile)（一个 [file-like](https://docs.python.org/3/glossary.html#term-file-like-object) 对象）。这是实际的 Python 文件对象，你可以直接传递给其他期望「file-like」对象的函数或库。

`UploadFile` 具有以下 `async` 方法。它们都会在底层调用对应的文件方法（使用内部的 `SpooledTemporaryFile`）。

- `write(data)`：将 `data` (`str` 或 `bytes`) 写入文件。

- `read(size)`：读取文件中 `size` (`int`) 个字节/字符。

- `seek(offset)`：移动到文件中字节位置`offset` (`int`)。
  - 例如，`await myfile.seek(0)` 会移动到文件开头。
  - 如果你先运行过 `await myfile.read()`，然后需要再次读取内容时，这尤其有用。

- `close()`：关闭文件。

由于这些方法都是 `async` 方法，你需要对它们使用 await。

例如，在 `async` *路径操作函数* 内，你可以这样获取内容：

```python
contents = await myfile.read()
```

如果是在普通 `def` *路径操作函数* 内，你可以直接访问 `UploadFile.file`，例如：

```python
contents = myfile.file.read()
```





> 🖊️ `async`技术细节
>
> 当你使用这些 `async` 方法时，**FastAPI** 会在线程池中运行相应的文件方法并等待其完成。



----

### 什么是「表单数据」

HTML 表单（`<form></form>`）向服务器发送数据的方式通常会对数据使用一种「特殊」的编码，这与 JSON 不同。

**FastAPI** 会确保从正确的位置读取这些数据，而不是从 JSON 中读取。



> 🔔 技术细节
>
> 当不包含文件时，来自表单的数据通常使用「媒体类型」`application/x-www-form-urlencoded` 编码。
>
> 但当表单包含文件时，会编码为 `multipart/form-data`。如果你使用 `File`，**FastAPI** 会知道需要从请求体的正确位置获取文件。
>
> 如果你想进一步了解这些编码和表单字段，请参阅 [MDN 关于 `POST` 的 Web 文档](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)。





### 可选文件上传

你可以通过使用标准类型注解并将 `None` 作为默认值的方式将一个文件参数设为可选:

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/request-files/#__tabbed_7_1)

```python
from typing import Annotated

from fastapi import FastAPI, File, UploadFile

app = FastAPI()


@app.post("/files/")
async def create_file(file: Annotated[bytes | None, File()] = None):
    if not file:
        return {"message": "No file sent"}
    else:
        return {"file_size": len(file)}


@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile | None = None):
    if not file:
        return {"message": "No upload file sent"}
    else:
        return {"filename": file.filename}
```



### 带有额外元数据的 `UploadFile`

你也可以将 `File()` 与 `UploadFile` 一起使用，例如，设置额外的元数据:

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/request-files/#__tabbed_9_1)

```python
from typing import Annotated

from fastapi import FastAPI, File, UploadFile

app = FastAPI()


@app.post("/files/")
async def create_file(file: Annotated[bytes, File(description="A file read as bytes")]):
    return {"file_size": len(file)}


@app.post("/uploadfile/")
async def create_upload_file(
    file: Annotated[UploadFile, File(description="A file read as UploadFile")],
):
    return {"filename": file.filename}
```

### 多文件上传

FastAPI 支持同时上传多个文件。

它们会被关联到同一个通过「表单数据」发送的「表单字段」。

要实现这一点，声明一个由 `bytes` 或 `UploadFile` 组成的列表（`List`）：

```python
from typing import Annotated

from fastapi import FastAPI, File, UploadFile
from fastapi.responses import HTMLResponse

app = FastAPI()


@app.post("/files/")
async def create_files(files: Annotated[list[bytes], File()]):
    return {"file_sizes": [len(file) for file in files]}


@app.post("/uploadfiles/")
async def create_upload_files(files: list[UploadFile]):
    return {"filenames": [file.filename for file in files]}


@app.get("/")
async def main():
    content = """
<body>
<form action="/files/" enctype="multipart/form-data" method="post">
<input name="files" type="file" multiple>
<input type="submit">
</form>
<form action="/uploadfiles/" enctype="multipart/form-data" method="post">
<input name="files" type="file" multiple>
<input type="submit">
</form>
</body>
    """
    return HTMLResponse(content=content)
```



接收的也是含 `bytes` 或 `UploadFile` 的列表（`list`）。



#### 带有额外元数据的多文件上传

和之前的方式一样，你可以为 `File()` 设置额外参数，即使是 `UploadFile`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/request-files/#__tabbed_13_1)

```python
from typing import Annotated

from fastapi import FastAPI, File, UploadFile
from fastapi.responses import HTMLResponse

app = FastAPI()


@app.post("/files/")
async def create_files(
    files: Annotated[list[bytes], File(description="Multiple files as bytes")],
):
    return {"file_sizes": [len(file) for file in files]}


@app.post("/uploadfiles/")
async def create_upload_files(
    files: Annotated[
        list[UploadFile], File(description="Multiple files as UploadFile")
    ],
):
    return {"filenames": [file.filename for file in files]}


@app.get("/")
async def main():
    content = """
<body>
<form action="/files/" enctype="multipart/form-data" method="post">
<input name="files" type="file" multiple>
<input type="submit">
</form>
<form action="/uploadfiles/" enctype="multipart/form-data" method="post">
<input name="files" type="file" multiple>
<input type="submit">
</form>
</body>
    """
    return HTMLResponse(content=content)
```





### 小结

使用 `File`、`bytes` 和 `UploadFile` 来声明在请求中上传的文件，它们以表单数据发送。





------

-----

## 请求表单与文件

FastAPI 支持同时使用 `File` 和 `Form` 定义文件和表单字段。

### 导入 `File` 与 `Form`

```python
from typing import Annotated

from fastapi import FastAPI, File, Form, UploadFile

app = FastAPI()


@app.post("/files/")
async def create_file(
    file: Annotated[bytes, File()],
    fileb: Annotated[UploadFile, File()],
    token: Annotated[str, Form()],
):
    return {
        "file_size": len(file),
        "token": token,
        "fileb_content_type": fileb.content_type,
    }
```



### 定义 `File` 与 `Form` 参数

创建文件和表单参数的方式与 `Body` 和 `Query` 一样：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/request-forms-and-files/#__tabbed_3_1)

```python
from typing import Annotated

from fastapi import FastAPI, File, Form, UploadFile

app = FastAPI()


@app.post("/files/")
async def create_file(
    file: Annotated[bytes, File()],
    fileb: Annotated[UploadFile, File()],
    token: Annotated[str, Form()],
):
    return {
        "file_size": len(file),
        "token": token,
        "fileb_content_type": fileb.content_type,
    }
```

文件和表单字段作为表单数据上传与接收。

并且你可以将部分文件声明为 `bytes`，将部分文件声明为 `UploadFile`。

> ⚠️注意
>
> 可在一个*路径操作*中声明多个 `File` 与 `Form` 参数，但不能同时声明要接收 JSON 的 `Body` 字段。因为此时请求体的编码为 `multipart/form-data`，不是 `application/json`。
>
> 这不是 **FastAPI** 的问题，而是 HTTP 协议的规定。



### 小结

在同一个请求中接收数据和文件时，应同时使用 `File` 和 `Form`。





------

-----

## 处理错误

某些情况下，需要向使用你的 API 的客户端返回错误提示。

这里所谓的客户端包括前端浏览器、他人的代码、物联网设备等。

你可能需要告诉客户端：

- 客户端没有执行该操作的权限
- 客户端没有访问该资源的权限
- 客户端要访问的项目不存在
- 等等

遇到这些情况时，通常要返回 **4XX**（400 至 499）**HTTP 状态码**。

这与表示请求成功的 **2XX**（200 至 299）HTTP 状态码类似。那些“200”状态码表示某种程度上的“成功”。

而 **4XX** 状态码表示客户端发生了错误。

大家都知道**「404 Not Found」**错误，还有调侃这个错误的笑话吧？



### 使用 `HTTPException`

向客户端返回 HTTP 错误响应，可以使用 `HTTPException`。

### 导入 `HTTPException`

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

items = {"foo": "The Foo Wrestlers"}


@app.get("/items/{item_id}")
async def read_item(item_id: str):
    if item_id not in items:
        raise HTTPException(status_code=404, detail="Item not found")
    return {"item": items[item_id]}
```





#### 在代码中触发 `HTTPException`

`HTTPException` 是额外包含了和 API 有关数据的常规 Python 异常。

因为是 Python 异常，所以不能 `return`，只能 `raise`。

这也意味着，如果你在*路径操作函数*里调用的某个工具函数内部触发了 `HTTPException`，那么*路径操作函数*中后续的代码将不会继续执行，请求会立刻终止，并把 `HTTPException` 的 HTTP 错误发送给客户端。

在介绍依赖项与安全的章节中，你可以更直观地看到用 `raise` 异常代替 `return` 值的优势。

本例中，客户端用不存在的 `ID` 请求 `item` 时，触发状态码为 `404` 的异常：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/handling-errors/#__tabbed_2_1)

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

items = {"foo": "The Foo Wrestlers"}


@app.get("/items/{item_id}")
async def read_item(item_id: str):
    if item_id not in items:
        raise HTTPException(status_code=404, detail="Item not found")
    return {"item": items[item_id]}
```





#### 响应结果

请求为 `http://example.com/items/foo`（`item_id` 为 `"foo"`）时，客户端会接收到 HTTP 状态码 200 及如下 JSON 响应结果：

```python
{
  "item": "The Foo Wrestlers"
}
```

但如果客户端请求 `http://example.com/items/bar`（不存在的 `item_id` `"bar"`），则会接收到 HTTP 状态码 404（“未找到”错误）及如下 JSON 响应结果：

```python
{
  "detail": "Item not found"
}
```

> 🔔 Tips
>
> 触发 `HTTPException` 时，可以用参数 `detail` 传递任何能转换为 JSON 的值，不仅限于 `str`。
>
> 还支持传递 `dict`、`list` 等数据结构。
>
> **FastAPI** 能自动处理这些数据，并将之转换为 JSON。
>
> ---



### 添加自定义响应头

有些场景下要为 HTTP 错误添加自定义响应头。例如，出于某些类型的安全需要。

一般情况下你可能不会在代码中直接使用它。

但在某些高级场景中需要时，你可以添加自定义响应头：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/handling-errors/#__tabbed_3_1)

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

items = {"foo": "The Foo Wrestlers"}


@app.get("/items-header/{item_id}")
async def read_item_header(item_id: str):
    if item_id not in items:
        raise HTTPException(
            status_code=404,
            detail="Item not found",
            headers={"X-Error": "There goes my error"},
        )
    return {"item": items[item_id]}
```





### 安装自定义异常处理器

可以使用[与 Starlette 相同的异常处理工具](https://www.starlette.dev/exceptions/)添加自定义异常处理器。

假设有一个自定义异常 `UnicornException`（你自己或你使用的库可能会 `raise` 它）。

并且你希望用 FastAPI 在全局处理该异常。

此时，可以用 `@app.exception_handler()` 添加自定义异常处理器：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/handling-errors/#__tabbed_4_1)

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse


class UnicornException(Exception):
    def __init__(self, name: str):
        self.name = name


app = FastAPI()


@app.exception_handler(UnicornException)
async def unicorn_exception_handler(request: Request, exc: UnicornException):
    return JSONResponse(
        status_code=418,
        content={"message": f"Oops! {exc.name} did something. There goes a rainbow..."},
    )


@app.get("/unicorns/{name}")
async def read_unicorn(name: str):
    if name == "yolo":
        raise UnicornException(name=name)
    return {"unicorn_name": name}
```





这里，请求 `/unicorns/yolo` 时，路径操作会触发 `UnicornException`。

但该异常将会被 `unicorn_exception_handler` 处理。

你会收到清晰的错误信息，HTTP 状态码为 `418`，JSON 内容如下：

```python
{"message": "Oops! yolo did something. There goes a rainbow..."}
```



### 覆盖默认异常处理器

**FastAPI** 自带了一些默认异常处理器。

当你触发 `HTTPException`，或者请求中包含无效数据时，这些处理器负责返回默认的 JSON 响应。

你也可以用自己的处理器覆盖它们。

#### 覆盖请求验证异常

请求中包含无效数据时，**FastAPI** 内部会触发 `RequestValidationError`。

它也内置了该异常的默认处理器。

要覆盖它，导入 `RequestValidationError`，并用 `@app.exception_handler(RequestValidationError)` 装饰你的异常处理器。

异常处理器会接收 `Request` 和该异常。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/handling-errors/#__tabbed_5_1)

```python
from fastapi import FastAPI, HTTPException
from fastapi.exceptions import RequestValidationError
from fastapi.responses import PlainTextResponse
from starlette.exceptions import HTTPException as StarletteHTTPException

app = FastAPI()


@app.exception_handler(StarletteHTTPException)
async def http_exception_handler(request, exc):
    return PlainTextResponse(str(exc.detail), status_code=exc.status_code)


@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request, exc: RequestValidationError):
    message = "Validation errors:"
    for error in exc.errors():
        message += f"\nField: {error['loc']}, Error: {error['msg']}"
    return PlainTextResponse(message, status_code=400)


@app.get("/items/{item_id}")
async def read_item(item_id: int):
    if item_id == 3:
        raise HTTPException(status_code=418, detail="Nope! I don't like 3.")
    return {"item_id": item_id}
```





现在，访问 `/items/foo` 时，默认的 JSON 错误为：

```json
{
    "detail": [
        {
            "loc": [
                "path",
                "item_id"
            ],
            "msg": "value is not a valid integer",
            "type": "type_error.integer"
        }
    ]
}
```

将得到如下文本内容：

```python
Validation errors:
Field: ('path', 'item_id'), Error: Input should be a valid integer, unable to parse string as an integer
```

#### 覆盖 `HTTPException` 错误处理器

同理，也可以覆盖 `HTTPException` 的处理器。

例如，只为这些错误返回纯文本响应，而不是 JSON：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/handling-errors/#__tabbed_6_1)

```python
from fastapi import FastAPI, HTTPException
from fastapi.exceptions import RequestValidationError
from fastapi.responses import PlainTextResponse
from starlette.exceptions import HTTPException as StarletteHTTPException

app = FastAPI()


@app.exception_handler(StarletteHTTPException)
async def http_exception_handler(request, exc):
    return PlainTextResponse(str(exc.detail), status_code=exc.status_code)


@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request, exc: RequestValidationError):
    message = "Validation errors:"
    for error in exc.errors():
        message += f"\nField: {error['loc']}, Error: {error['msg']}"
    return PlainTextResponse(message, status_code=400)


@app.get("/items/{item_id}")
async def read_item(item_id: int):
    if item_id == 3:
        raise HTTPException(status_code=418, detail="Nope! I don't like 3.")
    return {"item_id": item_id}
```



> 🫵 警告
>
> 请注意，`RequestValidationError` 包含发生验证错误的文件名和行号信息，你可以在需要时将其记录到日志中以提供相关信息。
>
> 但这也意味着，如果你只是将其直接转换为字符串并返回，可能会泄露一些关于系统的细节信息。因此，这里的代码会提取并分别显示每个错误。
>
>



#### 使用 `RequestValidationError` 的请求体

`RequestValidationError` 包含其接收到的带有无效数据的请求体 `body`。

开发时，你可以用它来记录请求体、调试错误，或返回给用户等。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/handling-errors/#__tabbed_7_1)

```python
from fastapi import FastAPI, Request
from fastapi.encoders import jsonable_encoder
from fastapi.exceptions import RequestValidationError
from fastapi.responses import JSONResponse
from pydantic import BaseModel

app = FastAPI()


@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request: Request, exc: RequestValidationError):
    return JSONResponse(
        status_code=422,
        content=jsonable_encoder({"detail": exc.errors(), "body": exc.body}),
    )


class Item(BaseModel):
    title: str
    size: int


@app.post("/items/")
async def create_item(item: Item):
    return item
```





现在试着发送一个无效的 `item`，例如：

```python
{
  "title": "towel",
  "size": "XL"
}
```

原来抛出的错误是这样的：

```python
{
  "detail": [
    { "loc": ["body", "size"], "msg": "Input should be a valid integer...", "type": "int_parsing" }
  ]
}
```



修改后收到的响应会告诉你数据无效，并包含收到的请求体：

```python
{
  "detail": [
    {
      "loc": [
        "body",
        "size"
      ],
      "msg": "value is not a valid integer",
      "type": "type_error.integer"
    }
  ],
  "body": {
    "title": "towel",
    "size": "XL"
  }
}
```

##### FastAPI 的 `HTTPException` vs Starlette 的 `HTTPException`

`FatAPI` 也提供了自有的 HTTPException。

**FastAPI** 的 `HTTPException` 错误类继承自 Starlette 的 `HTTPException` 错误类。

它们之间的唯一区别是，**FastAPI** 的 `HTTPException` 在 `detail` 字段中接受任意可转换为 JSON 的数据，而 Starlette 的 `HTTPException` 只接受字符串。

因此，你可以继续像平常一样在代码中触发 **FastAPI** 的 `HTTPException`。

但注册异常处理器时，应该注册到来自 Starlette 的 `HTTPException`。

这样做是为了，当 Starlette 的内部代码、扩展或插件触发 Starlette `HTTPException` 时，你的处理器能够捕获并处理它。

本例中，为了在同一份代码中同时使用两个 `HTTPException`，将 Starlette 的异常重命名为 `StarletteHTTPException`：

```python
from starlette.exceptions import HTTPException as StarletteHTTPException
```



#### 复用 **FastAPI** 的异常处理器

如果你想在自定义处理后仍复用 **FastAPI** 的默认异常处理器，可以从 `fastapi.exception_handlers` 导入并复用这些默认处理器：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/handling-errors/#__tabbed_8_1)

```python
from fastapi import FastAPI, HTTPException
from fastapi.exception_handlers import (
    http_exception_handler,
    request_validation_exception_handler,
)
from fastapi.exceptions import RequestValidationError
from starlette.exceptions import HTTPException as StarletteHTTPException

app = FastAPI()


@app.exception_handler(StarletteHTTPException)
async def custom_http_exception_handler(request, exc):
    print(f"OMG! An HTTP error!: {repr(exc)}")
    return await http_exception_handler(request, exc)


@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request, exc):
    print(f"OMG! The client sent invalid data!: {exc}")
    return await request_validation_exception_handler(request, exc)


@app.get("/items/{item_id}")
async def read_item(item_id: int):
    if item_id == 3:
        raise HTTPException(status_code=418, detail="Nope! I don't like 3.")
    return {"item_id": item_id}
```





虽然本例只是用非常夸张的信息打印了错误，但足以说明：你可以先处理异常，然后再复用默认的异常处理器。



--------

-------

## 路径操作配置

路径操作装饰器支持多种配置参数



> ⚠️
>
> 注意：一下操作参数应直接传给路径操作装饰器，不能传递给路径操作函数。



### 响应状态码

可以在*路径操作*的响应中定义（HTTP）`status_code`。

可以直接传递 `int` 代码，比如 `404`。

如果记不住数字码的含义，也可以用 `status` 的快捷常量：

```python
from fastapi import FastAPI, status
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()


@app.post("/items/", status_code=status.HTTP_201_CREATED)
async def create_item(item: Item) -> Item:
    return item
```

该状态码会用于响应中，并会被添加到 OpenAPI 概图。



### 标签

可以通过传入由 `str` 组成的 `list`（通常只有一个 `str`）的参数 `tags`，为*路径操作*添加标签：

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()


@app.post("/items/", tags=["items"])
async def create_item(item: Item) -> Item:
    return item


@app.get("/items/", tags=["items"])
async def read_items():
    return [{"name": "Foo", "price": 42}]


@app.get("/users/", tags=["users"])
async def read_users():
    return [{"username": "johndoe"}]
```

OpenAPI 概图会自动添加标签，供 API 文档接口使用：



![截屏2026-06-13 11.53.18](images/截屏2026-06-13 11.53.18.png)

#### 使用 Enum 的标签

如果你的应用很大，可能会积累出很多标签，你会希望确保相关的*路径操作*始终使用相同的标签。

这种情况下，把标签存放在 `Enum` 中会更合适。

**FastAPI** 对此的支持与使用普通字符串相同：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/path-operation-configuration/#__tabbed_3_1)

```python
from enum import Enum

from fastapi import FastAPI

app = FastAPI()


class Tags(Enum):
    items = "items"
    users = "users"


@app.get("/items/", tags=[Tags.items])
async def get_items():
    return ["Portal gun", "Plumbus"]


@app.get("/users/", tags=[Tags.users])
async def read_users():
    return ["Rick", "Morty"]
```





### 摘要和描述

可以添加 `summary` 和 `description`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/path-operation-configuration/#__tabbed_4_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()


@app.post(
    "/items/",
    summary="Create an item",
    description="Create an item with all the information, name, description, price, tax and a set of unique tags",
)
async def create_item(item: Item) -> Item:
    return item
```



OpenAPI 概图:

![截屏2026-06-13 12.03.37](images/截屏2026-06-13 12.03.37.png)



### 从 docstring 获取描述

描述内容比较长且占用多行时，可以在函数的 docstring 中声明*路径操作*的描述，**FastAPI** 会从中读取。

文档字符串支持 [Markdown](https://en.wikipedia.org/wiki/Markdown)，能正确解析和显示 Markdown 的内容，但要注意文档字符串的缩进。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/path-operation-configuration/#__tabbed_5_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()


@app.post("/items/", summary="Create an item")
async def create_item(item: Item) -> Item:
    """
    Create an item with all the information:

    - **name**: each item must have a name
    - **description**: a long description
    - **price**: required
    - **tax**: if the item doesn't have tax, you can omit this
    - **tags**: a set of unique tag strings for this item
    """
    return item
```





它会在交互式文档中使用：



![截屏2026-06-13 12.08.36](images/截屏2026-06-13 12.08.36.png)

### 响应描述

`response_description` 参数用于定义响应的描述说明：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/path-operation-configuration/#__tabbed_6_1)

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str | None = None
    price: float
    tax: float | None = None
    tags: set[str] = set()


@app.post(
    "/items/",
    summary="Create an item",
    response_description="The created item",
)
async def create_item(item: Item) -> Item:
    """
    Create an item with all the information:

    - **name**: each item must have a name
    - **description**: a long description
    - **price**: required
    - **tax**: if the item doesn't have tax, you can omit this
    - **tags**: a set of unique tag strings for this item
    """
    return item
```



> 🔥 `Tips`
>
> 注意，`response_description` 只用于描述响应，`description` 一般则用于描述*路径操作*。

### 弃用*路径操作*

如果需要把*路径操作*标记为弃用，但不删除它，可以传入 `deprecated` 参数：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/path-operation-configuration/#__tabbed_7_1)

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items/", tags=["items"])
async def read_items():
    return [{"name": "Foo", "price": 42}]


@app.get("/users/", tags=["users"])
async def read_users():
    return [{"username": "johndoe"}]


@app.get("/elements/", tags=["items"], deprecated=True)
async def read_elements():
    return [{"item_id": "Foo"}]
```





API 文档会把该路径操作标记为弃用：



![截屏2026-06-13 12.17.55](images/截屏2026-06-13 12.17.55.png)



### 小结

通过传递参数给*路径操作装饰器*，即可轻松地配置*路径操作*、添加元数据。







-----

------

## JSON 兼容编码器

在某些情况下，您可能需要将数据类型（如Pydantic模型）转换为与JSON兼容的数据类型（如`dict`、`list`等）。

比如，如果您需要将其存储在数据库中。

对于这种要求， **FastAPI**提供了`jsonable_encoder()`函数。

### 使用`jsonable_encoder`

让我们假设你有一个数据库名为`fake_db`，它只能接收与JSON兼容的数据。

例如，它不接收`datetime`这类的对象，因为这些对象与JSON不兼容。

因此，`datetime`对象必须转换为包含[ISO 格式](https://en.wikipedia.org/wiki/ISO_8601)的`str`类型对象。

同样，这个数据库也不会接收Pydantic模型（带有属性的对象），而只接收`dict`。

对此你可以使用`jsonable_encoder`。

它接收一个对象，比如Pydantic模型，并会返回一个JSON兼容的版本：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/encoder/#__tabbed_1_1)

```python
from datetime import datetime

from fastapi import FastAPI
from fastapi.encoders import jsonable_encoder
from pydantic import BaseModel

fake_db = {}


class Item(BaseModel):
    title: str
    timestamp: datetime
    description: str | None = None


app = FastAPI()


@app.put("/items/{id}")
def update_item(id: str, item: Item):
    json_compatible_item_data = jsonable_encoder(item)
    fake_db[id] = json_compatible_item_data
```





在这个例子中，它将Pydantic模型转换为`dict`，并将`datetime`转换为`str`。

调用它的结果后就可以使用Python标准编码中的[`json.dumps()`](https://docs.python.org/3/library/json.html#json.dumps)。

这个操作不会返回一个包含JSON格式（作为字符串）数据的庞大的`str`。它将返回一个Python标准数据结构（例如`dict`），其值和子值都与JSON兼容。





> !注意
>
> `jsonable_encoder`实际上是**FastAPI**内部用来转换数据的。但是它在许多其他场景中也很有用。







---

-----

## 请求体 - 更新数据

### 用 `PUT` 替换式更新

更新数据可以使用 [HTTP `PUT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT) 操作。

把输入数据转换为以 JSON 格式存储的数据（比如，使用 NoSQL 数据库时），可以使用 `jsonable_encoder`。例如，把 `datetime` 转换为 `str`。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-updates/#__tabbed_1_1)

```python
from fastapi import FastAPI
from fastapi.encoders import jsonable_encoder
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str | None = None
    description: str | None = None
    price: float | None = None
    tax: float = 10.5
    tags: list[str] = []


items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The bartenders", "price": 62, "tax": 20.2},
    "baz": {"name": "Baz", "description": None, "price": 50.2, "tax": 10.5, "tags": []},
}


@app.get("/items/{item_id}", response_model=Item)
async def read_item(item_id: str):
    return items[item_id]


@app.put("/items/{item_id}", response_model=Item)
async def update_item(item_id: str, item: Item):
    update_item_encoded = jsonable_encoder(item)
    items[item_id] = update_item_encoded
    return update_item_encoded
```





`PUT` 用于接收替换现有数据的数据。

#### 关于替换的警告

```python
{
    "name": "Barz",
    "price": 3,
    "description": None,
}
```

因为其中未包含已存储的属性 `"tax": 20.2`，输入模型会取 `"tax": 10.5` 的默认值。

因此，保存的数据会带有这个“新的” `tax` 值 `10.5`。

### 用 `PATCH` 进行部分更新

也可以使用 [HTTP `PATCH`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH) 操作对数据进行*部分*更新。

也就是说，你只需发送想要更新的数据，其余数据保持不变。



#### 使用 Pydantic 的 `exclude_unset` 参数

如果要接收部分更新，建议在 Pydantic 模型的 `.model_dump()` 中使用 `exclude_unset` 参数。

比如，`item.model_dump(exclude_unset=True)`。

这会生成一个 `dict`，只包含创建 `item` 模型时显式设置的数据，不包含默认值。

然后再用它生成一个只含已设置（在请求中发送）数据、且省略默认值的 `dict`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-updates/#__tabbed_2_1)

```python
from fastapi import FastAPI
from fastapi.encoders import jsonable_encoder
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str | None = None
    description: str | None = None
    price: float | None = None
    tax: float = 10.5
    tags: list[str] = []


items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The bartenders", "price": 62, "tax": 20.2},
    "baz": {"name": "Baz", "description": None, "price": 50.2, "tax": 10.5, "tags": []},
}


@app.get("/items/{item_id}", response_model=Item)
async def read_item(item_id: str):
    return items[item_id]


@app.patch("/items/{item_id}")
async def update_item(item_id: str, item: Item) -> Item:
    stored_item_data = items[item_id]
    stored_item_model = Item(**stored_item_data)
    update_data = item.model_dump(exclude_unset=True)
    updated_item = stored_item_model.model_copy(update=update_data)
    items[item_id] = jsonable_encoder(updated_item)
    return updated_item
```





#### 使用 Pydantic 的 `update` 参数

接下来，用 `.model_copy()` 为已有模型创建副本，并传入 `update` 参数，值为包含更新数据的 `dict`。

例如，`stored_item_model.model_copy(update=update_data)`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-updates/#__tabbed_3_1)

```python
from fastapi import FastAPI
from fastapi.encoders import jsonable_encoder
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str | None = None
    description: str | None = None
    price: float | None = None
    tax: float = 10.5
    tags: list[str] = []


items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The bartenders", "price": 62, "tax": 20.2},
    "baz": {"name": "Baz", "description": None, "price": 50.2, "tax": 10.5, "tags": []},
}


@app.get("/items/{item_id}", response_model=Item)
async def read_item(item_id: str):
    return items[item_id]


@app.patch("/items/{item_id}")
async def update_item(item_id: str, item: Item) -> Item:
    stored_item_data = items[item_id]
    stored_item_model = Item(**stored_item_data)
    update_data = item.model_dump(exclude_unset=True)
    updated_item = stored_item_model.model_copy(update=update_data)
    items[item_id] = jsonable_encoder(updated_item)
    return updated_item
```





#### 部分更新小结

简而言之，应用部分更新应当：

- （可选）使用 `PATCH` 而不是 `PUT`。

- 提取已存储的数据。

- 把该数据放入 Pydantic 模型。

- 生成不含输入模型默认值的`dict`（使用 `exclude_unset`）。

  - 这样只会更新用户实际设置的值，而不会用模型中的默认值覆盖已存储的值。

- 为已存储的模型创建副本，用接收到的部分更新数据更新其属性（使用 `update` 参数）。

- 把模型副本转换为可存入数据库的形式（比如，使用`jsonable_encoder`）。

  - 这类似于再次调用模型的 `.model_dump()` 方法，但会确保（并转换）值为可转换为 JSON 的数据类型，例如把 `datetime` 转换为 `str`。

- 把数据保存至数据库。

- 返回更新后的模型。

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/body-updates/#__tabbed_4_1)

```python
from fastapi import FastAPI
from fastapi.encoders import jsonable_encoder
from pydantic import BaseModel

app = FastAPI()


class Item(BaseModel):
    name: str | None = None
    description: str | None = None
    price: float | None = None
    tax: float = 10.5
    tags: list[str] = []


items = {
    "foo": {"name": "Foo", "price": 50.2},
    "bar": {"name": "Bar", "description": "The bartenders", "price": 62, "tax": 20.2},
    "baz": {"name": "Baz", "description": None, "price": 50.2, "tax": 10.5, "tags": []},
}


@app.get("/items/{item_id}", response_model=Item)
async def read_item(item_id: str):
    return items[item_id]


@app.patch("/items/{item_id}")
async def update_item(item_id: str, item: Item) -> Item:
    stored_item_data = items[item_id]
    stored_item_model = Item(**stored_item_data)
    update_data = item.model_dump(exclude_unset=True)
    updated_item = stored_item_model.model_copy(update=update_data)
    items[item_id] = jsonable_encoder(updated_item)
    return updated_item
```









----

----



## 依赖项

**FastAPI** 提供了简单直观但功能强大的**依赖注入**系统。

它被设计得非常易用，能让任何开发者都能轻松把其他组件与 **FastAPI** 集成。

### 什么是「依赖注入」

在编程中，**「依赖注入」**指的是，你的代码（本文中为*路径操作函数*）声明其运行所需并要使用的东西：“依赖”。

然后，由该系统（本文中为 **FastAPI**）负责执行所有必要的逻辑，为你的代码提供这些所需的依赖（“注入”依赖）。

当你需要以下内容时，这非常有用：

- 共享业务逻辑（同一段代码逻辑反复复用）
- 共享数据库连接
- 实施安全、认证、角色权限等要求
- 以及更多其他内容...

同时尽量减少代码重复。



### 第一步

先来看一个非常简单的例子。它现在简单到几乎没什么用。

但这样我们就可以专注于**依赖注入**系统是如何工作的。



#### 创建依赖项，或“dependable”

首先关注依赖项。

它只是一个函数，且可以接收与*路径操作函数*相同的所有参数：

```python
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


async def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}


@app.get("/items/")
async def read_items(commons: Annotated[dict, Depends(common_parameters)]):
    return commons


@app.get("/users/")
async def read_users(commons: Annotated[dict, Depends(common_parameters)]):
    return commons
```

它的形式和结构与所有*路径操作函数*相同。

你可以把它当作没有“装饰器”（没有 `@app.get("/some-path")`）的*路径操作函数*。

而且它可以返回任何你想要的内容。

本例中的依赖项预期接收：

- 类型为 `str` 的可选查询参数 `q`
- 类型为 `int` 的可选查询参数 `skip`，默认值 `0`
- 类型为 `int` 的可选查询参数 `limit`，默认值 `100`

然后它只需返回一个包含这些值的 `dict`。



---

#### 导入Depends

```python
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


async def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}


@app.get("/items/")
async def read_items(commons: Annotated[dict, Depends(common_parameters)]):
    return commons


@app.get("/users/")
async def read_users(commons: Annotated[dict, Depends(common_parameters)]):
    return commons
```



#### 在“dependant”中声明依赖项

与在*路径操作函数*的参数中使用 `Body`、`Query` 等相同，给参数使用 `Depends` 来声明一个新的依赖项：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/#__tabbed_5_1)

```python 
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


async def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}


@app.get("/items/")
async def read_items(commons: Annotated[dict, Depends(common_parameters)]):
    return commons


@app.get("/users/")
async def read_users(commons: Annotated[dict, Depends(common_parameters)]):
    return commons
```



虽然你在函数参数中使用 `Depends` 的方式与 `Body`、`Query` 等相同，但 `Depends` 的工作方式略有不同。

这里只能给 `Depends` 传入一个参数。

这个参数必须是类似函数的可调用对象。

你不需要直接调用它（不要在末尾加括号），只需将其作为参数传给 `Depends()`。

该函数接收的参数与*路径操作函数*的参数相同。



```mermaid
graph TD
    %% 1. 定义节点关系
    A(common_parameters) --> B[/items/]
    A --> C[/users/]


```
接收到新的请求时，**FastAPI** 会负责：

- 用正确的参数调用你的依赖项（“dependable”）函数
- 获取函数返回的结果
- 将该结果赋值给你的*路径操作函数*中的参数

这样，你只需编写一次共享代码，**FastAPI** 会在你的*路径操作*中为你调用它。



> 🔔 
>
> 注意，无需创建专门的类并传给 **FastAPI** 去“注册”之类的操作。
>
> 只要把它传给 `Depends`，**FastAPI** 就知道该怎么做了。



---

### 共享 `Annotated` 依赖项

在上面的示例中，你会发现这里有一点点**代码重复**。

当你需要使用 `common_parameters()` 这个依赖时，你必须写出完整的带类型注解和 `Depends()` 的参数：

```python
commons: Annotated[dict, Depends(common_parameters)]
```

但因为我们使用了 `Annotated`，可以把这个 `Annotated` 的值存到一个变量里，在多个地方复用：

[Python 3.10+]()

```python
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


async def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}


CommonsDep = Annotated[dict, Depends(common_parameters)]


@app.get("/items/")
async def read_items(commons: CommonsDep):
    return commons


@app.get("/users/")
async def read_users(commons: CommonsDep):
    return commons
```

这些依赖会照常工作，而**最棒的是**，**类型信息会被保留**，这意味着你的编辑器依然能提供**自动补全**、**行内报错**等。同样适用于 `mypy` 等其他工具。

当你在**大型代码库**中，在**很多路径操作**里反复使用**相同的依赖**时，这会特别有用。



### 要不要使用 `async`？

由于依赖项也会由 **FastAPI** 调用（与*路径操作函数*相同），因此定义函数时同样的规则也适用。

你可以使用 `async def` 或普通的 `def`。

你可以在普通的 `def` *路径操作函数*中声明 `async def` 的依赖项；也可以在异步的 `async def` *路径操作函数*中声明普通的 `def` 依赖项，等等。

都没关系，**FastAPI** 知道该怎么处理。



### 与 OpenAPI 集成

依赖项及子依赖项中声明的所有请求、验证和需求都会集成到同一个 OpenAPI 模式中。

因此，交互式文档中也会包含这些依赖项的所有信息：

![截屏2026-06-14 14.32.16](images/截屏2026-06-14 14.32.16.png)

### 简单用法

观察一下就会发现，只要*路径*和*操作*匹配，就会使用声明的*路径操作函数*。随后，**FastAPI** 会用正确的参数调用该函数，并从请求中提取数据。

事实上，所有（或大多数）Web 框架的工作方式都是这样的。

你从不会直接调用这些函数。它们由你的框架（此处为 **FastAPI**）调用。

通过依赖注入系统，你还可以告诉 **FastAPI**，你的*路径操作函数*还“依赖”某些应在*路径操作函数*之前执行的内容，**FastAPI** 会负责执行它并“注入”结果。

“依赖注入”的其他常见术语包括：

- 资源（resources）
- 提供方（providers）
- 服务（services）
- 可注入（injectables）
- 组件（components）



----

### **FastAPI** 插件

可以使用**依赖注入**系统构建集成和“插件”。但实际上，根本**不需要创建“插件”**，因为通过依赖项可以声明无限多的集成与交互，使其可用于*路径操作函数*。

依赖项可以用非常简单直观的方式创建，你只需导入所需的 Python 包，用*字面意义上的*几行代码就能把它们与你的 API 函数集成起来。

在接下来的章节中，你会看到关于关系型数据库、NoSQL 数据库、安全等方面的示例。



### **FastAPI** 兼容性

依赖注入系统的简洁让 **FastAPI** 能与以下内容兼容：

- 各类关系型数据库
- NoSQL 数据库
- 外部包
- 外部 API
- 认证与授权系统
- API 使用监控系统
- 响应数据注入系统
- 等等...



### 简单而强大

虽然**层级式依赖注入系统**的定义与使用非常简单，但它依然非常强大。

你可以定义依赖其他依赖项的依赖项。

最终会构建出一个依赖项的层级树，**依赖注入**系统会处理所有这些依赖（及其子依赖），并在每一步提供（注入）相应的结果。

例如，假设你有 4 个 API 路径操作（*端点*）：

- `/items/public/`
- `/items/private/`
- `/users/{user_id}/activate`
- `/items/pro/`

你可以仅通过依赖项及其子依赖项为它们添加不同的权限要求：

```mermaid
graph TD
    %% 1. 定义节点关系
    A[current_user] --> B[active_user]
    A --> C[/items/public]
    B --> D[admin_user]
    B --> E(paying_user)
    B --> F(/items/private/)
    D --> G["/users/{user_id}/active"]
    E --> H[/items/pro/]

```



### 与 **OpenAPI** 集成

在声明需求的同时，所有这些依赖项也会为你的*路径操作*添加参数、验证等内容。





-----

----

## 类作为依赖项

在深入探究 **依赖注入** 系统之前，让我们升级之前的例子。



### 什么构成了依赖项

到目前为止，你看到的依赖项都被声明为函数。

但这并不是声明依赖项的唯一方法(尽管它可能是更常见的方法)。

关键因素是依赖项应该是 "可调用对象"。

Python 中的 "**可调用对象**" 是指任何 Python 可以像函数一样 "调用" 的对象。

所以，如果你有一个对象 `something` (可能*不是*一个函数)，你可以 "调用" 它(执行它)，就像：

```python
something()
```

或者

```python
something(some_argument, some_keyword_argument="foo")
```

这就是 "可调用对象"。



### 类作为依赖项

你可能会注意到，要创建一个Python类的实例，你可以使用相同的语法。

举个例子：

```python
class Cat:
    def __init__(self, name: str):
        self.name = name


fluffy = Cat(name="Mr Fluffy")
```

在这个例子中, `fluffy` 是一个 `Cat` 类的实例。

为了创建 `fluffy`，你调用了 `Cat` 。

所以，Python 类也是 **可调用对象**。

因此，在 **FastAPI** 中，你可以使用一个 Python 类作为一个依赖项。

实际上 FastAPI 检查的是它是一个 "可调用对象"（函数，类或其他任何类型）以及定义的参数。

如果你在 **FastAPI** 中传递一个 "可调用对象" 作为依赖项，它将分析该 "可调用对象" 的参数，并以处理路径操作函数的参数的方式来处理它们。包括子依赖项。

这也适用于完全没有参数的可调用对象。这与不带参数的路径操作函数一样。

所以，我们可以将上面的依赖项 "可依赖对象" `common_parameters` 更改为类 `CommonQueryParams`:

注意用于创建类实例的 `__init__` 方法：

...它与我们以前的 `common_parameters` 具有相同的参数：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/classes-as-dependencies/#__tabbed_3_1)

```python
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]


class CommonQueryParams:
    def __init__(self, q: str | None = None, skip: int = 0, limit: int = 100):
        self.q = q
        self.skip = skip
        self.limit = limit


@app.get("/items/")
async def read_items(commons: Annotated[CommonQueryParams, Depends(CommonQueryParams)]):
    response = {}
    if commons.q:
        response.update({"q": commons.q})
    items = fake_items_db[commons.skip : commons.skip + commons.limit]
    response.update({"items": items})
    return response
```

...它与我们以前的 `common_parameters` 具有相同的参数：

```python
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


async def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}


@app.get("/items/")
async def read_items(commons: Annotated[dict, Depends(common_parameters)]):
    return commons


@app.get("/users/")
async def read_users(commons: Annotated[dict, Depends(common_parameters)]):
    return commons
```



这些参数就是 **FastAPI** 用来 "处理" 依赖项的。

在两个例子下，都有：

- 一个可选的 `q` 查询参数，是 `str` 类型。
- 一个 `skip` 查询参数，是 `int` 类型，默认值为 `0`。
- 一个 `limit` 查询参数，是 `int` 类型，默认值为 `100`。

在两个例子下，数据都将被转换、验证、在 OpenAPI schema 上文档化，等等。

**FastAPI** 调用 `CommonQueryParams` 类。这将创建该类的一个 "实例"，该实例将作为参数 `commons` 被传递给你的函数。



----

### 类型注解 vs `Depends`

注意，我们在上面的代码中编写了两次`CommonQueryParams`：

```python
commons: Annotated[CommonQueryParams, Depends(CommonQueryParams)]
```

..实际上是 **FastAPI** 用来知道依赖项是什么的。

FastAPI 将从依赖项中提取声明的参数，这才是 FastAPI 实际调用的。

在本例中，第一个 `CommonQueryParams` ：

```python
commons: Annotated[CommonQueryParams, ...
```

...对于 **FastAPI** 没有任何特殊的意义。FastAPI 不会使用它进行数据转换、验证等 (因为对于这，它使用 `Depends(CommonQueryParams)`)。

实际上可以只这样编写:

```python
commons: Annotated[Any, Depends(CommonQueryParams)]
```

可以这样：

```python
from typing import Annotated, Any

from fastapi import Depends, FastAPI

app = FastAPI()


fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]


class CommonQueryParams:
    def __init__(self, q: str | None = None, skip: int = 0, limit: int = 100):
        self.q = q
        self.skip = skip
        self.limit = limit


@app.get("/items/")
async def read_items(commons: Annotated[Any, Depends(CommonQueryParams)]):
    response = {}
    if commons.q:
        response.update({"q": commons.q})
    items = fake_items_db[commons.skip : commons.skip + commons.limit]
    response.update({"items": items})
    return response
```

但是声明类型是被鼓励的，因为那样你的编辑器就会知道将传递什么作为参数 `commons` ，然后它可以帮助你完成代码，类型检查，等等：



### 快捷方式

但是你可以看到，我们在这里有一些代码重复了，编写了`CommonQueryParams`两次：

```python:
commons: Annotated[CommonQueryParams, Depends(CommonQueryParams)]
```

**FastAPI** 为这些情况提供了一个快捷方式，在这些情况下，依赖项 *明确地* 是一个类，**FastAPI** 将 "调用" 它来创建类本身的一个实例。

对于这些特定的情况，你可以按如下操作：

```python
commons: Annotated[CommonQueryParams, Depends()]
```

 你声明依赖项作为参数的类型，并使用 `Depends()` 作为该函数的参数的 "默认" 值(在 `=` 之后)，而在 `Depends()` 中没有任何参数，而不是在 `Depends(CommonQueryParams)` 中*再次*编写完整的类。

同样的例子看起来像这样：你声明依赖项作为参数的类型，并使用 `Depends()` 作为该函数的参数的 "默认" 值(在 `=` 之后)，而在 `Depends()` 中没有任何参数，而不是在 `Depends(CommonQueryParams)` 中*再次*编写完整的类。

同样的例子看起来像这样：

```python
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]


class CommonQueryParams:
    def __init__(self, q: str | None = None, skip: int = 0, limit: int = 100):
        self.q = q
        self.skip = skip
        self.limit = limit


@app.get("/items/")
async def read_items(commons: Annotated[CommonQueryParams, Depends()]):
    response = {}
    if commons.q:
        response.update({"q": commons.q})
    items = fake_items_db[commons.skip : commons.skip + commons.limit]
    response.update({"items": items})
    return response
```

... **FastAPI** 会知道怎么处理。



----

-----

## 子依赖项

FastAPI 支持创建含**子依赖项**的依赖项。

并且，可以按需声明任意**深度**的子依赖项嵌套层级。

**FastAPI** 负责处理解析不同深度的子依赖项。



### 第一层依赖项 “dependable”

你可以创建一个第一层依赖项（“dependable”），如下：

```python
from typing import Annotated

from fastapi import Cookie, Depends, FastAPI

app = FastAPI()

  """"
def query_extractor(q: str | None = None):
    return q
  """"


def query_or_cookie_extractor(
    q: Annotated[str, Depends(query_extractor)],
    last_query: Annotated[str | None, Cookie()] = None,
):
    if not q:
        return last_query
    return q


@app.get("/items/")
async def read_query(
    query_or_default: Annotated[str, Depends(query_or_cookie_extractor)],
):
    return {"q_or_cookie": query_or_default}
```

这段代码声明了类型为 `str` 的可选查询参数 `q`，然后返回这个查询参数。

这个函数很简单（不过也没什么用），但却有助于让我们专注于了解子依赖项的工作方式。

### 第二层依赖项，“dependable”和“dependant”

接下来，创建另一个依赖项函数（一个“dependable”），并同时为它自身再声明一个依赖项（因此它同时也是一个“dependant”）：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/sub-dependencies/#__tabbed_3_1)

```python
from typing import Annotated

from fastapi import Cookie, Depends, FastAPI

app = FastAPI()


def query_extractor(q: str | None = None):
    return q


def query_or_cookie_extractor(
    q: Annotated[str, Depends(query_extractor)],          ########################### 第二层依赖项
    last_query: Annotated[str | None, Cookie()] = None,
):
    if not q:
        return last_query
    return q


@app.get("/items/")
async def read_query(
    query_or_default: Annotated[str, Depends(query_or_cookie_extractor)],
):
    return {"q_or_cookie": query_or_default}
```



这里重点说明一下声明的参数：

- 尽管该函数自身是依赖项（“dependable”），但还声明了另一个依赖项（它“依赖”于其他对象）

  - 该函数依赖 `query_extractor`, 并把 `query_extractor` 的返回值赋给参数 `q`

- 同时，该函数还声明了类型是`str`的可选 cookie（`last_query`）

  - 用户未提供查询参数 `q` 时，则使用上次使用后保存在 cookie 中的查询

### 使用依赖项

接下来，就可以使用依赖项：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/sub-dependencies/#__tabbed_5_1)

```python
from typing import Annotated

from fastapi import Cookie, Depends, FastAPI

app = FastAPI()


def query_extractor(q: str | None = None):
    return q


def query_or_cookie_extractor(
    q: Annotated[str, Depends(query_extractor)],
    last_query: Annotated[str | None, Cookie()] = None,
):
    if not q:
        return last_query
    return q


@app.get("/items/")
async def read_query(
  """
    query_or_default: Annotated[str, Depends(query_or_cookie_extractor)],
  """
):
    return {"q_or_cookie": query_or_default}
```



> ​                                                                                         `⚠️`
>
> 注意，这里在*路径操作函数*中只声明了一个依赖项，即 `query_or_cookie_extractor` 。
>
> 但 **FastAPI** 必须先处理 `query_extractor`，以便在调用 `query_or_cookie_extractor` 时使用 `query_extractor` 返回的结果。



```mermaid
graph TD
	A(query_extractor) --> B(query_or_Cookie_extractor)
	B --> C(/items/)
```

### 多次使用同一个依赖项

如果在同一个*路径操作* 多次声明了同一个依赖项，例如，多个依赖项共用一个子依赖项，**FastAPI** 在处理同一请求时，只调用一次该子依赖项。

FastAPI 不会为同一个请求多次调用同一个依赖项，而是把依赖项的返回值进行「缓存」，并把它传递给同一请求中所有需要使用该返回值的「依赖项」。

在高级使用场景中，如果不想使用「缓存」值，而是为需要在同一请求的每一步操作（多次）中都实际调用依赖项，可以把 `Depends` 的参数 `use_cache` 的值设置为 `False`:

```python
async def needy_dependency(fresh_value: Annotated[str, Depends(get_value, use_cache=False)]):
    return {"fresh_value": fresh_value}
```

### 小结

千万别被本章里这些花里胡哨的词藻吓倒了，其实**依赖注入**系统非常简单。

依赖注入无非是与*路径操作函数*一样的函数罢了。

但它依然非常强大，能够声明任意嵌套深度的「图」或树状的依赖结构。



> 🔥 提示
>
> 这些简单的例子现在看上去虽然没有什么实用价值，
>
> 但在**安全**一章中，您会了解到这些例子的用途，
>
> 以及这些例子所能节省的代码量。





------

----

## 路径操作装饰器依赖项

有时，我们并不需要在*路径操作函数*中使用依赖项的返回值。

或者说，有些依赖项不返回值。

但仍要执行或解析该依赖项。

对于这种情况，不必在声明*路径操作函数*的参数时使用 `Depends`，而是可以在*路径操作装饰器*中添加一个由 `dependencies` 组成的 `list`。



### 在*路径操作装饰器*中添加 `dependencies` 参数

*路径操作装饰器*支持可选参数 `dependencies`。

该参数的值是由 `Depends()` 组成的 `list`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-in-path-operation-decorators/#__tabbed_1_1)

```python
from typing import Annotated

from fastapi import Depends, FastAPI, Header, HTTPException

app = FastAPI()


async def verify_token(x_token: Annotated[str, Header()]):
    if x_token != "fake-super-secret-token":
        raise HTTPException(status_code=400, detail="X-Token header invalid")


async def verify_key(x_key: Annotated[str, Header()]):
    if x_key != "fake-super-secret-key":
        raise HTTPException(status_code=400, detail="X-Key header invalid")
    return x_key


@app.get("/items/", dependencies=[Depends(verify_token), Depends(verify_key)])
async def read_items():
    return [{"item": "Foo"}, {"item": "Bar"}]
```

路径操作装饰器依赖项的执行或解析方式和普通依赖项一样，但就算这些依赖项会返回值，它们的值也不会传递给*路径操作函数*。



### 依赖项错误和返回值

路径装饰器依赖项也可以使用普通的依赖项*函数*。

#### 依赖项的需求项

路径装饰器依赖项可以声明请求的需求项（比如响应头）或其他子依赖项：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-in-path-operation-decorators/#__tabbed_3_1)

```python
async def verify_token(x_token: Annotated[str, Header()]):



async def verify_key(x_key: Annotated[str, Header()]):

```

#### 触发异常

路径装饰器依赖项与正常的依赖项一样，可以 `raise` 异常：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-in-path-operation-decorators/#__tabbed_5_1)

```python
async def verify_token(x_token: Annotated[str, Header()]):
    if x_token != "fake-super-secret-token":
        raise HTTPException(status_code=400, detail="X-Token header invalid")



    if x_key != "fake-super-secret-key":
        raise HTTPException(status_code=400, detail="X-Key header invalid")
    return x_key
```



#### 返回值

无论路径装饰器依赖项是否返回值，路径操作都不会使用这些值。

因此，可以复用在其他位置使用过的、（能返回值的）普通依赖项，即使没有使用这个值，也会执行该依赖项：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-in-path-operation-decorators/#__tabbed_7_1)

```python
from typing import Annotated

from fastapi import Depends, FastAPI, Header, HTTPException

app = FastAPI()


async def verify_token(x_token: Annotated[str, Header()]):
    if x_token != "fake-super-secret-token":
        raise HTTPException(status_code=400, detail="X-Token header invalid")


async def verify_key(x_key: Annotated[str, Header()]):
    if x_key != "fake-super-secret-key":
        raise HTTPException(status_code=400, detail="X-Key header invalid")
    return x_key


@app.get("/items/", dependencies=[Depends(verify_token), Depends(verify_key)])
async def read_items():
    return [{"item": "Foo"}, {"item": "Bar"}]
```



### 为一组路径操作定义依赖项

稍后，[大型应用 - 多文件](https://fastapi.tiangolo.com/zh/tutorial/bigger-applications/)一章中会介绍如何使用多个文件创建大型应用程序，在这一章中，您将了解到如何为一组*路径操作*声明单个 `dependencies` 参数。

### 全局依赖项

接下来，我们将学习如何为 `FastAPI` 应用程序添加全局依赖项，创建应用于每个*路径操作*的依赖项。



-----

-----

## 全局依赖项

有时，我们要为整个应用添加依赖项。

通过与[将 `dependencies` 添加到*路径操作装饰器*](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-in-path-operation-decorators/) 类似的方式，可以把依赖项添加至整个 `FastAPI` 应用。

这样一来，就可以为所有*路径操作*应用该依赖项：

```python
from typing import Annotated

from fastapi import Depends, FastAPI, Header, HTTPException


async def verify_token(x_token: Annotated[str, Header()]):
    if x_token != "fake-super-secret-token":
        raise HTTPException(status_code=400, detail="X-Token header invalid")


async def verify_key(x_key: Annotated[str, Header()]):
    if x_key != "fake-super-secret-key":
        raise HTTPException(status_code=400, detail="X-Key header invalid")
    return x_key

			"""
app = FastAPI(dependencies=[Depends(verify_token), Depends(verify_key)])
			"""



@app.get("/items/")
async def read_items():
    return [{"item": "Portal Gun"}, {"item": "Plumbus"}]


@app.get("/users/")
async def read_users():
    return [{"username": "Rick"}, {"username": "Morty"}]
```



[将 `dependencies` 添加到*路径操作装饰器*](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-in-path-operation-decorators/) 一章的思路均适用于全局依赖项， 在本例中，这些依赖项可以用于应用中的所有*路径操作*。

### 为一组路径操作定义依赖项

稍后，[大型应用 - 多文件](https://fastapi.tiangolo.com/zh/tutorial/bigger-applications/)一章中会介绍如何使用多个文件创建大型应用程序，在这一章中，你将了解到如何为一组*路径操作*声明单个 `dependencies` 参数。





---

-----

## 使用yield的依赖项

FastAPI 支持那些在完成后执行一些额外步骤的依赖项。

为此，使用 `yield` 而不是 `return`，并把这些额外步骤（代码）写在后面。

> 🔥 Tips
>
> 确保在每个依赖里只使用一次 `yield`。
>
> 任何可以与以下装饰器一起使用的函数：
>
> - [`@contextlib.contextmanager`](https://docs.python.org/3/library/contextlib.html#contextlib.contextmanager) 或
> - [`@contextlib.asynccontextmanager`](https://docs.python.org/3/library/contextlib.html#contextlib.asynccontextmanager)
>
> 都可以作为 **FastAPI** 的依赖项。
>
> 实际上，FastAPI 在内部就是用的这两个装饰器。

### 使用 `yield` 的数据库依赖项

例如，你可以用这种方式创建一个数据库会话，并在完成后将其关闭。

在创建响应之前，只会执行 `yield` 语句及其之前的代码：

```python
async def get_db():
    db = DBSession()
    try:
        yield db
    finally:
        db.close()
```

`yield` 产生的值会注入到 *路径操作* 和其他依赖项中：

```python
        yield db
```

`yield` 语句后面的代码会在响应之后执行：

```python
    finally:
        db.close()
```

### 同时使用 `yield` 和 `tr#y` 的依赖项

如果你在带有 `yield` 的依赖中使用了 `try` 代码块，那么当使用该依赖时抛出的任何异常你都会收到。

例如，如果在中间的某处代码中（在另一个依赖或在某个 *路径操作* 中）发生了数据库事务“回滚”或产生了其他异常，你会在你的依赖中收到这个异常。

因此，你可以在该依赖中用 `except SomeException` 来捕获这个特定异常。

同样地，你可以使用 `finally` 来确保退出步骤一定会被执行，无论是否发生异常。

```python
    try:
        yield db
    finally:
        db.close()
```

### 使用 `yield` 的子依赖项

你可以声明任意大小和形状的子依赖及其“树”，其中任意一个或全部都可以使用 `yield`。

**FastAPI** 会确保每个带有 `yield` 的依赖中的“退出代码”按正确的顺序运行。

例如，`dependency_c` 可以依赖 `dependency_b`，而 `dependency_b` 则依赖 `dependency_a`：

```python
from typing import Annotated

from fastapi import Depends


async def dependency_a():
    dep_a = generate_dep_a()
    try:
        yield dep_a
    finally:
        dep_a.close()


async def dependency_b(dep_a: Annotated[DepA, Depends(dependency_a)]):
    dep_b = generate_dep_b()
    try:
        yield dep_b
    finally:
        dep_b.close(dep_a)


async def dependency_c(dep_b: Annotated[DepB, Depends(dependency_b)]):
    dep_c = generate_dep_c()
    try:
        yield dep_c
    finally:
        dep_c.close(dep_b)
```

并且它们都可以使用 `yield`。

在这种情况下，`dependency_c` 在执行其退出代码时需要 `dependency_b`（此处命名为 `dep_b`）的值仍然可用。

而 `dependency_b` 又需要 `dependency_a`（此处命名为 `dep_a`）的值在其退出代码中可用。

同样地，你可以将一些依赖用 `yield`，另一些用 `return`，并让其中一些依赖依赖于另一些。

你也可以有一个依赖需要多个带有 `yield` 的依赖，等等。

你可以拥有任何你想要的依赖组合。

**FastAPI** 将确保一切都按正确的顺序运行。

> 🖊️ 技术细节
>
> 这要归功于 Python 的[上下文管理器](https://docs.python.org/3/library/contextlib.html)。
>
> **FastAPI** 在内部使用它们来实现这一点。

### 同时使用 `yield` 和 `HTTPException` 的依赖项

你已经看到可以在带有 `yield` 的依赖中使用 `try` 块尝试执行一些代码，然后在 `finally` 之后运行一些退出代码。

你也可以使用 `except` 来捕获引发的异常并对其进行处理。

例如，你可以抛出一个不同的异常，如 `HTTPException`。

```python
from typing import Annotated

from fastapi import Depends, FastAPI, HTTPException

app = FastAPI()


data = {
    "plumbus": {"description": "Freshly pickled plumbus", "owner": "Morty"},
    "portal-gun": {"description": "Gun to create portals", "owner": "Rick"},
}


class OwnerError(Exception):
    pass


def get_username():
    try:
        yield "Rick"
    except OwnerError as e:
        raise HTTPException(status_code=400, detail=f"Owner error: {e}")


@app.get("/items/{item_id}")
def get_item(item_id: str, username: Annotated[str, Depends(get_username)]):
    if item_id not in data:
        raise HTTPException(status_code=404, detail="Item not found")
    item = data[item_id]
    if item["owner"] != username:
        raise OwnerError(username)
    return item
```

如果你想捕获异常并基于它创建一个自定义响应，请创建一个[自定义异常处理器](https://fastapi.tiangolo.com/zh/tutorial/handling-errors/#install-custom-exception-handlers)

### 同时使用 `yield` 和 `except` 的依赖项

如果你在带有 `yield` 的依赖中使用 `except` 捕获了一个异常，并且你没有再次抛出它（或抛出一个新异常），FastAPI 将无法察觉发生过异常，就像普通的 Python 代码那样：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-with-yield/#__tabbed_11_1)

```python
from typing import Annotated

from fastapi import Depends, FastAPI, HTTPException

app = FastAPI()


class InternalError(Exception):
    pass


def get_username():
    try:
        yield "Rick"
    except InternalError:
        print("Oops, we didn't raise again, Britney 😱")


@app.get("/items/{item_id}")
def get_item(item_id: str, username: Annotated[str, Depends(get_username)]):
    if item_id == "portal-gun":
        raise InternalError(
            f"The portal gun is too dangerous to be owned by {username}"
        )
    if item_id != "plumbus":
        raise HTTPException(
            status_code=404, detail="Item not found, there's only a plumbus here"
        )
    return item_id
```

在这种情况下，客户端会像预期那样看到一个 *HTTP 500 Internal Server Error* 响应，因为我们没有抛出 `HTTPException` 或类似异常，但服务器将**没有任何日志**或其他关于错误是什么的提示。😱

#### 在带有 `yield` 和 `except` 的依赖中务必 `raise`

如果你在带有 `yield` 的依赖中捕获到了一个异常，除非你抛出另一个 `HTTPException` 或类似异常，**否则你应该重新抛出原始异常**。

你可以使用 `raise` 重新抛出同一个异常：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-with-yield/#__tabbed_13_1)

```python
from typing import Annotated

from fastapi import Depends, FastAPI, HTTPException

app = FastAPI()


class InternalError(Exception):
    pass


def get_username():
    try:
        yield "Rick"
    except InternalError:
        print("We don't swallow the internal error here, we raise again 😎")
        raise


@app.get("/items/{item_id}")
def get_item(item_id: str, username: Annotated[str, Depends(get_username)]):
    if item_id == "portal-gun":
        raise InternalError(
            f"The portal gun is too dangerous to be owned by {username}"
        )
    if item_id != "plumbus":
        raise HTTPException(
            status_code=404, detail="Item not found, there's only a plumbus here"
        )
    return item_id
```

现在客户端仍会得到同样的 *HTTP 500 Internal Server Error* 响应，但服务器日志中会有我们自定义的 `InternalError`。😎

### 使用 `yield` 的依赖项的执行

执行顺序大致如下图所示。时间轴从上到下，每一列都代表交互或执行代码的一部分。

![截屏2026-06-14 23.32.05](images/截屏2026-06-14 23.32.05.png)





> 🔥 提示
>
> 如果你在 *路径操作函数* 的代码中引发任何异常，它都会被传递给带有 `yield` 的依赖项，包括 `HTTPException`。在大多数情况下，你会希望在带有 `yield` 的依赖中重新抛出相同的异常或一个新的异常，以确保它被正确处理。

### 提前退出与 `scope`

通常，带有 `yield` 的依赖的退出代码会在响应发送给客户端**之后**执行。

但如果你知道在从 *路径操作函数* 返回之后不再需要使用该依赖，你可以使用 `Depends(scope="function")` 告诉 FastAPI：应当在 *路径操作函数* 返回后、但在**响应发送之前**关闭该依赖。

```python
from typing import Annotated

from fastapi import Depends, FastAPI

app = FastAPI()


def get_username():
    try:
        yield "Rick"
    finally:
        print("Cleanup up before response is sent")


@app.get("/users/me")
def get_user_me(username: Annotated[str, Depends(get_username, scope="function")]):
    return username
```

 `Depends()` 接收一个 `scope` 参数，可为：

- `"function"`：在处理请求的 *路径操作函数* 之前启动依赖，在 *路径操作函数* 结束后结束依赖，但在响应发送给客户端**之前**。因此，依赖函数将围绕这个*路径操作函数*执行。
- `"request"`：在处理请求的 *路径操作函数* 之前启动依赖（与使用 `"function"` 时类似），但在响应发送给客户端**之后**结束。因此，依赖函数将围绕这个**请求**与响应周期执行。

如果未指定且依赖包含 `yield`，则默认 `scope` 为 `"request"`。

#### 子依赖的 `scope`

当你声明一个 `scope="request"`（默认）的依赖时，任何子依赖也需要有 `"request"` 的 `scope`。

但一个 `scope` 为 `"function"` 的依赖可以有 `scope` 为 `"function"` 和 `"request"` 的子依赖。

这是因为任何依赖都需要能够在子依赖之前运行其退出代码，因为它的退出代码中可能还需要使用这些子依赖。

![截屏2026-06-14 23.51.45](images/截屏2026-06-14 23.51.45.png)

----

### 包含 `yield`、`HTTPException`、`except` 和后台任务的依赖项

带有 `yield` 的依赖项随着时间演进以涵盖不同的用例并修复了一些问题。

如果你想了解在不同 FastAPI 版本中发生了哪些变化，可以在进阶指南中阅读更多：[高级依赖项 —— 包含 `yield`、`HTTPException`、`except` 和后台任务的依赖项](https://fastapi.tiangolo.com/zh/advanced/advanced-dependencies/#dependencies-with-yield-httpexception-except-and-background-tasks)。



### 上下文管理器

#### 什么是“上下文管理器”

“上下文管理器”是你可以在 `with` 语句中使用的任意 Python 对象。

例如，[你可以用 `with` 来读取文件](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files)：

```python
with open("./somefile.txt") as f:
    contents = f.read()
    print(contents)
```

在底层，`open("./somefile.txt")` 会创建一个“上下文管理器”对象。

当 `with` 代码块结束时，它会确保文件被关闭，即使期间发生了异常。

当你用 `yield` 创建一个依赖时，**FastAPI** 会在内部为它创建一个上下文管理器，并与其他相关工具结合使用。

#### 在带有 `yield` 的依赖中使用上下文管理器

在 Python 中，你可以通过[创建一个带有 `__enter__()` 和 `__exit__()` 方法的类](https://docs.python.org/3/reference/datamodel.html#context-managers)来创建上下文管理器。

你也可以在 **FastAPI** 的带有 `yield` 的依赖中，使用依赖函数内部的 `with` 或 `async with` 语句来使用它们：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/dependencies/dependencies-with-yield/#__tabbed_17_1)

```python
class MySuperContextManager:
    def __init__(self):
        self.db = DBSession()

    def __enter__(self):
        return self.db

    def __exit__(self, exc_type, exc_value, traceback):
        self.db.close()


async def get_db():
    with MySuperContextManager() as db:
        yield db
```





------

------

## 安全性

有许多方法可以处理安全性、身份认证和授权等问题。

而且这通常是一个复杂而「困难」的话题。

在许多框架和系统中，仅处理安全性和身份认证就会花费大量的精力和代码（在许多情况下，可能占编写的所有代码的 50％ 或更多）。

**FastAPI** 提供了多种工具，可帮助你以标准的方式轻松、快速地处理**安全性**，而无需研究和学习所有的安全规范。

但首先，让我们来看一些小的概念。



### OAuth2

OAuth2是一个规范，它定义了几种处理身份认证和授权的方法。

它是一个相当广泛的规范，涵盖了一些复杂的使用场景。

它包括了使用「第三方」进行身份认证的方法。

这就是所有带有「使用 Facebook，Google，X (Twitter)，GitHub 登录」的系统背后所使用的机制。



### OAuth 1

有一个 OAuth 1，它与 OAuth2 完全不同，并且更为复杂，因为它直接包含了有关如何加密通信的规范。

如今它已经不是很流行，没有被广泛使用了。

OAuth2 没有指定如何加密通信，它期望你为应用程序使用 HTTPS 进行通信。



### OpenID Connect

OpenID Connect 是另一个基于 **OAuth2** 的规范。

它只是扩展了 OAuth2，并明确了一些在 OAuth2 中相对模糊的内容，以尝试使其更具互操作性。

例如，Google 登录使用 OpenID Connect（底层使用OAuth2）。

但是 Facebook 登录不支持 OpenID Connect。它具有自己的 OAuth2 风格。

#### OpenID（非「OpenID Connect」）penid-connect)

还有一个「OpenID」规范。它试图解决与 **OpenID Connect** 相同的问题，但它不是基于 OAuth2。

因此，它是一个完整的附加系统。

如今它已经不是很流行，没有被广泛使用了。

### OpenAPI

OpenAPI（以前称为 Swagger）是用于构建 API 的开放规范（现已成为 Linux Foundation 的一部分）。

**FastAPI** 基于 **OpenAPI**。

这就是使多个自动交互式文档界面，代码生成等成为可能的原因。

OpenAPI 有一种定义多个安全「方案」的方法。

通过使用它们，你可以利用所有这些基于标准的工具，包括这些交互式文档系统。

OpenAPI 定义了以下安全方案：

- `apiKey`: 一个特定于应用程序的密钥，可以来自：

  - 查询参数。
  - 请求头。
  - cookie。

---

- `http`：标准的 HTTP 身份认证系统，包括：
    - `bearer`: 一个值为 `Bearer` 加令牌字符串的 `Authorization` 请求头。这是从 OAuth2 继承的。
    - HTTP Basic 认证方式。
    - HTTP Digest，等等。

---
- oauth2：所有的 OAuth2 处理安全性的方式（称为「流程」）。 *以下几种流程适合构建 OAuth 2.0 身份认证的提供者（例如 Google，Facebook，X (Twitter)，GitHub 等）： * implicit * clientCredentials * authorizationCode
	- 但是有一个特定的「流程」可以完美地用于直接在同一应用程序中处理身份认证：
		- `password`：接下来的几章将介绍它的示例。
- openIdConnect：提供了一种定义如何自动发现 OAuth2 身份认证数据的方法。
	- 此自动发现机制是 OpenID Connect 规范中定义的内容。



> 🔥 提示
>
> 集成其他身份认证/授权提供者（例如Google，Facebook，X (Twitter)，GitHub等）也是可能的，而且较为容易。
>
> 最复杂的问题是创建一个像这样的身份认证/授权提供程序，但是 **FastAPI** 为你提供了轻松完成任务的工具，同时为你解决了重活。

### **FastAPI** 实用工具

FastAPI 在 `fastapi.security` 模块中为每个安全方案提供了几种工具，这些工具简化了这些安全机制的使用方法。

在接下来的章节中，你将看到如何使用 **FastAPI** 所提供的这些工具为你的 API 增加安全性。

而且你还将看到它如何自动地被集成到交互式文档系统中。









----

---

## 安全 - 第一步

假设你的**后端** API 位于某个域名下。

而**前端**在另一个域名，或同一域名的不同路径（或在移动应用中）。

你希望前端能通过**username** 和 **password** 与后端进行身份验证。

我们可以用 **OAuth2** 在 **FastAPI** 中实现它。

但为了节省你的时间，不必为获取少量信息而通读冗长的规范。

我们直接使用 **FastAPI** 提供的安全工具。



### 效果预览

先直接运行代码看看效果，之后再回过头理解其背后的原理。

### 创建 `main.py`

把下面的示例代码复制到 `main.py`：

[Python 3.10+](https://fastapi.tiangolo.com/zh/tutorial/security/first-steps/#__tabbed_1_1)

```python 
from typing import Annotated

from fastapi import Depends, FastAPI
from fastapi.security import OAuth2PasswordBearer

app = FastAPI()

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")


@app.get("/items/")
async def read_items(token: Annotated[str, Depends(oauth2_scheme)]):
    return {"token": token}
```



### 查看

打开交互式文档：<http://127.0.0.1:8000/docs>。

你会看到类似这样的界面：

![截屏2026-06-15 12.04.38](images/截屏2026-06-15 12.04.38.png)

页面右上角已经有一个崭新的“Authorize”按钮。

你的*路径操作*右上角还有一个可点击的小锁图标。

点击它，会弹出一个授权表单，可输入 `username` 和 `password`（以及其它可选字段）：

![截屏2026-06-15 12.08.40](images/截屏2026-06-15 12.08.40.png)



> ✏️ 注意
>
> 目前无论在表单中输入什么都不会生效，我们稍后就会实现它。



这当然不是面向最终用户的前端，但它是一个很棒的自动化工具，可交互式地为整个 API 提供文档。

前端团队（也可能就是你自己）可以使用它。

第三方应用和系统也可以使用它。

你也可以用它来调试、检查和测试同一个应用。



### `password` 流

现在回过头来理解这些内容。

`password` “流”（flow）是 OAuth2 定义的处理安全与身份验证的一种方式。

OAuth2 的设计目标是让后端或 API 与负责用户认证的服务器解耦。

但在这个例子中，**FastAPI** 应用同时处理 API 和认证。

从这个简化的角度来看看流程：

- 用户在前端输入 `username` 和 `password`，然后按下 `Enter`。
- 前端（运行在用户浏览器中）把 `username` 和 `password` 发送到我们 API 中的特定 URL（使用 `tokenUrl="token"` 声明）。
- API 校验 `username` 和 `password`，并返回一个“令牌”（这些我们尚未实现）。
  - “令牌”只是一个字符串，包含一些内容，之后可用来验证该用户。
  - 通常，令牌会在一段时间后过期。
    - 因此，用户过一段时间需要重新登录。
    - 如果令牌被窃取，风险也更小。它不像一把永久有效的钥匙（在大多数情况下）。

- 前端会把令牌临时存储在某处。
- 用户在前端中点击跳转到前端应用的其他部分。
- 前端需要从 API 获取更多数据。
  - 但该端点需要身份验证。
  - 因此，为了与我们的 API 进行身份验证，它会发送一个 `Authorization` 请求头，值为 `Bearer` 加上令牌。
  - 如果令牌内容是 `foobar`，`Authorization` 请求头的内容就是：`Bearer foobar`。



---

### **FastAPI** 的 `OAuth2PasswordBearer`

**FastAPI** 在不同抽象层级提供了多种安全工具。

本示例将使用 **OAuth2** 的 **Password** 流程并配合 **Bearer** 令牌，通过 `OAuth2PasswordBearer` 类来实现。



> ❕information
>
> “Bearer” 令牌并非唯一选项。
>
> 但它非常适合我们的用例。
>
> 对于大多数用例，它也可能是最佳选择，除非你是 OAuth2 专家，并明确知道为何其他方案更适合你的需求。
>
> 在那种情况下，**FastAPI** 同样提供了相应的构建工具。

创建 `OAuth2PasswordBearer` 类实例时，需要传入 `tokenUrl` 参数。该参数包含客户端（运行在用户浏览器中的前端）用来发送 `username` 和 `password` 以获取令牌的 URL。

```python
from typing import Annotated

from fastapi import Depends, FastAPI
from fastapi.security import OAuth2PasswordBearer

app = FastAPI()

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")


@app.get("/items/")
async def read_items(token: Annotated[str, Depends(oauth2_scheme)]):
    return {"token": token}
```



> 🔥 提示
>
> 这里的 `tokenUrl="token"` 指向的是尚未创建的相对 URL `token`，等价于 `./token`。
>
> 因为使用的是相对 URL，若你的 API 位于 `https://example.com/`，它将指向 `https://example.com/token`；若你的 API 位于 `https://example.com/api/v1/`，它将指向 `https://example.com/api/v1/token`。
>
> 使用相对 URL 很重要，这能确保你的应用在诸如[使用代理](https://fastapi.tiangolo.com/zh/advanced/behind-a-proxy/)等高级用例中依然正常工作。



这个参数不会创建该端点/*路径操作*，而是声明客户端应使用 `/token` 这个 URL 来获取令牌。这些信息会用于 OpenAPI，进而用于交互式 API 文档系统。

我们很快也会创建对应的实际路径操作。

#### 使用

现在你可以通过 `Depends` 将 `oauth2_scheme` 作为依赖传入。

```python
from typing import Annotated

from fastapi import Depends, FastAPI
from fastapi.security import OAuth2PasswordBearer

app = FastAPI()

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")


@app.get("/items/")
async def read_items(token: Annotated[str, Depends(oauth2_scheme)]):
    return {"token": token}
```



该依赖会提供一个 `str`，赋值给*路径操作函数*的参数 `token`。

**FastAPI** 会据此在 OpenAPI 架构（以及自动生成的 API 文档）中定义一个“安全方案”。



### 它做了什么

它会在请求中查找 `Authorization` 请求头，检查其值是否为 `Bearer` 加上一些令牌，并将该令牌作为 `str` 返回。

如果没有 `Authorization` 请求头，或者其值不包含 `Bearer` 令牌，它会直接返回 401 状态码错误（`UNAUTHORIZED`）。

你甚至无需检查令牌是否存在即可返回错误；只要你的函数被执行，就可以确定会拿到一个 `str` 类型的令牌。

你已经可以在交互式文档中试试了：

![截屏2026-06-15 12.33.12](images/截屏2026-06-15 12.33.12.png)

我们还没有验证令牌是否有效，但这已经是一个良好的开端。



### 小结

只需增加三四行代码，你就已经拥有了一种初步的安全机制。





----

---

## 获取当前用户

上一章中，（基于依赖注入系统的）安全系统向*路径操作函数*传递了 `str` 类型的 `token`：

```python
from typing import Annotated

from fastapi import Depends, FastAPI
from fastapi.security import OAuth2PasswordBearer

app = FastAPI()

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")


@app.get("/items/")
async def read_items(token: Annotated[str, Depends(oauth2_scheme)]):
    return {"token": token}
```

但这并不实用。

接下来，我们学习如何返回当前用户。



### 创建用户模型

首先，创建 Pydantic 用户模型。

与使用 Pydantic 声明请求体相同，并且可在任何位置使用：

```python
from typing import Annotated

from fastapi import Depends, FastAPI
from fastapi.security import OAuth2PasswordBearer
from pydantic import BaseModel

app = FastAPI()

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")


class User(BaseModel):
    username: str
    email: str | None = None
    full_name: str | None = None
    disabled: bool | None = None


def fake_decode_token(token):
    return User(
        username=token + "fakedecoded", email="john@example.com", full_name="John Doe"
    )


async def get_current_user(token: Annotated[str, Depends(oauth2_scheme)]):
    user = fake_decode_token(token)
    return user


@app.get("/users/me")
async def read_users_me(current_user: Annotated[User, Depends(get_current_user)]):
    return current_user
```



### 创建 `get_current_user` 依赖

创建 `get_current_user` 依赖项。

还记得依赖项支持子依赖项吗？

`get_current_user` 使用 `oauth2_scheme` 作为依赖项。

与之前直接在路径操作中的做法相同，新的 `get_current_user` 依赖项从子依赖项 `oauth2_scheme` 中接收 `str` 类型的 `token`：

```python
async def get_current_user(token: Annotated[str, Depends(oauth2_scheme)]):
```



### 获取用户

`get_current_user` 使用创建的（伪）工具函数，该函数接收 `str` 类型的令牌，并返回 Pydantic 的 `User` 模型：

```python
def fake_decode_token(token):
    return User(
        username=token + "fakedecoded", email="john@example.com", full_name="John Doe"
    )


async def get_current_user(token: Annotated[str, Depends(oauth2_scheme)]):
    user = fake_decode_token(token)
    return user
```





### 注入当前用户

在*路径操作* 的 `Depends` 中使用 `get_current_user`：

```python
async def read_users_me(current_user: Annotated[User, Depends(get_current_user)]):
    return current_user
```



注意，此处把 `current_user` 的类型声明为 Pydantic 的 `User` 模型。

这有助于在函数内部使用代码补全和类型检查。



> 🔥 提示
>
> 还记得请求体也是使用 Pydantic 模型声明的吧。
>
> 放心，因为使用了 `Depends`，**FastAPI** 不会搞混。
>
> 依赖系统的这种设计方式可以支持不同的依赖项返回同一个 `User` 模型。
>
> 而不是局限于只能有一个返回该类型数据的依赖项。



### 其它模型

接下来，直接在*路径操作函数*中获取当前用户，并用 `Depends` 在**依赖注入**系统中处理安全机制。

开发者可以使用任何模型或数据满足安全需求（本例中是 Pydantic 的 `User` 模型）。

而且，不局限于只能使用特定的数据模型、类或类型。

不想在模型中使用 `username`，而是使用 `id` 和 `email`？当然可以。这些工具也支持。

只想使用字符串？或字典？甚至是数据库类模型的实例？工作方式都一样。

实际上，就算登录应用的不是用户，而是只拥有访问令牌的机器人、程序或其它系统？工作方式也一样。

尽管使用应用所需的任何模型、类、数据库。**FastAPI** 通过依赖注入系统都能帮您搞定。





### 代码大小

这个示例看起来有些冗长。毕竟这个文件同时包含了安全、数据模型的工具函数，以及路径操作等代码。

但，关键是：

**安全和依赖注入的代码只需要写一次。**

就算写得再复杂，也只是在一个位置写一次就够了。所以，要多复杂就可以写多复杂。

但是，就算有数千个端点（*路径操作*），它们都可以使用同一个安全系统。

而且，所有端点（或它们的任何部件）都可以利用这些依赖项或任何其它依赖项。

所有*路径操作*只需 3 行代码就可以了：

```python
@app.get("/users/me")
async def read_users_me(current_user: Annotated[User, Depends(get_current_user)]):
    return current_user
```



### 小结

现在，我们可以直接在*路径操作函数*中获取当前用户。

至此，安全的内容已经讲了一半。

只要再为用户或客户端的*路径操作*添加真正发送 `username` 和 `password` 的功能就可以了。



------

---

## OAuth2 实现简单的 Password 和 Bearer 验证

### 获取 `username` 和 `password`

首先，使用 **FastAPI** 安全工具获取 `username` 和 `password`。

OAuth2 规范要求使用“密码流”时，客户端或用户必须以表单数据形式发送 `username` 和 `password` 字段。

并且，这两个字段必须命名为 `username` 和 `password`，不能使用 `user-name` 或 `email` 等其它名称。

不过也不用担心，前端仍可以显示终端用户所需的名称。

数据库模型也可以使用所需的名称。

但对于登录*路径操作*，则要使用兼容规范的 `username` 和 `password`，（例如，实现与 API 文档集成）。

该规范要求必须以表单数据形式发送 `username` 和 `password`，因此，不能使用 JSON 对象。



#### `scope`

OAuth2 还支持客户端发送**scope**表单字段。

虽然表单字段的名称是 `scope`（单数），但实际上，它是以空格分隔的，由多个**scope**组成的长字符串。

**作用域**只是不带空格的字符串。

常用于声明指定安全权限，例如：

- 常见用例为，`users:read` 或 `users:write`
- 脸书和 Instagram 使用 `instagram_basic`
- 谷歌使用 `https://www.googleapis.com/auth/drive`



> ✏️ 信息
>
> OAuth2 中，**作用域**只是声明指定权限的字符串。
>
> 是否使用冒号 `:` 等符号，或是不是 URL 并不重要。
>
> 这些细节只是特定的实现方式。
>
> 对 OAuth2 来说，都只是字符串而已。



### 获取 `username` 和 `password` 的代码

接下来，使用 **FastAPI** 工具获取用户名与密码。

#### `OAuth2PasswordRequestForm`

首先，导入 `OAuth2PasswordRequestForm`，然后，在 `/token` *路径操作* 中，用 `Depends` 把该类作为依赖项。

```python
from typing import Annotated

from fastapi import Depends, FastAPI, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from pydantic import BaseModel

fake_users_db = {
    "johndoe": {
        "username": "johndoe",
        "full_name": "John Doe",
        "email": "johndoe@example.com",
        "hashed_password": "fakehashedsecret",
        "disabled": False,
    },
    "alice": {
        "username": "alice",
        "full_name": "Alice Wonderson",
        "email": "alice@example.com",
        "hashed_password": "fakehashedsecret2",
        "disabled": True,
    },
}

app = FastAPI()


def fake_hash_password(password: str):
    return "fakehashed" + password


oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")


class User(BaseModel):
    username: str
    email: str | None = None
    full_name: str | None = None
    disabled: bool | None = None


class UserInDB(User):
    hashed_password: str


def get_user(db, username: str):
    if username in db:
        user_dict = db[username]
        return UserInDB(**user_dict)


def fake_decode_token(token):
    # This doesn't provide any security at all
    # Check the next version
    user = get_user(fake_users_db, token)
    return user


async def get_current_user(token: Annotated[str, Depends(oauth2_scheme)]):
    user = fake_decode_token(token)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Not authenticated",
            headers={"WWW-Authenticate": "Bearer"},
        )
    return user


async def get_current_active_user(
    current_user: Annotated[User, Depends(get_current_user)],
):
    if current_user.disabled:
        raise HTTPException(status_code=400, detail="Inactive user")
    return current_user


@app.post("/token")
async def login(form_data: Annotated[OAuth2PasswordRequestForm, Depends()]):
    user_dict = fake_users_db.get(form_data.username)
    if not user_dict:
        raise HTTPException(status_code=400, detail="Incorrect username or password")
    user = UserInDB(**user_dict)
    hashed_password = fake_hash_password(form_data.password)
    if not hashed_password == user.hashed_password:
        raise HTTPException(status_code=400, detail="Incorrect username or password")

    return {"access_token": user.username, "token_type": "bearer"}


@app.get("/users/me")
async def read_users_me(
    current_user: Annotated[User, Depends(get_current_active_user)],
):
    return current_user
```

`OAuth2PasswordRequestForm` 是用以下几项内容声明表单请求体的类依赖项：

- `username`
- `password`
- 可选的 `scope` 字段，由多个空格分隔的字符串组成的长字符串
- 可选的 `grant_type`

- 可选的 `client_id`（本例未使用）
- 可选的 `client_secret`（本例未使用）

#### 使用表单数据

现在，即可使用表单字段 `username`，从（伪）数据库中获取用户数据。

如果不存在指定用户，则返回错误消息，提示**用户名或密码错误**。

本例使用 `HTTPException` 异常显示此错误

```python
    user_dict = fake_users_db.get(form_data.username)
    if not user_dict:
        raise HTTPException(status_code=400, detail="Incorrect username or password")
```

#### 校验密码

至此，我们已经从数据库中获取了用户数据，但尚未校验密码。

接下来，首先将数据放入 Pydantic 的 `UserInDB` 模型。

注意：永远不要保存明文密码，本例暂时先使用（伪）哈希密码系统。

如果密码不匹配，则返回与上面相同的错误。

##### 密码哈希

**哈希**是指，将指定内容（本例中为密码）转换为形似乱码的字节序列（其实就是字符串）。

每次传入完全相同的内容（比如，完全相同的密码）时，得到的都是完全相同的乱码。

但这个乱码无法转换回传入的密码。

###### 为什么使用密码哈希

原因很简单，假如数据库被盗，窃贼无法获取用户的明文密码，得到的只是哈希值。

这样一来，窃贼就无法在其它应用中使用窃取的密码，要知道，很多用户在所有系统中都使用相同的密码，风险超大。

```python
 user = UserInDB(**user_dict)
    hashed_password = fake_hash_password(form_data.password)
    if not hashed_password == user.hashed_password:
        raise HTTPException(status_code=400, detail="Incorrect username or password")
```

##### 关于 `**user_dict`

`UserInDB(**user_dict)` 是指：

*直接把 user_dict 的键与值当作关键字参数传递，等效于：*

```python
UserInDB(
    username = user_dict["username"],
    email = user_dict["email"],
    full_name = user_dict["full_name"],
    disabled = user_dict["disabled"],
    hashed_password = user_dict["hashed_password"],
)
```

### 返回 Token

`token` 端点的响应必须是 JSON 对象。

响应返回的内容应该包含 `token_type`。本例中用的是**Bearer**Token，因此， Token 类型应为**bearer**。

返回内容还应包含 `access_token` 字段，它是包含权限 Token 的字符串。

本例只是简单的演示，返回的 Token 就是 `username`，但这种方式极不安全。

```python
 return {"access_token": user.username, "token_type": "bearer"}
```



> 🔥 提示
>
> 按规范的要求，应像本示例一样，返回带有 `access_token` 和 `token_type` 的 JSON 对象。
>
> 这是开发者必须在代码中自行完成的工作，并且要确保使用这些 JSON 的键。
>
> 这几乎是唯一需要开发者牢记在心，并按规范要求正确执行的事。
>
> **FastAPI** 则负责处理其它的工作。



### 更新依赖项

接下来，更新依赖项。

使之仅在当前用户为激活状态时，才能获取 `current_user`。

为此，要再创建一个依赖项 `get_current_active_user`，此依赖项以 `get_current_user` 依赖项为基础。

如果用户不存在，或状态为未激活，这两个依赖项都会返回 HTTP 错误。

因此，在端点中，只有当用户存在、通过身份验证、且状态为激活时，才能获得该用户：

```python
from typing import Annotated

from fastapi import Depends, FastAPI, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from pydantic import BaseModel

fake_users_db = {
    "johndoe": {
        "username": "johndoe",
        "full_name": "John Doe",
        "email": "johndoe@example.com",
        "hashed_password": "fakehashedsecret",
        "disabled": False,
    },
    "alice": {
        "username": "alice",
        "full_name": "Alice Wonderson",
        "email": "alice@example.com",
        "hashed_password": "fakehashedsecret2",
        "disabled": True,
    },
}

app = FastAPI()


def fake_hash_password(password: str):
    return "fakehashed" + password


oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")


class User(BaseModel):
    username: str
    email: str | None = None
    full_name: str | None = None
    disabled: bool | None = None


class UserInDB(User):
    hashed_password: str


def get_user(db, username: str):
    if username in db:
        user_dict = db[username]
        return UserInDB(**user_dict)


def fake_decode_token(token):
    # This doesn't provide any security at all
    # Check the next version
    user = get_user(fake_users_db, token)
    return user


async def get_current_user(token: Annotated[str, Depends(oauth2_scheme)]):
    user = fake_decode_token(token)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Not authenticated",
            headers={"WWW-Authenticate": "Bearer"},
        )
    return user


async def get_current_active_user(
    current_user: Annotated[User, Depends(get_current_user)],
):
    if current_user.disabled:
        raise HTTPException(status_code=400, detail="Inactive user")
    return current_user


@app.post("/token")
async def login(form_data: Annotated[OAuth2PasswordRequestForm, Depends()]):
    user_dict = fake_users_db.get(form_data.username)
    if not user_dict:
        raise HTTPException(status_code=400, detail="Incorrect username or password")
    user = UserInDB(**user_dict)
    hashed_password = fake_hash_password(form_data.password)
    if not hashed_password == user.hashed_password:
        raise HTTPException(status_code=400, detail="Incorrect username or password")

    return {"access_token": user.username, "token_type": "bearer"}


@app.get("/users/me")
async def read_users_me(
    current_user: Annotated[User, Depends(get_current_active_user)],
):
    return current_user
```

### 实际效果

打开交互式文档：<http://127.0.0.1:8000/docs>。

### 身份验证[¶](https://fastapi.tiangolo.com/zh/tutorial/security/simple-oauth2/#authenticate)

点击“Authorize”按钮。

使用以下凭证：

用户名：`johndoe`

密码：`secret`

![截屏2026-06-15 17.01.57](images/截屏2026-06-15 17.01.57.png)

通过身份验证后，显示下图所示的内容：![截屏2026-06-15 17.02.50](images/截屏2026-06-15 17.02.50.png)

#### 获取当前用户数据

使用 `/users/me` 路径的 `GET` 操作。

可以提取如下当前用户数据：

```json
{
  "username": "johndoe",
  "email": "johndoe@example.com",
  "full_name": "John Doe",
  "disabled": false,
  "hashed_password": "fakehashedsecret"
}
```

![截屏2026-06-15 17.05.55](images/截屏2026-06-15 17.05.55.png)

点击小锁图标，注销后，再执行同样的操作，则会得到 HTTP 401 错误：

![截屏2026-06-15 17.06.14](images/截屏2026-06-15 17.06.14.png)

#### 未激活用户

测试未激活用户，输入以下信息，进行身份验证：

用户名：`alice`

密码：`secret2`

然后，执行 `/users/me` 路径的 `GET` 操作。

显示下列**未激活用户**错误信息：

```python
{
  "detail": "Inactive user"
}
```



### 小结

使用本章的工具实现基于 `username` 和 `password` 的完整 API 安全系统。

这些工具让安全系统兼容任何数据库、用户及数据模型。

唯一欠缺的是，它仍然不是真的**安全**。

下一章你将看到如何使用安全的密码哈希库和 JWT 令牌。





---

----

## 使用密码（及哈希）的 OAuth2，基于 JWT 的 Bearer 令牌

现在我们已经有了完整的安全流程，接下来用 JWT 令牌和安全的密码哈希，让应用真正安全起来。

这些代码可以直接用于你的应用，你可以把密码哈希保存到数据库中，等等。

我们将从上一章结束的地方继续，逐步完善。

### 关于 JWT

JWT 意为 “JSON Web Tokens”。

它是一种标准，把一个 JSON 对象编码成没有空格、很密集的一长串字符串。看起来像这样：

```json
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

它不是加密的，所以任何人都可以从内容中恢复信息。

但它是“签名”的。因此，当你收到一个自己签发的令牌时，你可以验证它确实是你签发的。

这样你就可以创建一个例如有效期为 1 周的令牌。然后当用户第二天带着这个令牌回来时，你能知道该用户仍然处于登录状态。

一周后令牌过期，用户将不再被授权，需要重新登录以获取新令牌。而如果用户（或第三方）尝试修改令牌来更改过期时间，你也能发现，因为签名将不匹配。

如果你想动手体验 JWT 令牌并了解它的工作方式，请访问 [https://jwt.io](https://jwt.io/)。

### 安装 `PyJWT`

我们需要安装 `PyJWT`，以便在 Python 中生成和校验 JWT 令牌。

请确保创建并激活一个[虚拟环境](https://fastapi.tiangolo.com/zh/virtual-environments/)，然后安装 `pyjwt`：

### 密码哈希

“哈希”是指把一些内容（这里是密码）转换成看起来像乱码的一串字节（其实就是字符串）。

当你每次传入完全相同的内容（完全相同的密码）时，都会得到完全相同的“乱码”。

但你无法从这个“乱码”反向还原出密码。

#### 为什么使用密码哈希

如果你的数据库被盗，窃贼拿到的不会是用户的明文密码，而只是哈希值。

因此，窃贼无法把该密码拿去尝试登录另一个系统（很多用户在各处都用相同的密码，这将非常危险）。

### 安装 `pwdlib`

pwdlib 是一个用于处理密码哈希的优秀 Python 包。

它支持多种安全的哈希算法以及相关工具。

推荐的算法是 “Argon2”。

请确保创建并激活一个[虚拟环境](https://fastapi.tiangolo.com/zh/virtual-environments/)，然后安装带 Argon2 的 pwdlib：



> 🔥 提示
>
> 使用 `pwdlib`，你甚至可以把它配置为能够读取由 **Django**、**Flask** 安全插件或其他许多工具创建的密码。
>
> 例如，你可以在数据库中让一个 Django 应用和一个 FastAPI 应用共享同一份数据。或者在使用同一个数据库的前提下，逐步迁移一个 Django 应用到 FastAPI。
>
> 同时，你的用户既可以从 Django 应用登录，也可以从 **FastAPI** 应用登录。



### 哈希并校验密码

从 `pwdlib` 导入所需工具。

用推荐设置创建一个 PasswordHash 实例——它将用于哈希与校验密码。

创建一个工具函数来哈希用户传入的密码。

再创建一个工具函数来校验接收的密码是否匹配已存储的哈希。

再创建一个工具函数来进行身份验证并返回用户。

```python
from datetime import datetime, timedelta, timezone
from typing import Annotated

import jwt
from fastapi import Depends, FastAPI, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from jwt.exceptions import InvalidTokenError
from pwdlib import PasswordHash
from pydantic import BaseModel

# to get a string like this run:
# openssl rand -hex 32
SECRET_KEY = "09d25e094faa6ca2556c818166b7a9563b93f7099f6f0f4caa6cf63b88e8d3e7"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30


fake_users_db = {
    "johndoe": {
        "username": "johndoe",
        "full_name": "John Doe",
        "email": "johndoe@example.com",
        "hashed_password": "$argon2id$v=19$m=65536,t=3,p=4$wagCPXjifgvUFBzq4hqe3w$CYaIb8sB+wtD+Vu/P4uod1+Qof8h+1g7bbDlBID48Rc",
        "disabled": False,
    }
}


class Token(BaseModel):
    access_token: str
    token_type: str


class TokenData(BaseModel):
    username: str | None = None


class User(BaseModel):
    username: str
    email: str | None = None
    full_name: str | None = None
    disabled: bool | None = None


class UserInDB(User):
    hashed_password: str


password_hash = PasswordHash.recommended()

DUMMY_HASH = password_hash.hash("dummypassword")

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

app = FastAPI()


def verify_password(plain_password, hashed_password):
    return password_hash.verify(plain_password, hashed_password)


def get_password_hash(password):
    return password_hash.hash(password)


def get_user(db, username: str):
    if username in db:
        user_dict = db[username]
        return UserInDB(**user_dict)


def authenticate_user(fake_db, username: str, password: str):
    user = get_user(fake_db, username)
    if not user:
        verify_password(password, DUMMY_HASH)
        return False
    if not verify_password(password, user.hashed_password):
        return False
    return user


def create_access_token(data: dict, expires_delta: timedelta | None = None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.now(timezone.utc) + expires_delta
    else:
        expire = datetime.now(timezone.utc) + timedelta(minutes=15)
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt


async def get_current_user(token: Annotated[str, Depends(oauth2_scheme)]):
    credentials_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Could not validate credentials",
        headers={"WWW-Authenticate": "Bearer"},
    )
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username = payload.get("sub")
        if username is None:
            raise credentials_exception
        token_data = TokenData(username=username)
    except InvalidTokenError:
        raise credentials_exception
    user = get_user(fake_users_db, username=token_data.username)
    if user is None:
        raise credentials_exception
    return user


async def get_current_active_user(
    current_user: Annotated[User, Depends(get_current_user)],
):
    if current_user.disabled:
        raise HTTPException(status_code=400, detail="Inactive user")
    return current_user


@app.post("/token")
async def login_for_access_token(
    form_data: Annotated[OAuth2PasswordRequestForm, Depends()],
) -> Token:
    user = authenticate_user(fake_users_db, form_data.username, form_data.password)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Incorrect username or password",
            headers={"WWW-Authenticate": "Bearer"},
        )
    access_token_expires = timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    access_token = create_access_token(
        data={"sub": user.username}, expires_delta=access_token_expires
    )
    return Token(access_token=access_token, token_type="bearer")


@app.get("/users/me/")
async def read_users_me(
    current_user: Annotated[User, Depends(get_current_active_user)],
) -> User:
    return current_user


@app.get("/users/me/items/")
async def read_own_items(
    current_user: Annotated[User, Depends(get_current_active_user)],
):
    return [{"item_id": "Foo", "owner": current_user.username}]
```

当使用一个在数据库中不存在的用户名调用 `authenticate_user` 时，我们仍然会针对一个虚拟哈希运行 `verify_password`。

这可以确保无论用户名是否有效，端点的响应时间大致相同，从而防止可用于枚举已存在用户名的“时间攻击”（timing attacks）。





### 处理 JWT 令牌

导入已安装的模块。

创建一个用于对 JWT 令牌进行签名的随机密钥。

使用下列命令生成一个安全的随机密钥：

```bash
openssl rand -hex 32
```



把输出复制到变量 `SECRET_KEY`（不要使用示例中的那个）。

创建变量 `ALGORITHM`，设置用于签名 JWT 令牌的算法，这里设为 `"HS256"`。

创建一个变量用于设置令牌的过期时间。

定义一个用于令牌端点响应的 Pydantic 模型。

创建一个生成新访问令牌的工具函数。

```python
SECRET_KEY = "09d25e094faa6ca2556c818166b7a9563b93f7099f6f0f4caa6cf63b88e8d3e7"
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30



class Token(BaseModel):
    access_token: str
    token_type: str
      
      
      
      
def create_access_token(data: dict, expires_delta: timedelta | None = None):
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.now(timezone.utc) + expires_delta
    else:
        expire = datetime.now(timezone.utc) + timedelta(minutes=15)
    to_encode.update({"exp": expire})
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt

```

### 更新依赖项

更新 `get_current_user` 以接收与之前相同的令牌，但这次使用的是 JWT 令牌。

解码接收到的令牌，进行校验，并返回当前用户。

如果令牌无效，立即返回一个 HTTP 错误。

```python
async def get_current_user(token: Annotated[str, Depends(oauth2_scheme)]):
    credentials_exception = HTTPException(
        status_code=status.HTTP_401_UNAUTHORIZED,
        detail="Could not validate credentials",
        headers={"WWW-Authenticate": "Bearer"},
    )
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username = payload.get("sub")
        if username is None:
            raise credentials_exception
        token_data = TokenData(username=username)
    except InvalidTokenError:
        raise credentials_exception
    user = get_user(fake_users_db, username=token_data.username)
    if user is None:
        raise credentials_exception
    return user
```





### 更新 `/token` 路径操作

用令牌的过期时间创建一个 `timedelta`。

创建一个真正的 JWT 访问令牌并返回它。

```python
@app.post("/token")
async def login_for_access_token(
    form_data: Annotated[OAuth2PasswordRequestForm, Depends()],
) -> Token:
    user = authenticate_user(fake_users_db, form_data.username, form_data.password)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Incorrect username or password",
            headers={"WWW-Authenticate": "Bearer"},
        )
    access_token_expires = timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)
    access_token = create_access_token(
        data={"sub": user.username}, expires_delta=access_token_expires
    )
    return Token(access_token=access_token, token_type="bearer")
```



#### 关于 JWT “主题” `sub` 的技术细节[JWT 规范中有一个 `sub` 键，表示令牌的“主题”（subject）。

使用它是可选的，但通常会把用户的标识放在这里，所以本例中我们使用它。

JWT 除了用于识别用户并允许其直接在你的 API 上执行操作之外，还可能用于其他场景。

例如，你可以用它来标识一辆“车”或一篇“博客文章”。

然后你可以为该实体添加权限，比如“drive”（用于车）或“edit”（用于博客）。

接着，你可以把这个 JWT 令牌交给一个用户（或机器人），他们就可以在没有账户的前提下，仅凭你的 API 生成的 JWT 令牌来执行这些操作（开车、编辑文章）。

基于这些想法，JWT 可以用于更复杂的场景。

在这些情况下，多个实体可能会有相同的 ID，比如都叫 `foo`（用户 `foo`、车 `foo`、博客文章 `foo`）。

因此，为了避免 ID 冲突，在为用户创建 JWT 令牌时，你可以给 `sub` 键的值加一个前缀，例如 `username:`。所以在这个例子中，`sub` 的值可以是：`username:johndoe`。

需要牢记的一点是，`sub` 键在整个应用中应该是一个唯一标识符，并且它应该是字符串。



### 检查

运行服务器并打开文档：<http://127.0.0.1:8000/docs>。

你会看到这样的用户界面：

![截屏2026-06-16 12.36.53](https://raw.githubusercontent.com/Otrname/my---notes/main/%E7%AC%94%E8%AE%B0/images/%E6%88%AA%E5%B1%8F2026-06-16%2012.36.53.png)

像之前一样进行授权。

使用以下凭证：

用户名: `johndoe` 密码: `secret`

调用 `/users/me/` 端点，你将得到如下响应：

```json
{
  "username": "johndoe",
  "email": "johndoe@example.com",
  "full_name": "John Doe",
  "disabled": false
}
```

![截屏2026-06-16 12.39.48](images/截屏2026-06-16 12.39.48.png)

如果你打开开发者工具，你会看到发送的数据只包含令牌。密码只会在第一个请求中用于认证用户并获取访问令牌，之后就不会再发送密码了：

![截屏2026-06-16 12.48.17](images/截屏2026-06-16 12.48.17.png)

### 使用 `scopes` 的高级用法

OAuth2 支持 “scopes”（作用域）。

你可以用它们为 JWT 令牌添加一组特定的权限。

然后你可以把这个令牌直接交给用户或第三方，在一组限制条件下与 API 交互。

在**高级用户指南**中你将学习如何使用它们，以及它们如何集成进 **FastAPI**。

### 小结

通过目前所学内容，你可以使用 OAuth2 和 JWT 等标准来搭建一个安全的 **FastAPI** 应用。

在几乎任何框架中，处理安全问题都会很快变得相当复杂。

许多把安全流程大幅简化的包，往往要在数据模型、数据库和可用特性上做出大量妥协。而有些过度简化的包实际上在底层存在安全隐患。

------

**FastAPI** 不会在任何数据库、数据模型或工具上做妥协。

它给予你完全的灵活性，选择最适合你项目的方案。

而且你可以直接使用许多维护良好、广泛使用的包，比如 `pwdlib` 和 `PyJWT`，因为 **FastAPI** 不需要复杂机制来集成外部包。

同时它也为你提供尽可能简化流程的工具，而不牺牲灵活性、健壮性或安全性。

你可以以相对简单的方式使用和实现像 OAuth2 这样的安全、标准协议。

在**高级用户指南**中，你可以进一步了解如何使用 OAuth2 的 “scopes”，以遵循相同标准实现更细粒度的权限系统。带作用域的 OAuth2 是许多大型身份认证提供商（如 Facebook、Google、GitHub、Microsoft、X（Twitter）等）用来授权第三方应用代表其用户与其 API 交互的机制。



------

----

## 中间件

你可以向 **FastAPI** 应用添加中间件。

“中间件”是一个函数，它会在每个特定的*路径操作*处理每个**请求**之前运行，也会在返回每个**响应**之前运行。

- 它接收你的应用的每一个**请求**。

- 然后它可以对这个**请求**做一些事情或者执行任何需要的代码。

- 然后它将这个**请求**传递给应用程序的其他部分（某个*路径操作*）处理。

- 之后它获取应用程序生成的**响应**（由某个*路径操作*产生）。

- 它可以对该**响应**做一些事情或者执行任何需要的代码。

- 然后它返回这个**响应**。

### 创建中间件

  要创建中间件，你可以在函数的顶部使用装饰器 `@app.middleware("http")`。

  中间件函数会接收：

  - `request`。

  - 一个函数`call_next`，它会把`request`作为参数接收。
    - 这个函数会把 `request` 传递给相应的*路径操作*。
    - 然后它返回由相应*路径操作*生成的 `response`。
  - 在返回之前，你可以进一步修改 `response`。
```python
import time

from fastapi import FastAPI, Request

app = FastAPI()


@app.middleware("http")
async def add_process_time_header(request: Request, call_next):
    start_time = time.perf_counter()
    response = await call_next(request)
    process_time = time.perf_counter() - start_time
    response.headers["X-Process-Time"] = str(process_time)
    return response
```



> 🔥 提示
>
> 记住可以[使用 `X-` 前缀](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)添加专有自定义请求头。
>
> 但是如果你有希望让浏览器中的客户端可见的自定义请求头，你需要把它们加到你的 CORS 配置（[CORS（跨域资源共享）](https://fastapi.tiangolo.com/zh/tutorial/cors/)）的 `expose_headers` 参数中，参见 [Starlette 的 CORS 文档](https://www.starlette.dev/middleware/#corsmiddleware)。





#### 在 `response` 之前与之后

你可以在任何*路径操作*接收 `request` 之前，添加要与该 `request` 一起运行的代码。

也可以在生成 `response` 之后、返回之前添加代码。

例如，你可以添加一个自定义请求头 `X-Process-Time`，其值为处理请求并生成响应所花费的秒数：

```python
 start_time = time.perf_counter()
 process_time = time.perf_counter() - start_time
 response.headers["X-Process-Time"] = str(process_time)
```

### 多个中间件的执行顺序

当你使用 `@app.middleware()` 装饰器或 `app.add_middleware()` 方法添加多个中间件时，每个新中间件都会包裹应用，形成一个栈。最后添加的中间件是“最外层”的，最先添加的是“最内层”的。

在请求路径上，最外层的中间件先运行。

在响应路径上，它最后运行。

例如：

```python
app.add_middleware(MiddlewareA)
app.add_middleware(MiddlewareB)
```

这会产生如下执行顺序：

- 请求：MiddlewareB → MiddlewareA → 路由
- 响应：路由 → MiddlewareA → MiddlewareB

这种栈式行为确保中间件按可预测且可控的顺序执行。

### 其他中间件

你可以稍后在[高级用户指南：高级中间件](https://fastapi.tiangolo.com/zh/advanced/middleware/)中阅读更多关于其他中间件的内容。

你将在下一节中了解如何使用中间件处理 CORS。









---

----

## CORS(跨域资源共享)

[CORS 或者「跨域资源共享」](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) 指浏览器中运行的前端拥有与后端通信的 JavaScript 代码，而后端处于与前端不同的「源」的情况。



### 源

源是协议（`http`，`https`）、域（`myapp.com`，`localhost`，`localhost.tiangolo.com`）以及端口（`80`、`443`、`8080`）的组合。

因此，这些都是不同的源：

- `http://localhost`
- `https://localhost`
- `http://localhost:8080`

即使它们都在 `localhost` 中，但是它们使用不同的协议或者端口，所以它们都是不同的「源」。



### 步骤

假设你的浏览器中有一个前端运行在 `http://localhost:8080`，并且它的 JavaScript 正在尝试与运行在 `http://localhost` 的后端通信（因为我们没有指定端口，浏览器会采用默认的端口 `80`）。

然后，浏览器会向 `:80` 的后端发送一个 HTTP `OPTIONS` 请求，如果后端发送适当的 headers 来授权来自这个不同源（`http://localhost:8080`）的通信，那么运行在 `:8080` 的浏览器就会允许前端中的 JavaScript 向 `:80` 的后端发送请求。

为此，`:80` 的后端必须有一个「允许的源」列表。

在这种情况下，它必须包含 `http://localhost:8080`，这样 `:8080` 的前端才能正常工作。



### 通配符

也可以使用 `"*"`（一个「通配符」）声明这个列表，表示全部都是允许的。

但这仅允许某些类型的通信，不包括所有涉及凭据的内容：比如 Cookies，以及那些使用 Bearer 令牌的 Authorization 请求头等。

因此，为了一切都能正常工作，最好显式地指定允许的源。



### 使用 `CORSMiddleware`

你可以在 **FastAPI** 应用中使用 `CORSMiddleware` 来配置它。

- 导入 `CORSMiddleware`。
- 创建一个允许的源列表（由字符串组成）。
- 将其作为「中间件」添加到你的 **FastAPI** 应用中。

你也可以指定后端是否允许：

- 凭证（Authorization 请求头、Cookies 等）。
- 特定的 HTTP 方法（`POST`，`PUT`）或者使用通配符 `"*"` 允许所有方法。
- 特定的 HTTP 请求头或者使用通配符 `"*"` 允许所有请求头。

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

origins = [
    "http://localhost.tiangolo.com",
    "https://localhost.tiangolo.com",
    "http://localhost",
    "http://localhost:8080",
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)


@app.get("/")
async def main():
    return {"message": "Hello World"}
```



默认情况下，这个 `CORSMiddleware` 实现所使用的默认参数较为保守，所以你需要显式地启用特定的源、方法或者 headers，以便浏览器能够在跨域上下文中使用它们。

支持以下参数：

- `allow_origins` - 一个允许跨域请求的源列表。例如 `['https://example.org', 'https://www.example.org']`。你可以使用 `['*']` 允许任何源。

- `allow_origin_regex` - 一个正则表达式字符串，匹配的源允许跨域请求。例如 `'https://.*\.example\.org'`。

- `allow_methods` - 一个允许跨域请求的 HTTP 方法列表。默认为 `['GET']`。你可以使用 `['*']` 来允许所有标准方法。

- `allow_headers` - 一个允许跨域请求的 HTTP 请求头列表。默认为 `[]`。你可以使用 `['*']` 允许所有的请求头。`Accept`、`Accept-Language`、`Content-Language` 以及 `Content-Type` 这几个请求头在[简单 CORS 请求](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#simple_requests)中总是被允许。

- `allow_credentials` - 指示跨域请求支持 cookies。默认是 `False`。

  当 `allow_credentials` 设为 `True` 时，`allow_origins`、`allow_methods` 和 `allow_headers` 都不能设为 `['*']`。它们必须[显式指定](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#credentialed_requests_and_wildcards)。

- `expose_headers` - 指示可以被浏览器访问的响应头。默认为 `[]`。

- `max_age` - 设定浏览器缓存 CORS 响应的最长时间，单位是秒。默认为 `600`。

中间件响应两种特定类型的 HTTP 请求...

#### CORS 预检请求

这是些带有 `Origin` 和 `Access-Control-Request-Method` 请求头的 `OPTIONS` 请求。

在这种情况下，中间件将拦截传入的请求并进行响应，出于提供信息的目的返回一个使用了适当的 CORS headers 的 `200` 或 `400` 响应。

#### 简单请求

任何带有 `Origin` 请求头的请求。在这种情况下，中间件将像平常一样传递请求，但是在响应中包含适当的 CORS headers。

### 更多信息

更多关于 CORS 的信息，请查看 [Mozilla CORS 文档](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。



---

---

## SQL (关系型 )数据库
