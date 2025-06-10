# SSTI注入
- current_app,这是全局变量代理
```
{{url_for.__globals__['current_app'].config}}
```

