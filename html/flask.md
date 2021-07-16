# Flask使用遇到的问题  
### 1、flask_wtf表单验证始终不通过  
注意：flask-wtf模块是携带csrf校验的，只是需要开启。在flask form中默认csrf_token 是没有开启的，需要我们手动去启动form表单的csrf_token。  
以注册表单为例：<form>后加入{{ form.csrf_token}}
```html
<form class="m-t" role="form" action="" method="post" enctype="multipart/form-data">
    {{ form.csrf_token}}
     <div class="form-group">
        <input type="text" class="form-control" placeholder="请输入用户名" required="", name="username">
    </div>
    <div class="form-group">
        <input type="password" class="form-control" placeholder="请输入密码" required="" name="password">
    </div>
    <div class="form-group">
        <input type="password" class="form-control" placeholder="请再次输入密码" required="" name="password_confirm">
    </div>
    <div class="form-group">
        <input type="email" class="form-control" placeholder="请输入邮箱" required="" name="email">
    </div>
    <div class="form-group text-left">
        <div class="checkbox i-checks">
            <label class="no-padding">
                <input type="checkbox"><i></i> 我同意注册协议</label>
        </div>
    </div>
    <button type="submit" class="btn btn-primary block full-width m-b">注 册</button>
    <p class="text-muted text-center"><small>已经有账户了？</small><a href="login.html">点此登录</a></p>
</form>
```  

### 2、ModuleNotFoundError: No module named 'flask._compat'  
问题原因：flask版本问题    
解决办法：安装低版本flask  
pip uninstall flask  
pip install flask==1.1.1  

### 3、ModuleNotFoundError: No module named 'ConfigParser'  

是因为引用错了包 from blueprint import Blueprint  
应该引用 from flask import Blueprint  

### 4、Flask注册路由的两种方式  
1、装饰器
@app.route('/')
2、add_url_rule
app.add_url_rule('/', view_func=hello) # 这里的hello是一个视图函数