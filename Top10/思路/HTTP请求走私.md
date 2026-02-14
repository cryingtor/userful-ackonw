# HTTP请求走私
CL.TE：前端服务器用Content-Length头，后端服务器用Transfer-Encoding头。
TE.CL：前端服务器用Transfer-Encoding头，后端服务器用Content-Length头。
TE.TE：前端和后端服务器都支持该Transfer-Encoding标头，但可以通过某种方式混淆标头来诱导其中一个服务器不处理它。 