## 创建程序实例
`app = Flask(__name__)`

## 路由 
### 将函数绑定到 URL: 
使用 `@app.route(URL)` 装饰函数, 可以将函数绑定到 URL, 参数是需要绑定的 URL.
### 定制错误页面
使用 `@app.errorhandler(err_code)` 可以将函数绑定到对应的错误页面, 参数是错误代码.
### 在 URL 中添加变量: 
- 将 URL 的一部分标记为 `<variable_name>` 或者 `<converter:variable_name>`  
- 转换器类型包括: `string, path, int, float, uuid` (`path`可以包含斜杠, 但是`string`不可以)  
- 由此得到的变量可以以函数参数的形式传入参数中, 参数的名称和`<variable_name>`中的名称相同  
### HTTP 方法: 
- `route` 装饰器接受 `methods` 参数以处理不同的 HTTP 方法.  
- `methods` 参数接受一个列表, 列表中值为 `"POST"` 和/或 `"GET"`. (默认为只有 `"GET"`).  
- 在函数中, 可以使用 `request.method` 获取 HTTP 方法.看看


## URL 构建 
### URL 构建 
- 使用 `url_for()` 函数可以用于构建函数 URL
- 函数第一个参数为函数名称, 并接受若干关键字参数作为 URL 中的变量 (未知变量将作为查询参数)
### 使用静态文件
- 将静态文件放到包中的 `static` 文件夹中.
- 在 `url_for()` 中使用 `static` 端点就可以生成相应的 URL,
  如: `url_for('static', filename='style.css')`, 结果为 `static/style.css`.


## 渲染模板
- 使用 `render_template()` 可以渲染模板, 函数需要模板文件名称和相关模板中出现的参数作为参数.
- 模板需要放在 `templates` 文件夹中.


## 测试程序
- 使用 `test_request_context` 可以对程序进行测试, 如:
```Python
from flask import request
with app.test_request_context('/hello', method='POST'):
    # Now you can do something with the request until the
    # end of the with block, such as basic assertions:
    assert request.path == '/hello'
    assert request.method == 'POST'
```


## 处理请求
### 当前请求方法
使用 `request` 的 `method` 属性可以获取请求方法 (其值为 `POST` 或 `GET`)  
如: `if request.method == 'POST': pass`  
### 表单数据
使用 `request` 的 `form` 属性可以获取表单数据 (是一个字典)  
如: `username = request.form['username']`  
### 获取 URL 中的查询
使用 `request` 的 `args` 属性 (使用 `get()` 获取数据以防止出现 400 页面)  
如: `search_word = request.args.get('search', '')`  
### 文件上传
如果需要浏览器上传文件, 需要在 HTML 表单中设置 `enctype="multipart/form-data"`.    
使用 `request` 的 `files` 属性可以获取文件 (这个属性仍是一个字典),  
获取到的文件是一个 Python file, 额外多出了一个用于保存的 `save()` 方法.  
如果需要知道其在用户客户端系统中的名称, 可以使用 `filename` 属性;
如果要将这个文件名作为在服务器上存储的文件名, 可以使用 `secure_filename()` 函数.
如: `fp = request.files['file']; fp.save('files/' + secure_filename(fp.filename))`


## 重定向
使用 `redirect()` 函数可以进行重定向, 如: `return redirect(url_for('login'))`
使用 `abort()` 可以退出请求并返回错误代码, 如: `abort(401)`
